---
permalink: /
title: ""
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

👋 Hey! I’m **Tanay Makharia**, a backend & AI engineer with **3.5+ years** building data systems and production AI at scale — a **Claude-powered payroll assistant** shipped to **10–15k paying businesses**, **SDGR-Bot** (a solo **RAG policy assistant**), **pgvector + hybrid retrieval** search over a **10K-product catalog**, payroll for **80k+ SMBs**, a **computational-chemistry platform**, and a **33M-user marketplace**. I love clean code, backend chaos, and ReactJS that doesn’t break on refresh.

I specialize in **zero-downtime PostgreSQL migrations**, **multi-tenant access control**, and **query-performance engineering** — and I ship **production AI**: **RAG pipelines** (pgvector, hybrid retrieval, rerankers), **LLM tool/function calling**, **structured outputs**, **eval harnesses (NDCG/MRR)**, and **guardrails**. I own every architecture decision end-to-end while using **AI coding agents** as leverage.

---

## 🔥 Work Highlights

<style>
.exp-container {
  display: flex;
  flex-wrap: wrap;
  gap: 1.2rem;
  margin-top: 2rem;
}
.exp-logo {
  flex: 0 0 70px;
}
.exp-logo img {
  width: 70px;
  height: auto;
}
.exp-details {
  flex: 1 1 300px;
}
.exp-details .role {
  font-style: italic;
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  column-gap: 1rem;
  row-gap: 0.15rem;
}
.exp-details .date {
  color: #888;
  font-style: normal;
  margin-left: auto;
  white-space: nowrap;
}
.exp-details ul {
  line-height: 1.55;
}
</style>

<div class="exp-container">
  <div class="exp-logo">
    <img src="{{ base_path }}/images/pagarbook_logo.webp" alt="Pagarbook Logo">
  </div>
  <div class="exp-details">
    <strong>Pagarbook</strong><br>
    <div class="role"><strong>Software Development Engineer II</strong><span class="date">Aug 2025 – Jul 2026</span></div>
    <ul>
      <li>Co-built an <strong>AI assistant</strong> for product docs and <strong>natural-language payroll/attendance queries</strong>; owned <strong>tool-calling + document-context selection</strong> across web, Android, and iOS; tested with <strong>10–15k paid businesses</strong>, reducing routine support contacts</li>
      <li>Set a <strong>Claude model-escalation ladder</strong>: Haiku → <strong>LLM mood classifier</strong> at 15 messages → Opus for frustrated users → support handoff at 20; tuned <strong>prompt/context</strong> for accuracy, cost, and latency; <strong>session-scoped tools</strong> keep business ID out of the model</li>
      <li>Led a <strong>zero-downtime payroll-config migration</strong>, moving TDS, month-size & shift-hours to <strong>per-staff/per-template sources of truth</strong> with <strong>temporal snapshotting</strong> of settled pay cycles</li>
      <li>Architected a <strong>multi-tenant Business→Division access hierarchy</strong> spanning login, auth context, RBAC, and the data layer</li>
      <li>Cut a <strong>multi-million-row PostgreSQL backfill</strong> from hours to minutes (<strong>~8× smaller working set</strong>) and dropped another query from <strong>~70s → ~15s</strong> via a <strong>partial index over a 96.9M-row scan</strong></li>
      <li>Diagnosed and healed <strong>production organizationId drift</strong> across 7 tenant-scoped tables, fixing <strong>soft-delete/orphan-row bugs</strong> that had blocked customers for days</li>
      <li>Owned a <strong>breaking payroll API change</strong> across <strong>5 codebases</strong> (NestJS, iOS, Android, Kotlin Multiplatform, 2 React apps), preempting a fleet-wide login lockout</li>
      <li>Overhauled the <strong>busiest attendance endpoint</strong>, collapsing per-staff N+1 queries from <strong>≈600 → 5</strong> for a 100-staff org and <strong>parallelizing 12 serial setup queries</strong></li>
      <li>Expanded RBAC with <strong>granular payments/payroll/roster permissions</strong> and built a <strong>transactional bulk staff-deactivation pipeline</strong> with <strong>backoff-retry</strong></li>
    </ul>
  </div>
</div>

