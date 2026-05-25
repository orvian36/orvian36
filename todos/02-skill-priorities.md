# Skill Priorities — Your Gaps, Ordered by ROI

> For each skill: what evidence says you need it, where you stand today, what to learn specifically, and which portfolio project will demonstrate it.

## Your starting position (assessed honestly)

**You're not entry-level.** You're a mid-level full-stack engineer with one year of *real* production LLM work. The gap is not "learn ML" — it's "round out the modern AI engineering stack so recruiters can't filter you out."

| Strength | Why it matters |
|---|---|
| Production RAG on Weaviate at Makebell | Real, not demo — most candidates' RAG experience is tutorial-level |
| Next.js + FastAPI + Postgres + Supabase full-stack | Hits the "full-stack" part of the JD |
| LangChain in production | Baseline ticket |
| YOLOv8 + NCNN + Raspberry Pi thesis (mAP@50 = 0.936) | Differentiator — most LLM candidates can't ship edge ML |
| 3,000+ CP problems, Codeforces Specialist | Solid CS fundamentals signal |
| Clean architecture / SOLID / design patterns | Hireable judgment, not just code |
| Celery + Temporal + Redis async stack | Production-grade backend |

## Tier 1 — Critical gaps (will autofilter you out of JDs)

Each of these is keyword-searched by recruiters in 2026.

### 1. LangGraph + agentic patterns

**Why**: AI Agent Engineer = #1 fastest-growing role (340% YoY). 60% of new enterprise software projects in 2026 include an agentic component.

**Where you are**: LangChain only.

**What to learn**:
- LangGraph: state graphs, conditional routing, human-in-the-loop, persistence/checkpointing
- Reflection / replanning patterns
- Multi-agent: supervisor pattern, swarm pattern, hand-off
- Tool calling at scale: parallel tools, retries, validation
- LangGraph Studio for debugging

**Demonstrated in**: Project 1 (Agentic RAG Assistant)

### 2. MCP (Model Context Protocol)

**Why**: 78% enterprise adoption (April 2026), 9,400+ public servers, supported by every major IDE and LLM platform. Single highest-signal, lowest-effort portfolio addition.

**Where you are**: Likely zero.

**What to learn**:
- MCP spec (resources, tools, prompts)
- TypeScript + Python SDK
- stdio vs SSE transports
- Auth patterns for production MCP
- Publishing to the public MCP registry

**Demonstrated in**: Project 3 (MCP Server Suite)

### 3. Cloud (AWS specifically — Bedrock + Lambda + RDS)

**Why**: Top autofilter on JDs. You have zero cloud on your CV.

**Where you are**: Local + Docker + Vercel/Supabase.

**What to learn**:
- Bedrock: model invocation, agents, knowledge bases, guardrails
- Lambda + API Gateway for FastAPI deployments (via Mangum)
- RDS Postgres with pgvector extension
- S3 + Textract for document processing
- IAM basics
- AWS CDK or SAM for infra-as-code
- CloudWatch for logs/metrics
- **Optional but cheap**: AWS Certified Cloud Practitioner cert (~2 weeks)

**Demonstrated in**: Projects 1 and 2 deployed to AWS

### 4. Evals — Ragas + DeepEval + LLM-as-judge

**Why**: The single most discriminating senior interview question is "how do you measure your RAG?" Without an answer, you're capped at mid-level.

**Where you are**: Nothing explicit.

**What to learn**:
- RAG metrics: faithfulness, answer_relevancy, context_precision, context_recall
- Retrieval metrics: recall@k, MRR, nDCG
- Ragas for ad-hoc, DeepEval for pytest/CI integration
- LLM-as-judge: design, biases, calibration
- Golden eval sets — how to build, version, and grow them
- Snapshot testing for prompts

**Demonstrated in**: Project 4 (LLM Eval Harness — your OSS contribution)

### 5. LLM Observability — LangSmith or Langfuse

**Why**: Production necessity. "We just print logs" disqualifies senior candidates.

**Where you are**: Likely nothing structured.

**What to learn**:
- Tracing concepts: spans, runs, sessions
- LangSmith dataset & evaluator workflows (for LangGraph projects)
- Langfuse self-hosted setup (for the OSS angle)
- Cost / token tracking, latency dashboards
- Prompt versioning and A/B testing

**Demonstrated in**: Projects 1, 4

### 6. Public technical writing + 1 OSS contribution

**Why**: Recruiters engage 80% more with portfolios that include writing. Strongest cheap moat for remote-from-Bangladesh candidates.

**Where you are**: Portfolio site exists but no posts.

**What to learn**:
- Pick 4–6 specific, numbers-driven post topics from each project
- One small OSS PR (Langfuse, Ragas, LangGraph, or a popular MCP server)

**Demonstrated in**: `habib36.dev/blog` + GitHub OSS PR

## Tier 2 — High-value upgrades (signal senior-level depth)

### 7. Advanced retrieval — pgvector + hybrid + reranking + query transformation

**Why**: Weaviate-only puts you in "I read the LangChain RAG tutorial" bucket. Senior interviews probe chunking, hybrid search, reranking specifically.

**What to learn**:
- pgvector with HNSW, ivfflat tradeoffs
- BM25 + dense hybrid search (Reciprocal Rank Fusion)
- Re-rankers: Cohere Rerank, BGE re-ranker, ColBERT-v2
- Chunking strategies: semantic, structural, parent-child / hierarchical
- Query transformation: HyDE, multi-query, query decomposition
- Modern document parsers: Unstructured, LlamaParse, Docling

**Demonstrated in**: Project 2 (Hybrid Search Knowledge Base)

### 8. Vercel AI SDK + streaming chat UIs

**Why**: 20M+ monthly downloads. Default expectation for Next.js + chat. You already use Next.js, just bolt this on.

