---
title: 'Rate Limiting Algorithms'
date: 2026-09-24
permalink: /posts/2026/09/rate-limiting-algorithms/
tags:
  - rate-limiting
  - system-design
  - algorithms
  - redis
  - api
  - distributed-systems
---

Think of a nightclub with a bouncer at the door. The club can safely hold 200 people. If 1,000 show up at once and everyone is let in, it gets unsafe. The bouncer controls how fast people enter: enough to keep the place busy, but never more than it can handle. That's exactly what **rate limiting** does for a server.

Rate limiting answers one simple question: **"how many requests is this client allowed to make in a given amount of time?"** Anything beyond that is rejected (or delayed).

## Why Rate Limiting Exists

Without it, a single client can take down your whole API. If one client sends 1,000 requests/second, your server runs out of memory, CPU, or database connections and everyone suffers.

Rate limiting is used to:

1. **Protect the server from overload** — keep traffic within capacity.
2. **Enforce usage tiers** — free plan = 100 requests/day, paid = 10,000/day.
3. **Prevent abuse** — brute-force logins, scraping, DoS attacks.
4. **Share resources fairly** — one noisy client shouldn't starve the rest.

## The Mental Model

Every rate limiting algorithm answers two questions:

1. **How do I track usage?** (a counter, a set of timestamps, or tokens)
2. **What do I do when the limit is hit?** (reject, queue, or throttle)

The algorithms differ in four ways:

- **Memory cost** — O(1) vs O(N) per client.
- **Burst behavior** — do they allow short spikes or produce a smooth output?
- **Boundary correctness** — what happens at the edges between time windows?
- **Implementation complexity** — how hard are they to build and reason about?

With that framing, the five common algorithms become easy to compare.

---

## Algorithm 1 — Token Bucket

### Mechanism

A bucket holds tokens, up to a maximum `capacity`. It refills at `refill_rate` tokens per second. Every request must take **1 token**; if the bucket is empty, the request is rejected.

```text
   ┌──────────────┐
   │ ●●●●●●       │  ← bucket (capacity=10, currently 6 tokens)
   │              │
   └──────┬───────┘
          │ refills at 5 tokens/sec
          ▼
   each request takes 1 token
```

The bucket starts full, so a client can immediately send a **burst** of up to `capacity` requests, then settles into the steady `refill_rate`.

### Walkthrough — capacity=10, refill=5/sec

| Time | Event | Bucket | Result |
|---|---|---|---|
| 0.0s | Start | 10 | — |
| 0.0s | 10 requests arrive instantly | 0 | All 10 pass (burst absorbed) |
| 0.5s | Refilled by 0.5 × 5 = 2.5 | 2.5 | — |
| 0.5s | 3 requests arrive | 0.5 | First 2 pass, 3rd rejected |
| 1.0s | Refilled by 0.5 × 5 = 2.5 | 3.0 | — |
| 1.0s | 1 request | 2.0 | Pass |
| 5.0s | Idle 4 sec → refill by 4 × 5 = 20, capped at 10 | 10 | Bucket full again |

### Properties

- **Allows bursts** up to `capacity` (its defining feature).
- **Smooths over time** to `refill_rate`.
- **O(1) memory** per client.
- **Lazy refill** — no background thread; tokens are calculated on each request.

### When to Use

- Most APIs (the sensible default choice).
- When you want to allow controlled bursts (mobile app syncs, batch jobs).
- When you need a predictable steady-state rate.

### When NOT to Use

- When output must be perfectly smooth (use Leaky Bucket).
- In a distributed system without shared state (use Sliding Window Counter in Redis).

### Real World

- **AWS API Gateway** — token bucket per API key.
- **Stripe API** — token bucket per account.
- **Linux `tc` (traffic control)** — token bucket for network shaping.

### Code

