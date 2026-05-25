# Project 1 — Agentic RAG Due-Diligence Researcher

> **Tagline**: An agentic researcher that turns "should we acquire Acme Corp?" into a cited, multi-source due-diligence brief in 8 minutes — replacing a half-day analyst task with $0.10 of inference, deployed on AWS with LangGraph, MCP, and full evals.

## The use case (the story you actually tell)

Boutique M&A advisors, corp-dev teams at mid-size strategics, and PE associates working below partner level all hit the same workflow:

> "Run a first-pass due diligence read on Target X. Pull the last 4 quarterly filings, recent news, executive bios, recent press releases, any litigation, regulatory exposure. Synthesize a 3-page brief by tomorrow."

Today this costs them 6–8 hours of an analyst's time at $40–60/hr, plus a **$20–35K/seat/year subscription to PitchBook or Capital IQ** — which delivers raw data, not synthesis. The output is inconsistent across analysts. Things get missed (a 2-month-old SEC 8-K filed late on a Friday, a regulator's enforcement action against a subsidiary). And the brief is non-trivial to update when news drops two days later.

Existing AI tools have not solved this:
- **ChatGPT / Claude alone** hallucinate company-specific facts and can't cite primary sources — fatal for due diligence where citations are a legal requirement.
- **Vector-search "chat with your docs" tools** retrieve passages but don't *synthesize across sources* with self-checking.
- **PitchBook / Capital IQ** are data terminals, not researchers — you still write the brief.

You build the missing piece: an agentic RAG researcher with citations, reflection loops that catch coverage gaps, and verifiable faithfulness.

## Why agentic RAG specifically (defended)

| Alternative | Why it fails for this use case |
|---|---|
| **Single-shot RAG** (retrieve + answer in one pass) | Misses coverage gaps. Brief skips regulatory exposure because no one asked the question explicitly. |
| **Chain-of-Thought without retrieval** | Hallucinates company facts. Fatal in a legal/financial setting. |
| **LangChain chains** (linear pipeline) | No reflection, no conditional branching, no ability to "go back and search more if confidence is low." Caps out at junior-analyst quality. |
| **Build it as a multi-step prompt with GPT-5** | No grounding to primary sources. Can't generate the citation tree the brief requires. |

What this use case *requires*:
- Multi-source synthesis (SEC EDGAR + news APIs + press releases + LinkedIn bios)
- A **reflection loop** that asks "did we cover litigation? regulatory? executive turnover?" and goes back to retrieve if not
- **Inline citations** linking every claim to a source span
- A **faithfulness check** before returning — measured, not vibes
- **Streaming UI** so the user sees the agent working (M&A people are impatient)

LangGraph + MCP + AWS Bedrock is the smallest stack that delivers all five.

## What you build

A web app where a user pastes a target company name (and optionally seed URLs). The agent:

1. **Plans** — decomposes into 6–8 sub-questions covering financials, leadership, market, regulatory, litigation, recent events (LangGraph node)
2. **Researches in parallel** — pulls SEC EDGAR filings, recent news (NewsAPI), press releases, executive backgrounds (tool calls to MCP servers from Project 3)
3. **Reflects** — coverage check: any sub-question without a confident answer? Loop back (LangGraph conditional edge with `max_iterations=3`)
4. **Drafts** — structured brief with inline citations and a Sources section
5. **Self-evaluates** — runs faithfulness + citation-coverage checks before returning
6. **Streams the UI** — user sees "🔍 Pulling 10-K…" "⚠️ Found a delayed 8-K, re-checking litigation…" "✍️ Drafting Regulatory section…"

Demo dataset: 20 publicly-traded mid-caps with real SEC filings and real news. Anyone clicking the demo gets a real, cited brief in 8 minutes.

## Tech stack (deliberately chosen)

