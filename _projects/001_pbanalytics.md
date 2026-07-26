---
title: "PB Analytics: Self-Hosted Product Analytics Platform"
excerpt: "An 8-service Amplitude/Mixpanel alternative — SQL-free funnel and usage exploration for PMs, built solo with AI coding agents handling implementation."
collection: portfolio
---

### 📅 Duration: Apr 2026 – May 2026
### 🛠️ Tech Stack: Django · Kafka · ClickHouse · Streamlit · PostgreSQL · Redis · Docker

**PB Analytics** is a self-initiated internal product-analytics platform built as an Amplitude/Mixpanel alternative, giving PMs SQL-free funnel and usage exploration.

---

### 🔍 Features

- Designed and built the entire 8-service platform solo — ingestion, streaming, storage, and query — making every architecture and design decision myself, with AI coding agents handling implementation.
- Engineered the streaming pipeline (Django ingest → Kafka → ClickHouse, 180-day TTL) for ~20K events/sec, with disk-queue fallback for zero event loss during broker outages.
- Tested the pipeline against a 160M-event dataset to validate throughput and durability under load.

---

### 🔗 Links

- 📂 **GitHub**: [tanaymakharia17/pb-analytics-main](https://github.com/tanaymakharia17/pb-analytics-main)

---

> This project let me own an entire system end-to-end — from ingestion to query — while using AI coding agents as leverage for implementation, so I could focus fully on architecture and design decisions.
