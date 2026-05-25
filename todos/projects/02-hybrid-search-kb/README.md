# Project 2 — Hybrid Search for OSS Docs (Drop-in DocSearch Replacement)

> **Tagline**: A benchmarked Postgres-native hybrid search engine that replaces Algolia DocSearch on a real open-source project's docs site — and delivers +40% retrieval success on the queries Algolia silently fails.

## The use case (the story you actually tell)

Every popular open-source project has a docs site. Most use **Algolia DocSearch** or built-in static-site search. Both are **keyword-matching engines** that fail in predictable ways:

- A developer types **"how do I make my window draggable"** — Tauri docs phrase it as "Configuring window decorations and drag regions." Algolia returns zero results.
- A developer types **"agent stops after one tool call"** — LangGraph docs say "Conditional edges and recursion limits." No match.
- A developer types **"my React app keeps refetching"** — TanStack Query docs say "Stale-while-revalidate semantics." No match.

These are real questions. They get asked daily in the project's Discord and on Stack Overflow, then re-asked the next day because no one can find the answer in the docs. **The docs *contain* the answer; the search engine can't find it.**

Hybrid search (BM25 + dense + rerank) fixes this. But shipping it as a project requires more than a benchmark on MS MARCO — it requires picking a real OSS project, indexing their docs, mining their Discord/GitHub Discussions for actual user queries, and proving the upgrade on real questions. That's what you build.

## Why hybrid + rerank specifically (defended)

| Alternative | Why it fails for this use case |
|---|---|
| **Algolia DocSearch** (current default for OSS docs) | Pure keyword. Loses on conceptual paraphrasing. Free for OSS but quality-capped. |
| **Built-in static-site search** (Docusaurus, Nextra) | Worse than Algolia. Substring-match. |
| **Dense-only RAG** (LangChain default tutorial) | Loses on exact-term matches ("recursion_limit" → finds blogs about React recursion, not the LangGraph param) |
| **BM25-only** (Postgres tsvector alone) | Loses on conceptual paraphrasing — same problem as Algolia |
| **Pinecone managed** | Can't tune HNSW; costs ~$180/mo at this scale; no BM25 hybrid out of the box |
| **A 70B LLM that "reads the docs"** | $/query is untenable for free OSS docs traffic; latency too high |

The combination of **BM25 + dense (RRF fusion) + Cohere/BGE reranker** wins on both axes — exact terms *and* conceptual paraphrasing — at p95 < 200ms and a per-query cost under $0.0005. That's the only setup that can be a real Algolia replacement.

## What you build

1. **Pick one real OSS project** as the target. Strong candidates (high traffic, technical audience, easy to demo): **Tauri**, **LangGraph**, **Drizzle ORM**, or **Tanstack Query**. Pick one with active Discord/GitHub Discussions so you have real user queries to mine.
2. **Index their docs** end-to-end: scrape, chunk (structural + semantic), embed, store in pgvector with HNSW.
3. **Mine real user queries** — pull 100–200 actual questions from their Discord / Discussions / Stack Overflow. This becomes your eval set.
4. **Run the head-to-head**: query each on (a) the project's current search and (b) your hybrid+rerank pipeline. Score retrieval success manually (or with an LLM judge calibrated against 30 human-labeled samples).
5. **Build a Vercel demo** anyone can search. Same UX as Algolia's overlay.
6. **Write the open letter** to the project: "Here's what your current search misses, here's the benchmark, here's the drop-in replacement, take it if you want."

The OSS-contribution angle is the multiplier. Even if they say no, *the offer itself* is portfolio gold.

## Tech stack

| Layer | Tech | Why |
|---|---|---|
| Storage + vectors | **Postgres + pgvector (HNSW)** | Fast, cheap, JD-relevant |
| Lexical search | **Postgres tsvector + ts_rank_cd** | Native, no Elasticsearch needed |
| Reranker | **Cohere Rerank 3.5** + **BGE re-ranker v2 (m3)** self-hosted option | Both API and self-hosted in repo |
| Chunking | **LangChain semantic chunker** + **Docling** structural | Multiple strategies, benchmarked |
| Document parsing | **Docling** for code-heavy docs | Modern post-OCR / structural-aware |
| Query transforms | **HyDE** + multi-query expansion | Tier 1 interview topics |
| Backend | **FastAPI** on AWS Lambda | Reuses Project 1's deployment template |
| Frontend | **Next.js + cmdk** search overlay (Algolia-style) | Familiar UX |
| Benchmarks | **Real Discord/Discussions queries** + BEIR subset for cross-validation | Public + real |

## Target metrics (tied to outcomes)

| Metric | Target | Outcome story |
|---|---|---|
| **Retrieval success on real user queries** | **+35–50%** vs the project's current search | Lead bullet — the headline win |
| **Recall@10 vs dense-only** | **+25–35%** on BEIR cross-check | Cross-validation against a public benchmark |
| **nDCG@10 (hybrid+rerank vs dense-only)** | **+40–50%** | Reranking is where the big gains live |
| **p95 retrieval latency** | < 200ms (no rerank), < 400ms (with rerank) | Hard SLO — must feel "instant" like Algolia |
| **Cost per 1M queries** | < $500 (rerank-on) | OSS-affordable; Algolia free tier comparison |
| **Ingestion throughput** | ≥ 50 pages/min on Lambda | Realistic for a single-tenant KB |
| **`make benchmark` reruns everything** | One command from clean clone | Reproducibility = senior signal |

