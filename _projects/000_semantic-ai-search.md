---
title: "Semantic AI Search: Natural-Language Product Search"
excerpt: "Hybrid semantic + full-text search over a 10K-product catalog, fusing pgvector embeddings with Postgres full-text via Reciprocal Rank Fusion and LLM intent parsing."
collection: portfolio
---

### 📅 Duration: Jun 2026 – Jul 2026
### 🛠️ Tech Stack: Python · pgvector · PostgreSQL (full-text) · LLM (Groq / Llama) · Docker

**Semantic AI Search** brings natural-language search to a 10K-product catalog, combining vector semantics with classic full-text retrieval so queries understand *meaning*, not just keywords.

---

### 🔍 Highlights

- Built **hybrid retrieval** that combines **pgvector** semantic embeddings with Postgres full-text search, fused via **Reciprocal Rank Fusion** for the best of both signals.
- Added **LLM-based intent parsing** (price, sort, category) with a regex fallback, so filters are extracted from plain-English queries.
- Fully **Dockerized** with a **black-box evaluation harness** to measure retrieval quality objectively.

---

> This project was my hands-on dive into AI engineering — embeddings, vector search, and retrieval fusion — while keeping the same architecture-first, own-every-decision approach I bring to backend systems.