| Layer | Tech | Why |
|---|---|---|
| Agent orchestration | **LangGraph** | #1 recruiter keyword for agentic roles |
| LLM | **AWS Bedrock — Claude 3.7 Sonnet** primary, Llama 3.3 fallback | AWS narrative + multi-model judgment |
| Tool integration | **MCP servers from Project 3** | Portfolio composability story |
| Vector store | **pgvector on RDS** | Default 2026 choice; cheaper than dedicated DB |
| Embeddings | `text-embedding-3-small` (OpenAI) + `bge-base-en-v1.5` self-hosted alt | Mix of API + self-hosted shows judgment |
| Document parsing | **Docling** + **Unstructured** | Modern post-OCR stack for 10-Ks |
| Data sources | SEC EDGAR (free), NewsAPI (free tier), web scrape via Firecrawl | Real data, not toy |
| Backend | **FastAPI** on **AWS Lambda** (SAM) | Cloud-native FastAPI |
| Async / queues | **Celery + Redis** (ElastiCache) | Matches your existing stack |
| Frontend | **Next.js + Vercel AI SDK** | Default 2026 chat stack |
| Tracing | **LangSmith** | Deepest LangGraph integration |
| Evals | **Project 4 harness** (Ragas + DeepEval) in CI | Hard evidence of production rigor |
| Infra | **AWS SAM** + GitHub Actions | Real IaC, not click-ops |

## Target metrics (these go in the README hero — tie to outcomes, not just numbers)

| Metric | Target | Why it matters / outcome story |
|---|---|---|
| **End-to-end time to brief** | < 8 min for a mid-cap | vs 6–8 hr analyst time — **45× speedup** |
| **Cost per brief** | < $0.10 (Sonnet + retrieval) | vs ~$300 in analyst labor — **3000× cost reduction** |
| **Faithfulness (Ragas)** | ≥ 0.85 on 100-sample eval | Industry-standard, recruiters know this number |
| **Citation coverage** | 100% of factual claims linked to a source span | Legal requirement for due diligence |
| **Answer relevancy (Ragas)** | ≥ 0.80 | Two-dimension measurement |
| **Context precision (Ragas)** | ≥ 0.75 | Retrieval quality separated from generation |
| **p95 retrieval latency** | < 300ms | pgvector + HNSW should easily hit this |
| **Reflection loop terminates** | ≤ 3 iterations, always | Termination guarantee — common interview question |
| **Eval suite in CI** | Yes, fails PR on regression | Senior-engineer signal |
| **Deployed at a real URL** | Yes | Non-negotiable in 2026 |

## What recruiters will look for