```python
import time
from threading import Lock

class TokenBucket:
    def __init__(self, capacity: int, refill_rate: float):
        self.capacity = capacity
        self.refill_rate = refill_rate
        self.tokens = float(capacity)        # start full
        self.last_refill = time.time()
        self.lock = Lock()

    def allow(self) -> bool:
        with self.lock:
            now = time.time()
            elapsed = now - self.last_refill
            self.tokens = min(self.capacity, self.tokens + elapsed * self.refill_rate)
            self.last_refill = now
            if self.tokens >= 1:
                self.tokens -= 1
                return True
            return False
```

---

## Algorithm 2 — Leaky Bucket

### Mechanism

A bucket of fixed capacity. Requests **fill** it. Requests **leak out** (get processed) at a constant rate. If the bucket is full and a request arrives, it overflows and is rejected.

```text
   incoming requests (bursty)
          ▼ ▼ ▼ ▼
   ┌──────────────┐
   │ ●●●●         │  ← bucket
   │              │
   └──────┬───────┘
          │ leaks at constant rate (e.g., 5/sec)
          ▼
   processed requests (smooth)
```

Two ways to implement it:

1. **Queue-based** — requests wait in a queue and are processed at a constant rate.
2. **Counter-based** — track a "water level" and decrease it at a constant rate.

### Walkthrough — capacity=10, leak=5/sec

| Time | Event | Bucket | Result |
| --- | --- | --- | --- |
| 0.0s | Start | 0 | — |
| 0.0s | 10 requests arrive | 10 | All accepted (queued) |
| 0.0s | Output starts processing 5/sec | — | — |
| 0.5s | 2.5 leaked, 5 more arrive | reject 2.5 worth | 2–3 rejected (overflow) |
| 1.0s | 2.5 more leaked | 7.5 | — |
| 2.0s | 5 more leaked | 2.5 | Idle |
| 2.0s | 100 requests arrive | 10 (cap) | First 7–8 accepted, rest rejected |

### Properties

- **Smooth output** — exactly `leak_rate` requests/sec are processed.
- **No bursts at output** — input can burst, but requests queue up.
- **O(1) memory** per client (or O(queue_size) for the queue implementation).
- Adds **latency**, because requests wait in the queue.

### Token vs Leaky — The Key Difference

| | Token Bucket | Leaky Bucket |
|---|---|---|
| **Input bursty?** | Allowed | Allowed |
| **Output bursty?** | **Yes** (up to capacity) | **No** (smooth) |
| **Adds latency?** | No (immediate accept/reject) | Yes (queue wait) |
| **Typical use** | API rate limiting | Network shaping |

### When to Use

- Network traffic shaping (smoothing bursts before sending).
- When the downstream **must** receive requests at a constant rate (e.g., a slow database).
- Telecom / call processing.

### When NOT to Use

- API rate limiting where you want immediate accept/reject.
- Real-time systems where queue latency hurts.

### Real World

- **Network routers** (TCP traffic shaping).
- **Telecom switches**.

### Code

```python
import time
from threading import Lock

class LeakyBucket:
    def __init__(self, capacity: int, leak_rate: float):
        self.capacity = capacity
        self.leak_rate = leak_rate
        self.water = 0.0
        self.last_leak = time.time()
        self.lock = Lock()

    def allow(self) -> bool:
        with self.lock:
            now = time.time()
            elapsed = now - self.last_leak
            self.water = max(0.0, self.water - elapsed * self.leak_rate)
            self.last_leak = now
            if self.water + 1 <= self.capacity:
                self.water += 1
                return True
            return False
```

---

## Algorithm 3 — Fixed Window Counter

### Mechanism

Time is split into fixed windows (e.g., one minute each). Each window has a counter. On every request, increment the counter; reject if it exceeds the limit. The counter resets at each window boundary.

```text
12:00:00 ─── 12:00:59     12:01:00 ─── 12:01:59
[count = 87 / 100]        [count = 0 / 100]
```