> **Critical**: every number must come from your own scripts on the project's actual docs and real user queries — not from blog posts.

## What recruiters will look for

Per [DataCamp's 2026 RAG interview Q&A](https://www.datacamp.com/blog/rag-interview-questions) and [the kore1 hiring guide](https://www.kore1.com/hire-rag-engineers-2026/):

### Tier 1 signals

- ✅ **A real OSS project as the target** — not "BEIR benchmark"
- ✅ **A table of numbers up top** — "+42% retrieval success on Tauri Discord questions, p95 180ms"
- ✅ **All 4 retrieval modes implemented**, not just claimed
- ✅ **HNSW parameters explained** (`m`, `ef_construction`, `ef`) — proves you tuned
- ✅ **Chunking comparison** with numbers
- ✅ **`make benchmark`** anyone can rerun
- ✅ **The "open letter" to the project** — even if rejected, the offer itself is signal

### Tier 2 signals

- ✅ **Hybrid fusion math explained** (RRF formula, not just code)
- ✅ **Failure cases listed** — "hybrid loses to dense on these query types: …" — judgment signal
- ✅ **Cost breakdown** per query
- ✅ **Caching strategy** for embeddings and rerank
- ✅ **Multimodal stretch** — index docs images with CLIP for one section
- ✅ **PR opened on the target project** to improve search (even if not merged)

### Red flags

- ❌ Numbers without an eval set
- ❌ "Improves quality" claims without metrics
- ❌ Complicated retrieval setup with no comparison baseline
- ❌ Pinecone instead of pgvector
- ❌ Benchmark on synthetic data only — no real user queries

## The 60-second pitch

> "OSS docs all use Algolia DocSearch or built-in static search. Both are keyword engines that fail on conceptual queries — Tauri's Discord has 200+ unanswered 'how do I X' threads where the answer is in their docs under a different phrasing. I indexed Tauri's docs with pgvector + BM25 + Cohere Rerank, mined 150 real user queries from their Discord, and benchmarked head-to-head: +42% retrieval success, p95 180ms, 100× cheaper per query than the alternative I considered. The demo's live. I sent the maintainers an open-letter PR. Reusable as a drop-in DocSearch replacement for any OSS project."

## Folder structure

```
hybrid-search-oss-docs/
├── README.md            ← target project + table of numbers + demo URL
├── USE_CASE.md          ← who suffers, why current search fails, real query examples
├── BENCHMARKS.md        ← full methodology, datasets, hyperparams
├── DATASETS.md          ← dataset cards (target docs + mined queries)
├── OPEN_LETTER.md       ← the proposal to the target project's maintainers
├── src/
│   ├── ingestion/       ← Docling + Unstructured parsers, chunkers
│   ├── retrieval/       ← dense / BM25 / hybrid / rerank modes
│   ├── transforms/      ← HyDE, multi-query
│   └── api/             ← FastAPI endpoints
├── benchmarks/
│   ├── run.py
│   ├── target_docs/     ← Tauri / LangGraph / whichever
│   ├── mined_queries/   ← 150+ real Discord / Discussions queries
│   └── beir_xcheck/     ← public-benchmark cross-validation
├── notebooks/
│   ├── 01-chunking-comparison.ipynb
│   └── 02-rerank-ablation.ipynb
├── demo/
│   └── nextjs/          ← Algolia-style overlay search demo
└── infra/
    └── template.yaml    ← AWS SAM (RDS + Lambda)
```

## Blog post outline

Title: **"Algolia misses 42% of the questions developers ask in Tauri's Discord — here's a pgvector + rerank drop-in that doesn't"**

Sections:
1. The "answered-in-docs-but-unfindable" problem (with real Discord screenshots)
2. Why keyword search fails on conceptual queries — and why pure dense fails too
3. The four retrieval modes, code + math
4. The eval set: mining 150 real queries from a real Discord
5. Results table — including the queries where my system *also* failed
6. Reranking — when it's worth the latency
7. The open letter — what I sent to the maintainers
8. Reproducing the whole thing from a clean clone

This is HN/r/programming front-page material if executed well — quantitative, original, useful, slightly provocative.

## Stretch features

- **ColBERT-v2** as a third retrieval mode
- **Multi-vector retrieval** via parent-child chunking
- **Filter performance test** — recall@10 with metadata filters
- **Streaming results** — return top-1 immediately, rest via SSE
- **A second target project** if the first goes well — establishes pattern

## Checklist

- [ ] Phase 2 Week 5 — Eval harness v1 from Project 4 ready
- [ ] Phase 2 Week 5 — Target OSS project chosen + docs indexed
- [ ] Phase 2 Week 6 — 4 retrieval modes implemented + first benchmark
- [ ] Phase 2 Week 7 — 150 real queries mined + chunking + query transforms benchmark
- [ ] Phase 2 Week 8 — Streamlit/Next.js demo deployed + open-letter drafted + blog post live
- [ ] BENCHMARKS.md with all numbers
- [ ] `make benchmark` works from clean clone
- [ ] Open letter / PR submitted to target project
