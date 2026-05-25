# Career Roadmap → Job-Ready Full-Stack AI Engineer

> Personal plan for Habibur Rahman. Goal: land a remote full-stack AI engineer role at a serious AI-first or AI-adopting company within ~4–6 months.

This folder is your single source of truth for the next 4 months. Everything is grounded in 2026 market data (sources cited in each document) and tuned to your current CV.

## Reading order

| # | Doc | What it does |
|---|---|---|
| 1 | [`01-market-research.md`](./01-market-research.md) | Real 2026 data: what's hot, what's dying, salary bands, recruiter keywords |
| 2 | [`02-skill-priorities.md`](./02-skill-priorities.md) | Your current state vs. demand → ordered gap list with reasoning |
| 3 | [`03-learning-plan.md`](./03-learning-plan.md) | 16-week, week-by-week plan tying skills → projects → artifacts |
| 4 | [`projects/`](./projects) | 5 portfolio projects, each in its own folder, each with target metrics |

## The 5 portfolio projects (build in this order)

| # | Project (use case) | Primary skills | Folder |
|---|---|---|---|
| 1 | **Agentic RAG Due-Diligence Researcher** — replaces 6–8hr M&A analyst task with $0.10 of inference | LangGraph · MCP · pgvector · AWS Bedrock · Vercel AI SDK · Evals | [`projects/01-agentic-rag-assistant/`](./projects/01-agentic-rag-assistant) |
| 2 | **Hybrid Search for OSS Docs** — drop-in DocSearch replacement, +40% retrieval success on real user queries | Hybrid retrieval · reranking · pgvector · benchmarking | [`projects/02-hybrid-search-kb/`](./projects/02-hybrid-search-kb) |
| 3 | **MCP Server Suite** — three servers solving workflows you actually hit (KB search, safe Postgres, bilingual termbase) | MCP protocol · tool design · OSS distribution | [`projects/03-mcp-server-suite/`](./projects/03-mcp-server-suite) |
| 4 | **`evalgate`** — OSS eval-gate I built because Ragas + DeepEval + Langfuse don't combine cleanly for CI | Ragas · DeepEval · Langfuse · pytest CI | [`projects/04-llm-eval-harness/`](./projects/04-llm-eval-harness) |
| 5 | **On-Device Legal Translator** — privacy-compliant tuned 2B model translators can run on a laptop | QLoRA · Gemma 2 2B / Qwen 2.5 · Unsloth · llama.cpp · Tauri | [`projects/05-finetuned-domain-llm/`](./projects/05-finetuned-domain-llm) |

## The four "must hit" outcomes by end of week 16

1. **AWS deployments on your GitHub** — at least 2 projects deployed to AWS with infra-as-code (CDK or SAM)
2. **4+ live demo URLs** linked from your README and portfolio
3. **4–6 blog posts** on `habib36.dev`, each with concrete numbers ("cut p95 from 4.2s to 800ms")
4. **MCP server in the public registry** + **OSS repo with 50+ stars** (lowest-friction is #4 LLM Eval Harness)

## Daily/weekly cadence

- **Mon–Fri**: 2–3 hr deep-work block on whichever project's active that week
- **One blog post per ~3 weeks** (4 weeks per project = 1 post per project minimum)
- **Sunday**: review weekly progress against the plan, update `todos/` checkboxes, push code

## What this plan does NOT include (deliberate)

- Generic ML/DL courses (Andrew Ng etc.) — you're past that level
- More competitive programming — 3,000+ problems is enough
- Java / Rust / Go — only if a target JD requires it
- A 5th portfolio website — you have `habib36.dev` already, polish the writing instead

## Memory

- Stack confirmed in your README: **Celery + Temporal + Redis** for async (not BullMQ/Redis Streams). All learning-plan code samples use this stack.
- You're **actively looking, full-time, remote-worldwide**. All projects optimize for what remote-first AI companies screen for.