### Walkthrough — limit=100/min

| Time | Event | Window | Count | Result |
|---|---|---|---|---|
| 12:00:30 | Request | 12:00:00–59 | 1 | Pass |
| 12:00:45 | 99 requests | 12:00:00–59 | 100 | All pass |
| 12:00:50 | Request | 12:00:00–59 | 101 | **Reject** |
| 12:01:00 | New window starts | 12:01:00–59 | 0 | — |
| 12:01:01 | Request | 12:01:00–59 | 1 | Pass |

### The Boundary Bug (the critical flaw)

```text
Time:    12:00:59          12:01:00
Window:  [00:00 - 00:59]   [01:00 - 01:59]
         100 requests       100 requests
                ↓                ↓
       In just 2 seconds: 200 requests served
       But the limit is "100/min" — this violates the limit
```

**The problem:** the limit is "100 per minute", but a client can send **200 requests in a 2-second span** by clustering requests around a window boundary. Two windows each allow 100, so the instant between them effectively allows double.

### Properties

- **O(1) memory** per client (one counter).
- **Trivial to implement.**
- **Broken at boundaries** — allows a 2× burst at the edges.
- No smoothing.

### When to Use

- Quick prototypes.
- When approximate limits are acceptable.
- Coarse limits (e.g., per-day quotas where a 2× burst doesn't matter).

### When NOT to Use

- Strict per-second or per-minute guarantees.
- Anywhere precision matters.

### Code

```python
import time
from threading import Lock

class FixedWindow:
    def __init__(self, limit: int, window_size: int):
        self.limit = limit
        self.window_size = window_size       # seconds
        self.count = 0
        self.window_start = int(time.time()) // window_size
        self.lock = Lock()

    def allow(self) -> bool:
        with self.lock:
            current_window = int(time.time()) // self.window_size
            if current_window > self.window_start:
                self.window_start = current_window
                self.count = 0
            if self.count < self.limit:
                self.count += 1
                return True
            return False
```

---

## Algorithm 4 — Sliding Window Log

### Mechanism

Store the timestamp of every request in a list (the "log"). On a new request:

1. Drop timestamps older than the window.
2. Count what's left.
3. If the count is below the limit, allow it and log the new timestamp.

```text
Now = 12:00:30, window = 60s
Log: [12:00:05, 12:00:12, 12:00:20, 12:00:25, 12:00:28]
                    ↑ all within the last 60s, count = 5
```

### Walkthrough — limit=5/min

| Time | Action | Log (after) | Result |
|---|---|---|---|
| 12:00:00 | Req | [12:00:00] | Pass (1/5) |
| 12:00:10 | Req | [12:00:00, 12:00:10] | Pass (2/5) |
| 12:00:20 | Req | [3 entries] | Pass (3/5) |
| 12:00:30 | Req | [4 entries] | Pass (4/5) |
| 12:00:40 | Req | [5 entries] | Pass (5/5) |
| 12:00:45 | Req | (drop nothing, count=6) | **Reject** |
| 12:01:01 | Req | drop 12:00:00, then add | Pass (5/5 again) |

### Properties

- **No boundary bug** — always exactly "the last N seconds".
- **O(N) memory** per client (N = requests in the window).
- **Strict accuracy** — no approximation.
- **Expensive** at high throughput (the log grows large).

### When to Use

- Strict precision is required (financial APIs, security).
- Low to moderate request volume.

### When NOT to Use

- High throughput (memory grows quickly).
- Distributed systems (synchronizing logs is expensive).

### Code

```python
import time
from collections import deque
from threading import Lock

class SlidingWindowLog:
    def __init__(self, limit: int, window_size: float):
        self.limit = limit
        self.window_size = window_size
        self.log = deque()
        self.lock = Lock()

    def allow(self) -> bool:
        with self.lock:
            now = time.time()
            # Drop old timestamps
            while self.log and self.log[0] <= now - self.window_size:
                self.log.popleft()
            if len(self.log) < self.limit:
                self.log.append(now)
                return True
            return False
```

---

## Algorithm 5 — Sliding Window Counter (Hybrid)

### Mechanism

This combines Fixed Window's cheapness with Sliding Window's correctness. Track the count for the **current window** and the **previous window**, then estimate the current rate with a weighted formula:

```text
weight_of_previous = 1 - (elapsed_in_current_window / window_size)
estimated_count    = current_count + previous_count × weight_of_previous
```

If `estimated_count < limit`, allow the request.

### Walkthrough — limit=100/min, window=60s

```text
Previous window [12:00 - 12:59]: count = 80
Current window  [13:00 - 13:59]: started at 13:00:00

At 13:00:30 (50% into the current window):
  weight_of_previous = 1 - 30/60 = 0.5
  estimated = current_count + 80 × 0.5
            = current_count + 40

  If current_count = 50:
    estimated = 50 + 40 = 90  → ALLOW
  If current_count = 65:
    estimated = 65 + 40 = 105 → REJECT
```

The closer you are to the start of the current window, the more "weight" the previous window carries — which smooths the boundary.

### Properties

- **O(1) memory** per client (just two counters).
- **No boundary bug** (smooth handoff between windows).
- **Approximate** (it assumes requests were spread evenly across the previous window).
- Most production systems use this.

### When to Use

- High-throughput APIs that need accuracy.
- The usual production default for distributed rate limiters.
- When the O(N) memory of the Sliding Window Log is too expensive.

### When NOT to Use

- When strict precision is required (use Sliding Window Log).
- Very low traffic (Token Bucket is simpler).

### Real World

- **Cloudflare** — sliding window counter for DDoS protection.
- **Kong API Gateway** — sliding window counter.
- Most rate limiters in Redis-based deployments.

### Code

```python
import time
from threading import Lock

class SlidingWindowCounter:
    def __init__(self, limit: int, window_size: float):
        self.limit = limit
        self.window_size = window_size
        self.current_count = 0
        self.previous_count = 0
        self.window_start = int(time.time() / window_size) * window_size
        self.lock = Lock()

    def allow(self) -> bool:
        with self.lock:
            now = time.time()
            current_window = int(now / self.window_size) * self.window_size

            # Window rolled over
            if current_window > self.window_start:
                if current_window == self.window_start + self.window_size:
                    self.previous_count = self.current_count
                else:
                    self.previous_count = 0  # gap > 1 window
                self.current_count = 0
                self.window_start = current_window

            elapsed_in_current = now - self.window_start
            weight = 1 - (elapsed_in_current / self.window_size)
            estimated = self.current_count + self.previous_count * weight

            if estimated < self.limit:
                self.current_count += 1
                return True
            return False
```

---

## Comparison — All 5 Algorithms

| Algorithm | Memory | Allows Burst? | Smooth Output? | Boundary Bug? | Latency Added? | Complexity |
|---|---|---|---|---|---|---|
| **Token Bucket** | O(1) | Yes (up to cap) | No | No | None | Easy |
| **Leaky Bucket** | O(1) or O(queue) | No | **Yes** | No | **Yes** (queue) | Easy |
| **Fixed Window** | O(1) | Yes (2× at boundary) | No | **Yes** | None | Trivial |
| **Sliding Window Log** | **O(N)** | No | No | No | None | Easy |
| **Sliding Window Counter** | O(1) | Slight | No | No | None | Medium |

### Behavioral Comparison

To compare them at a glance: the client sends **10 requests at t=0** and **10 more at t=1**. The window and log algorithms have a limit of **5 requests/sec**, while the Token Bucket has **capacity 10** and refills at **5/sec** — so it alone can absorb the first burst of 10.

| Algorithm | t=0 (send 10) | t=1 (send 10) | t=2 (send 10) | What it does |
|---|---|---|---|---|
| **Token Bucket** (cap 10, refill 5/s) | 10 pass | 5 pass | 5 pass | absorbs the burst, then stays steady |
| **Leaky Bucket** (leak 5/s) | queued, output 5/s | queued, output 5/s | queued, output 5/s | smooth output, adds latency |
| **Fixed Window** (5/s) | 5 pass | 5 pass (new window) | 5 pass | can serve 10 across the boundary |
| **Sliding Window Log** (5/s) | 5 pass | 0 pass | 5 pass | exactly 5 in any 1-second window |
| **Sliding Window Counter** (5/s) | 5 pass | a few pass | 5 pass | estimates, so the boundary is smooth |

The key contrast: **Token Bucket** allows a burst and then settles; **Leaky Bucket** never bursts (it queues); **Fixed Window** can accidentally allow double at the boundary; **Sliding Window Log** is exact but strict; **Sliding Window Counter** approximates and stays smooth.

### Choosing Guide

```text
Q: Need to allow controlled bursts?
├── Yes → TOKEN BUCKET
└── No  → Q: Need smooth output rate?
           ├── Yes → LEAKY BUCKET
           └── No  → Q: Is O(N) memory acceptable?
                     ├── Yes → SLIDING WINDOW LOG (most accurate)
                     └── No  → Q: Distributed?
                               ├── Yes → SLIDING WINDOW COUNTER (best for Redis)
                               └── No  → SLIDING WINDOW COUNTER or TOKEN BUCKET
```

### Quick Reference

| Scenario | Choose |
|---|---|
| General API rate limiting | **Token Bucket** |
| Distributed across N servers | **Sliding Window Counter (in Redis)** |
| Network traffic shaping | **Leaky Bucket** |
| Strict accuracy, low traffic | **Sliding Window Log** |
| Quick prototype, don't care | **Fixed Window** (but know its boundary bug) |

---

## Implementing for Multiple Clients

Real systems have many clients, so the limiter needs one bucket (or counter) **per client**. The Strategy pattern lets us swap algorithms without changing the caller.

```python
from abc import ABC, abstractmethod

class RateLimitStrategy(ABC):
    @abstractmethod
    def allow(self, client_id: str) -> bool: ...

class TokenBucketStrategy(RateLimitStrategy):
    def __init__(self, capacity: int, refill_rate: float):
        self.capacity = capacity
        self.refill_rate = refill_rate
        self.buckets: dict[str, TokenBucket] = {}
        self.dict_lock = Lock()              # protects bucket creation only

    def allow(self, client_id: str) -> bool:
        with self.dict_lock:
            if client_id not in self.buckets:
                self.buckets[client_id] = TokenBucket(self.capacity, self.refill_rate)
            bucket = self.buckets[client_id]
        return bucket.allow()                # each bucket has its own lock

class RateLimiter:
    def __init__(self, strategy: RateLimitStrategy):
        self.strategy = strategy             # depend on the abstraction, not a concrete class

    def is_allowed(self, client_id: str) -> bool:
        return self.strategy.allow(client_id)
```

**Locking design:**

- The outer lock is used **only** to insert a new client's bucket into the dict.
- Each bucket has its own lock for the actual rate-limit check.
- Two different clients can therefore be checked in parallel with no contention.

The general principle: **don't wrap fine-grained locks inside one coarse lock**, or you lose all the parallelism you were trying to get.

---

## Distributed Rate Limiting

A single server keeps an in-memory dict and is done. With 10 servers behind a load balancer, each server has its own counter — so a client can get **10× the intended rate** (one full quota per server). Here are the common fixes.

### Approach 1 — Sticky Routing

The load balancer pins each client to one server (consistent hashing on `client_id`).

- **Pro:** simple; single-server logic still works.
- **Con:** uneven load; a hot client can overload its assigned server.

### Approach 2 — Centralized Redis

```text
client request → server → Redis: INCR counter:client_id
                               EXPIRE counter:client_id window_size
                               if value > limit → reject
```

- **Pro:** accurate; all servers see the same view.
- **Con:** a network hop per request; Redis becomes a single point of failure.

**Optimization:** put the `INCR` + `EXPIRE` + check in a Redis **Lua script** so the sequence runs atomically.

### Approach 3 — Approximate Per-Server

Each server allows `limit / N` (where N is the number of servers).

- **Pro:** no coordination, no extra latency.
- **Con:** less precise — uneven traffic across servers wastes quota.

### Approach 4 — Sliding Window Counter in Redis

The same Sliding Window Counter algorithm as before, but stored in a Redis hash.

- **Pro:** accurate, cheap memory, and handles boundaries correctly.
- **Con:** a network hop.

This is what most production systems do (Cloudflare, GitHub, and others).

### Putting It Together

> In practice, a distributed setup usually uses a **Sliding Window Counter stored in Redis**, with `INCR` + `EXPIRE` in a Lua script for atomicity, accepting one network hop per request. If latency matters more than precision, approximate per-server with `limit / N`.

---

## Things to Get Right

| Topic | What matters |
|---|---|
| **Algorithm choice** | Token Bucket as the default; Sliding Window Counter when distributed |
| **Why not Fixed Window** | The boundary bug — a 2× burst at the window edges |
| **Pattern** | Strategy (so algorithms can be swapped) + depend on an abstraction |
| **Per-client storage** | A dict keyed by `client_id` |
| **Thread safety** | Per-bucket locks, **not** one global lock |
| **Lazy refill** | Compute on each request; no background thread |
| **Distributed** | Redis with an atomic Lua script; accept the network hop |
| **Edge cases** | Cap the bucket with `min`, and use float tokens for partial refill |

---

## Common Mistakes

1. **A background refill thread** — wasteful; refill lazily on each request.
2. **One global lock** — kills parallelism between independent clients.
3. **Integer-only tokens** — breaks fractional refill; use a float.
4. **Forgetting `min(capacity, ...)`** — the bucket overflows and allows an infinite burst after a long idle period.
5. **Using Fixed Window where precision matters** — remember its boundary bug.
6. **No per-client storage** — a single global counter would limit *all* clients to N req/s combined.
7. **`time.sleep` in tests** — slow and flaky; mock time or use short windows.
8. **Racy lazy bucket creation** — two threads can create two buckets for the same client; fix with `setdefault` or double-checked locking.

---

## Variants Worth Knowing

- **Per-endpoint limits** — different limits for different endpoints. Use `(client_id, endpoint)` as the dict key, or compose several limiters.
- **Tiered limits (free vs paid)** — return different limits based on the user's tier: `client_id → tier → (capacity, rate)`.
- **Burst allowance** — Token Bucket already does this: set `capacity = burst_allowance` and `refill_rate = sustained_rate`.
- **Cost-weighted requests** — some requests are expensive (cost 5 tokens), others cheap (1 token). Change `allow(cost)` to consume `cost` tokens.
- **Concurrency limits (different from rate limits)** — rate limits are requests/second; concurrency limits cap how many requests are *in flight at once*. Use a semaphore for that, not a rate limiter.

---

## Summary

There are five main algorithms. **Token Bucket** is the default — it allows controlled bursts up to the bucket capacity, refills lazily on each request, and uses O(1) memory per client. **Leaky Bucket** smooths the output rate and is used in networking. **Fixed Window** is simple but broken at window boundaries. **Sliding Window Log** is exact but uses O(N) memory. **Sliding Window Counter** is the production favorite: O(1) memory, no boundary bug, and it works well in Redis.

For a single server, wrap the algorithm in a Strategy pattern with per-client locks. For many servers, use a **Sliding Window Counter in Redis** with an atomic Lua script.