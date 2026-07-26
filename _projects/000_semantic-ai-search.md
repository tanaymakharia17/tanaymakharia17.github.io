---
title: "Semantic AI Search: Natural-Language Product Search"
excerpt: "Natural-language search over a 10K-product catalog — hybrid pgvector + full-text retrieval fused with Reciprocal Rank Fusion, plus LLM-powered intent parsing."
collection: portfolio
---

### 📅 Duration: Jun 2026 – Jul 2026
### 🛠️ Tech Stack: Python · pgvector · PostgreSQL (full-text) · LLM (Groq / Llama) · Docker

**Semantic AI Search** brings natural-language search to a 10K-product catalog — understanding what a query *means* instead of just matching keywords, by blending vector semantics with classic full-text retrieval.

---

### 🔍 How It Works

#### 🔎 Hybrid Retrieval:
- Combines **pgvector** semantic embeddings with Postgres **full-text** search.
- Fuses both rankings via **Reciprocal Rank Fusion (RRF)**, so results are relevant both semantically and lexically.

#### 🧠 LLM Intent Parsing:
- Uses an **LLM** to extract structured intent — price, sort order, and category — from plain-English queries.
- Falls back to a **regex** parser when the LLM is unavailable, keeping search resilient.

#### 🧪 Evaluation & Deployment:
- Ships with a **black-box evaluation harness** to measure retrieval quality objectively.
- Fully **Dockerized** for reproducible, one-command setup.

---

### 🔗 Links

- 📂 **GitHub**: [tanaymakharia17/semantic-ai-search](https://github.com/tanaymakharia17/semantic-ai-search)

---

> This was my hands-on entry into AI engineering — embeddings, vector search, and retrieval fusion — bringing the same architecture-first, own-every-decision approach I use for backend systems to the AI stack.