**What to learn**:
- `useChat` hook, `streamText`, `streamObject`
- Tool-call rendering, generative UI patterns
- SSE vs WebSocket tradeoffs
- React Server Components + AI SDK

**Demonstrated in**: Project 1 frontend

### 9. Prompt-injection defense + LLM security

**Why**: OWASP LLM Top 10 #1. Increasingly on JDs.

**What to learn**:
- Direct vs indirect prompt injection
- Input/output filtering, prompt-isolation patterns
- RBAC on retrieved chunks (PII redaction, row-level access)
- Tool-call sandboxing for agents
- Adversarial / red-team test cases

**Demonstrated in**: Projects 1, 3 (security section in each README)

### 10. Fine-tuning — QLoRA on a small (1–3B) model + on-device serving

**Why**: $195K–$350K specialty band. More importantly: ability to defend **"when *would* we fine-tune?"** in interviews is critical. The strongest 2026 answer isn't "to beat GPT-5" (you won't) — it's **"to deploy where cloud APIs can't go"** (privacy, offline, cost-at-scale). Small models + QLoRA is the right tool for that story.

**What to learn**:
- LoRA / QLoRA mechanics (rank, alpha, target modules, RSLoRA)
- Unsloth for fast iteration on free / cheap GPUs (Colab T4, Modal A10G)
- Axolotl for YAML-driven pipelines
- TRL for SFT / DPO / ORPO
- The escalation framework: prompt → RAG → fine-tune (and how to justify each step with measurements)
- Small base models: **Gemma 2 2B**, **Qwen 2.5 1.5B/3B**, **Phi-3.5 mini**, **Llama 3.2 1B/3B**, **SmolLM2**
- GGUF quantization (q4_k_m, q5_k_m) + llama.cpp / Ollama for CPU serving
- vLLM for GPU serving (mention in resume even if not primary)
- Packaging tuned models as shippable products (Tauri, VS Code extension, MCP server)

**Demonstrated in**: Project 5 (On-Device Legal Translator)

## Tier 3 — Nice-to-have (mention, don't lead)

| Skill | Why mention | Effort |
|---|---|---|
| Streaming-first data engineering (Kafka basics) | Larger systems context | 1 week tutorial |
| Multimodal retrieval (CLIP-based image RAG) | Hot in 2026 interviews | Side experiment in Project 2 |
| DSPy for prompt optimization | Cool tool to namedrop | Half a weekend |
| OpenTelemetry for AI tracing | Vendor-neutral observability | Built into Langfuse work |
| DDIA (Designing Data-Intensive Applications) | System-design interviews | Read end-to-end across 16 weeks |

## Tier 4 — Explicitly skip

| Skip | Why |
|---|---|
| Andrew Ng / generic ML courses | You're past this level — opportunity cost |
| More competitive programming | 3,000+ problems is diminishing returns |
| Another personal portfolio website | habib36.dev exists; polish content instead |
| New languages (Rust, Go) | Only if a JD demands |
| TensorFlow / Keras | PyTorch is the standard; you only need it if a JD demands |
| Gradio "demo" projects | Real Next.js apps signal more |
| Pinecone-only stacks | Losing market share, costly |
| LangChain *without* LangGraph | Mid-level-cap signal |

## Visual: skill → project matrix

```
                      P1     P2     P3     P4     P5
                      Agt    Hyb    MCP    Eval   FT
LangGraph             ★★★    -      -      ★      -
MCP                   ★★     -      ★★★    -      -
AWS Bedrock           ★★★    ★★     -      -      ★★
AWS Lambda/RDS        ★★★    ★★     -      -      -
pgvector              ★★     ★★★    -      -      -
Hybrid + rerank       ★      ★★★    -      -      -
Ragas/DeepEval        ★★     ★★     -      ★★★    ★★
LangSmith/Langfuse    ★★★    ★      -      ★★★    ★
Vercel AI SDK         ★★★    ★      -      -      -
Prompt-inj defense    ★★     ★      ★★     -      -
QLoRA / Unsloth       -      -      -      -      ★★★
Small-LM serving      -      -      -      -      ★★★
 (llama.cpp / Ollama)
Tauri desktop app     -      -      -      -      ★★
Public writing        ★      ★      ★      ★      ★
```

Each project hits 4–6 skills. Five projects cover everything in Tiers 1 and 2.

## How interviewers will probe each tier

Per [DataCamp's 2026 RAG interview Q&A](https://www.datacamp.com/blog/rag-interview-questions) and [MockExperts roadmap](https://www.mockexperts.com/blog/2026-ai-engineer-interview-roadmap-rag-llms):

| Tier | Sample question | What demonstrates you've shipped it |
|---|---|---|
| Agentic | "How do you stop an agent from looping forever?" | LangGraph state graph w/ max-iterations + reflection — point to Project 1 |
| MCP | "When would you build an MCP server vs a REST API?" | Project 3 — show the trade-off you made |
| Cloud | "Walk me through a Bedrock deployment with knowledge bases" | Project 1 deployed; show CDK/SAM |
| Evals | "How do you know your RAG is actually working?" | Project 4 dashboards + CI eval gate |
| Retrieval | "When does pgvector stop being enough?" | Project 2 benchmark numbers |
| Security | "How do you defend against indirect prompt injection?" | Project 1 security README section |
| Fine-tuning | "When *would* you fine-tune?" | Project 5 — defend the privacy/on-device use case + show the escalation prompt→RAG→tune |

## Bottom line

Your job is not to learn everything — it's to build **5 projects that demonstrate Tier-1 and Tier-2 fluency**, each with concrete metrics, deployed URLs, and write-ups. The plan in [`03-learning-plan.md`](./03-learning-plan.md) sequences this over 16 weeks.