Per [the 2026 AI Engineer Interview Roadmap](https://www.mockexperts.com/blog/2026-ai-engineer-interview-roadmap-rag-llms) and [LangChain's State of Agent Engineering](https://www.langchain.com/state-of-agent-engineering):

### Tier 1 signals (must hit all)

- ✅ **Real use case stated up front** — "M&A due diligence" beats "research assistant"
- ✅ **LangGraph code**, not LangChain chains — visible state graph, conditional edges, checkpointing
- ✅ **Termination guarantees** — `max_iterations=3` on reflection, called out in README
- ✅ **Real eval suite** — `pytest --eval` runs Ragas, fails CI on regression
- ✅ **LangSmith public trace URL** in the README
- ✅ **AWS deployment with IaC** — `template.yaml` committed
- ✅ **Streaming UI with tool-call rendering** — recruiters open demo URL and watch tokens
- ✅ **Hero metrics tied to outcomes** — "8 min vs 8 hr, $0.10 vs $300" in first paragraph

### Tier 2 signals (hit 3+ of these)

- ✅ **Cost dashboard** — LangSmith cost panel screenshot or live link
- ✅ **Prompt-injection defense** — explicit section + adversarial tests
- ✅ **Cache layer** — Redis cache for embeddings and reranks; latency wins demoed
- ✅ **Graceful degradation** — when Bedrock throttles, fall back to a self-hosted Llama
- ✅ **Architecture diagram** — Excalidraw/Mermaid embedded in README
- ✅ **Blog post link** with the design write-up

### Red flags to avoid

- ❌ LangChain chains (use LangGraph)
- ❌ Generic "research assistant" framing with no real user
- ❌ No eval numbers
- ❌ Pinecone-only stack (use pgvector to differentiate)
- ❌ Localhost-only
- ❌ "Hallucination is unsolvable" anywhere in the README

## The 60-second pitch (memorize this for interviews)

> "M&A analysts spend 6–8 hours on first-pass due diligence and pay $25K/year for terminals that don't synthesize. I built an agentic RAG researcher that produces a cited brief in 8 minutes for $0.10. LangGraph state machine with a reflection loop capped at 3 iterations, Bedrock Claude 3.7 Sonnet with Llama fallback, pgvector retrieval, citations grounded to SEC EDGAR filings, faithfulness 0.87 on a 100-sample held-out eval, the eval gate runs in CI on every PR. Streaming UI via Vercel AI SDK. Deployed on AWS Lambda with SAM. Here's the live URL."

That is what a senior interviewer wants to hear.

## Suggested folder structure

```
agentic-rag-due-diligence/
├── README.md                  ← hero metrics + arch diagram + demo URL
├── USE_CASE.md                ← who uses this, what task it replaces, cost story
├── ARCHITECTURE.md            ← deeper diagrams + decision log
├── EVALS.md                   ← eval methodology + dataset card
├── SECURITY.md                ← prompt-injection threat model + tests
├── template.yaml              ← AWS SAM
├── apps/
│   ├── api/                   ← FastAPI + LangGraph agent
│   │   ├── graph/             ← LangGraph nodes + edges
│   │   ├── retrieval/         ← pgvector hybrid (uses Project 2's lib)
│   │   ├── tools/             ← SEC EDGAR, NewsAPI, MCP client glue
│   │   └── evals/             ← Ragas test suite (uses Project 4)
│   └── web/                   ← Next.js + Vercel AI SDK
├── infra/
│   ├── rds/                   ← pgvector schema + migrations
│   └── github-actions/        ← deploy + eval workflows
└── notebooks/
    └── eval-experiments.ipynb
```

## Stretch features (only if Week 4 goes well)

- **Multi-agent variant** — researcher + critic + writer as separate agents (supervisor pattern)
- **Human-in-the-loop** — LangGraph interrupt before publishing; user edits citations
- **Citation grounding** — every claim links to a span in the source PDF
- **Self-RAG style adaptive retrieval** — agent decides when retrieval is needed
- **Subscription mode** — re-runs the brief weekly and emails a "delta" report (real product wedge)

## Blog post outline

Title: **"I built an agentic researcher that replaces a $300 analyst task with $0.10 of LLM — here's how LangGraph + AWS made it production-grade"**

Sections:
1. The M&A-analyst-shaped hole (the user pain, with cost numbers)
2. Why chain-based RAG isn't enough (and why LangGraph specifically)
3. The state graph (with diagram)
4. Why pgvector over Pinecone (with $/month numbers)
5. The reflection loop — and how I stop it from looping forever
6. Evals (with Ragas numbers in a table)
7. Deployment lessons from Bedrock + Lambda
8. What I'd do differently — and what's next (subscription/delta-reports)

## Checklist

- [ ] Phase 1 Week 1 — LangGraph patterns repo done
- [ ] Phase 1 Week 2 — AWS bootstrap deployed
- [ ] Phase 1 Week 3 — Agent backend running locally against SEC EDGAR + NewsAPI
- [ ] Phase 1 Week 4 — Frontend + deployed + blog post live
- [ ] Eval gate in CI
- [ ] LangSmith public trace link in README
- [ ] Architecture diagram in README
- [ ] Hero metrics tied to outcomes ("45× faster, 3000× cheaper")
- [ ] Listed at top of habib36.dev