<div class="exp-container">
  <div class="exp-logo">
    <img src="{{ base_path }}/images/sdgr-logo.png" alt="Schrödinger Logo">
  </div>
  <div class="exp-details">
    <strong>Schrödinger (D.E. Shaw)</strong><br>
    <div class="role"><strong>Software Development Engineer I → II</strong><span class="date">Oct 2023 – Jul 2025</span></div>
    <ul>
      <li>Built and shipped <strong>SDGR-Bot</strong>, a solo <strong>RAG-based policy assistant</strong> on GCP; indexed <strong>450+ HR/IT/finance policy docs</strong> with <strong>Gemini text-embedding-004 + pgvector</strong> and <strong>hybrid retrieval</strong> (PostgreSQL FTS + <strong>Reciprocal Rank Fusion</strong>); served <strong>70+ employees</strong>, self-serving <strong>60% of routine policy queries</strong> at <strong>p95 < 2s</strong></li>
      <li>Added <strong>LLM intent parsing</strong> with <strong>regex fallback</strong>, <strong>cross-encoder reranking</strong>, <strong>citation-grounded answers</strong>, and <strong>guardrails</strong>; built an <strong>eval harness</strong> reaching <strong>0.94 NDCG@10 / 0.91 MRR</strong> on 120 labeled Q&A, with <strong>role-scoped access controls</strong></li>
      <li>Built and owned <strong>40+ Django REST APIs</strong> for ST3, Schrödinger's internal target-tracking platform, developed <strong>TDD-first with Pytest</strong>, and shipped the matching React features end-to-end, cutting <strong>time-to-insight by 35%</strong></li>
      <li>Designed <strong>Celery + Redis async pipelines</strong> for structured-data processing, notifications, and audit trails, keeping long-running jobs off the request path</li>
      <li>Automated the refresh of <strong>ChEMBL</strong> and other external biology datasets via a scheduled job, <strong>Airbyte</strong> ingestion, and <strong>DBT</strong> reshaping, cutting data latency from <strong>~1–2 hours → ~10–15 minutes</strong></li>
      <li>Ran the team's <strong>containerized microservices on GCP</strong> (Docker, Kubernetes) and authored <strong>reusable Terraform modules</strong> adopted across 4–5 environments</li>
      <li>Interviewed backend candidates and mentored interns; <strong>promoted to SDE II in 11 months</strong></li>
    </ul>
  </div>
</div>

<div class="exp-container">
  <div class="exp-logo">
    <img src="{{ base_path }}/images/olx-logo.png" alt="OLX Logo">
  </div>
  <div class="exp-details">
    <strong>OLX Group (Autos Division)</strong><br>
    <div class="role"><strong>Software Engineer Intern</strong> <span class="date">Jan 2023 – Jul 2023</span></div>
    <ul>
      <li>Improved OLX Autos' <strong>user sell journey</strong> (<strong>33M+ monthly users</strong>), increasing <strong>completion rate by 8%</strong></li>
      <li>Raised <strong>front-end test coverage from 30% to 70%</strong> using <strong>React Testing Library</strong></li>
      <li>Shipped UI improvements with cross-functional teams via <strong>GitLab CI/CD</strong></li>
    </ul>
  </div>
</div>

---

## 🎓 Education

<div class="exp-container">
  <div class="exp-logo">
    <img src="{{ base_path }}/images/lnmiit-logo.png" alt="LNMIIT Logo">
  </div>
  <div class="exp-details">
    <strong>The LNM Institute of Information Technology (LNMIIT)</strong><br>
    <div class="role">B.Tech in Computer Science and Engineering <span class="date">Aug 2019 – May 2023</span></div>
    <ul>
      <li>Focused on <strong>systems</strong>, <strong>backend engineering</strong>, and <strong>cloud infrastructure</strong></li>
      <li>Explored <strong>competitive programming</strong> and <strong>problem-setting</strong></li>
      <li>Member of programming and tech clubs; organized a <strong>Codeforces-rated contest</strong></li>
    </ul>
  </div>
</div>

Fun fact: I survived 8 semesters, competitive programming addiction, and 2am bug hunts.

---

## 💡 What I Do

I love building things that work reliably, scale gracefully, and occasionally surprise me with a `200 OK`.

- 🧠 **AI Engineering** — **RAG pipelines**, **vector search** (pgvector, FAISS), embeddings, **hybrid retrieval**, **Reciprocal Rank Fusion**, **rerankers**, **tool/function calling**, **structured outputs**, LLM APIs (Claude, Gemini, Llama/Groq), **prompt & context engineering**, **intent classification**, **guardrails**, **eval harnesses (NDCG/MRR)**, LangChain, LangGraph, LlamaIndex, **fine-tuning (LoRA)**
- 🤖 **AI-Assisted Dev** — Cursor, Claude Code, GitHub Copilot as leverage, not autopilot
- 💻 **Languages** — TypeScript, JavaScript, Python, Java, SQL, C++
- 🧱 **Backend Engineering** — Node.js, NestJS, FastAPI, Django, DRF, Sequelize/TypeORM, Celery... basically my playground
- 📊 **Data & Messaging** — PostgreSQL, Redis, MySQL, Kafka, RabbitMQ, ClickHouse, Airbyte, DBT, custom ETL flows, and SQL queries that don't need `EXPLAIN`... usually
- 🌐 **Frontend (when absolutely necessary)** — React, Next.js, Redux, and TypeScript magic
- ⚙️ **Cloud & DevOps** — Docker, Kubernetes, GCP, AWS, Terraform, GitHub Actions, Datadog, Git (with as few `--force` pushes as possible)
- 🧪 **Testing** — Pytest, Unittest, Swagger docs, and tests that actually run on CI

---

## 🏆 Nerd Stats

- CodeChef 4★ (1930) · Codeforces Specialist (1422)  
- Solved 1200+ problems across LeetCode, Codeforces, CodeChef  
- Authored 35+ problems on HackerEarth that stumped more people than I expected  
- Co-organized a Codeforces-rated contest at LNMIIT  

---

## 🚀 Outside Work

When I'm not shipping code or debugging my own brain:
- I'm usually designing side projects or playing with system design puzzles  
- Writing articles, creating tools, or helping juniors prep for tech roles  
- Secretly refreshing [AWS status page](https://status.aws.amazon.com/) during outages just for fun

---

🎉 Thanks for visiting!  
If you like what you see, feel free to explore my work using the sidebar. Or just say hi—I don’t bite (unless you `git push --force` on main).
