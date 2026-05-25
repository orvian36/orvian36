# 16-Week Learning Plan

> Designed to take you from your current state to job-ready full-stack AI engineer with 5 deployed portfolio projects and 4–6 blog posts. Assumes ~15–20 hr/week of focused work.

## Structure

- **4 phases × 4 weeks each** — each phase produces one major artifact
- Project 4 (eval harness) runs *parallel* with Projects 1–3 because you'll use it on them anyway
- Every Sunday: 1-hour review against the plan, push code, update [`projects/*`](./projects) checklists

## Phase 0 — Setup (Week 0, ~5 hr before Week 1)

Before you start, do these one-time setups:

- [ ] **AWS account** with billing alerts ($30/month cap to start)
- [ ] **HuggingFace account** + login locally
- [ ] **LangSmith account** (free tier)
- [ ] **Langfuse self-hosted** locally via Docker
- [ ] **OpenAI + Anthropic API keys** with budget alerts
- [ ] **Replicate or Modal account** for GPU rentals (Project 5)
- [ ] **`habib36.dev/blog` ready to publish** — draft template, RSS, OG images
- [ ] Read once: [the MCP spec quickstart](https://modelcontextprotocol.io)

## Phase 1 — Foundations + Agentic RAG (Weeks 1–4)

**Goal**: ship **Project 1 (Agentic RAG Research Assistant)** deployed on AWS with evals.

### Week 1 — LangGraph mastery
- Build 3 toy LangGraph apps progressively:
  1. Simple linear graph with two LLM calls
  2. Conditional routing + tool calling
  3. Reflection loop with checkpointing
- Read LangChain's *State of Agent Engineering* report end-to-end
- Add LangSmith tracing to all three
- **Output**: GitHub repo `langgraph-patterns` with 3 reference examples

### Week 2 — AWS core for AI
- Deploy a "Hello World" FastAPI app to **Lambda + API Gateway** via SAM
- Spin up an RDS Postgres instance with **pgvector**
- Use **Bedrock** to invoke Claude 3.5 / Llama 3 from the FastAPI app
- Set up CloudWatch dashboards
- **Output**: published AWS deployment template repo (becomes Project 1's base)

### Week 3 — Project 1 backend
- Build the agent: LangGraph state machine with research → write → cite steps
- Document ingestion: PDF + URL → pgvector with metadata
- Bedrock for inference (Claude 3.7 Sonnet as primary)
- Background ingestion via Celery + Redis (your existing stack)
- LangSmith traces on every node

### Week 4 — Project 1 frontend + ship
- Next.js + **Vercel AI SDK** chat UI with streaming
- Tool-call rendering (show "searching docs..." "writing answer...")
- Citations rendered as clickable links
- Deploy frontend (Vercel) + backend (AWS)
- **First eval run** with Ragas baseline metrics
- **Write blog post #1**: "Building a production agentic RAG app on AWS with LangGraph + pgvector"

**Phase 1 deliverable**: Project 1 live at a real URL, blog post published, LangSmith dashboard public-link enabled

## Phase 2 — Hybrid Retrieval + Eval Discipline (Weeks 5–8)

**Goal**: ship **Project 2 (Hybrid Search KB)** + **Project 4 (Eval Harness)** v1.

### Week 5 — Eval harness v1 (Project 4)
- Wrap Ragas + DeepEval into a single pytest plugin
- Add `pytest --eval-gate` CLI that fails CI on regressions
- Integrate Langfuse self-hosted for trace persistence
- Apply it retroactively to Project 1 — measure baseline

### Week 6 — Advanced retrieval theory + hybrid search
- Implement BM25 in pgvector via tsvector
- Combine BM25 + dense with Reciprocal Rank Fusion (RRF)
- Add Cohere Rerank as final stage
- Benchmark on a public dataset (MS MARCO subset or BEIR)
- Compare 4 configurations: dense-only / BM25-only / hybrid / hybrid+rerank

### Week 7 — Project 2 chunking & query rewriting
- Implement 3 chunking strategies: fixed, semantic (langchain semantic chunker), structural (markdown-aware)
- Add HyDE and multi-query expansion
- Integrate Docling for advanced PDF parsing
- Run full benchmark again
- **Blog post #2**: "Hybrid search beats dense by 31% on faithfulness — here's the benchmark"

### Week 8 — Project 2 UI + deploy
- Minimal Next.js frontend for querying the KB
- Eval dashboard (using Langfuse) showing live metrics
- Deploy backend to AWS (same SAM template as P1)
- **Eval harness v2**: add to GitHub Actions, run on every PR

**Phase 2 deliverable**: Project 2 live with public benchmark numbers in README, Project 4 used by Projects 1 and 2

## Phase 3 — MCP + OSS Footprint (Weeks 9–12)

**Goal**: ship **Project 3 (MCP Server Suite)** with at least 2 servers in the public MCP registry. Push Project 4 to 50+ GitHub stars.

### Week 9 — MCP fundamentals
- Read the MCP spec end-to-end
- Build a trivial MCP server: weather lookup
- Build a second: PostgreSQL query tool (read-only, well-scoped)
- Test in Claude Desktop + Cursor + Windsurf
- **Submit one** to the public MCP registry

### Week 10 — MCP server #2: RAG-as-MCP
- Wrap Project 2's hybrid search as an MCP server
- Auth: API key + per-tool RBAC
- Cache layer with Redis
- **Submit to the registry**

### Week 11 — MCP server #3: ChatOps-style internal tool
- Pick one: Linear/Jira ticket creator, GitHub PR summarizer, or Notion search
- Production hardening: rate limits, input validation, structured errors
- **Submit to the registry**

### Week 12 — Project 4 OSS push
- Polish README, add architecture diagram, demo GIF
- Write a clear "why this exists" section
- Submit Show HN + r/MachineLearning + r/LocalLLaMA posts
- Submit a PR upstream to Langfuse or Ragas (bug fix or small feature)
- **Blog post #3**: "How I built and shipped 3 MCP servers to the public registry"

**Phase 3 deliverable**: 3 MCP servers live, 1 OSS PR merged upstream, Project 4 has 50+ stars (achievable with a decent Show HN)

## Phase 4 — Fine-Tuning + Polish + Job Apps (Weeks 13–16)

**Goal**: ship **Project 5 (Fine-Tuned Domain LLM)**, refresh resume + README, start applying.

### Week 13 — QLoRA fine-tuning a small (1–3B) model with a real use case
- **Frame the use case first** (one paragraph in the repo's `PROBLEM.md`): on-device legal translator assistant — translators handling client-privileged documents that legally cannot be sent to Claude/GPT, running offline on a laptop CPU
- Dataset prep: ~2–4K legal/financial bilingual pairs (EN↔ZH or EN↔BN — pick the one matching your strongest data sources). Mix of public corpora (HK bilingual legislation, UN parallel) + synthetic + 200–500 gold hand-curated for eval
- Fine-tune **Gemma 2 2B Instruct** (or **Qwen 2.5 1.5B / 3B** if EN↔ZH) with **Unsloth** on **free Colab T4** or a $0.50/hr Modal A10G — total spend under $5
- Compare base vs fine-tuned on 200-sample held-out eval (use Project 4's harness)
- Track everything in W&B; make the run public-linkable

### Week 14 — Project 5 serving + product packaging
- Quantize to **GGUF q4_k_m** for laptop CPU; verify <2GB RAM at inference
- Serve via **llama.cpp / Ollama** for the on-device demo (primary)
- Optional: **vLLM on Modal serverless** for the GPU path (mention on CV; not the primary demo)
- Add the model as a **"private mode" backend option** in Project 1's agent — ties the privacy story across the portfolio
- Cost/latency comparison: Bedrock Claude vs your tuned 2B for the same legal-translation task, with the numbers in a table
- Begin a minimal **Tauri 2 desktop app** wrapping the local model so translators can install and use offline

### Week 15 — Polish + portfolio assembly
- Each project README gets:
  - Hero metric (3-second scan)
  - Architecture diagram
  - Demo GIF or video
  - Eval numbers
  - "What I learned" section
- Update [habib36.dev](https://habib36.dev) projects page with all 5
- Ship the Tauri app (at least macOS build, signed) and publish the GGUF model + dataset card to HuggingFace Hub
- **Blog post #4**: "A 2B model on my laptop beat Claude on legal terminology — by losing on everything else. When small models actually win."
- **Blog post #5**: a synthesis: "Building my 2026 AI engineering portfolio — what worked, what didn't"
- Update your README on GitHub with concrete numbers everywhere
- Update CV with quantified bullets

### Week 16 — Application sprint
- Build a target list: 30 companies (mix of AI-native + AI-adopting + remote-first)
- Customize cover letters per target (use one of your own LangGraph agents to draft)
- Reach out to 5 founders/eng leaders per week on LinkedIn (warm DMs, not generic)
- Set up `/now` page on habib36.dev with "open to work" status
- Schedule first system-design + LLM interview mock with a friend or paid service

**Phase 4 deliverable**: 5 polished projects on GitHub, 5 blog posts, refreshed CV, 30 applications submitted

## Parallel ongoing work (every week)

- **DDIA**: 1 chapter/week (16 chapters fits exactly)
- **System design**: 1 problem/week from [Hello Interview](https://hellointerview.com) or Alex Xu vol 2
- **LinkedIn**: 1 thoughtful post per project milestone (not generic motivation)
- **Open issues**: monitor LangGraph, Ragas, Langfuse, MCP repos for "good first issue" — grab one per phase

## Budget (USD, total over 16 weeks)

| Item | Cost |
|---|---|
| AWS (Lambda + RDS + Bedrock + S3) | $40–80/month → ~$200–320 total |
| OpenAI + Anthropic APIs | $100 |
| GPU rental for fine-tuning (Colab T4 free / Modal A10G) | $0–5 |
| Domain + portfolio hosting (already paid) | $0 |
| Optional AWS Cloud Practitioner cert | $100 |
| **Total** | **~$400–525** |

This is the right level of spend for the career payoff. Set hard caps in each provider's console.

## Daily routine template

| Time | Activity |
|---|---|
| Mornings (1 hr) | Read: 1 DDIA section OR 1 system-design problem OR 1 LangChain/Anthropic blog post |
| Deep-work block (2 hr, Mon–Fri) | Active project work |
| Evenings (30 min, 3×/week) | Blog drafting / OSS PR review / GitHub polish |
| Sunday (1 hr) | Plan review, commits pushed, todos updated |

## Red flags — adjust the plan if any of these happen

- **End of Week 4, Project 1 not deployed**: cut scope (drop hybrid search from P2, ship faster)
- **End of Week 8, no blog post published**: writing is the bottleneck — block half a Saturday and ship a draft
- **End of Week 12, Project 4 has <10 stars**: do a stronger Show HN / write a better launch post
- **End of Week 14, no interviews**: your CV or portfolio messaging is the issue — get a paid resume review before Week 16

## Stretch goals (only if ahead of schedule)

- **MCP server in the official Anthropic-curated list** (high signal)
- **Co-author a blog post** with a known practitioner (Hamel Husain, Eugene Yan, Jason Liu)
- **Talk submitted** to AI Engineer Summit or local meetup
- **One additional fine-tune** with DPO/ORPO for instruction-following

## How this plan connects to projects

| Phase | Primary project | Secondary work |
|---|---|---|
| 1 | [Project 1 — Agentic RAG Due-Diligence Researcher](./projects/01-agentic-rag-assistant/) | LangGraph patterns repo |
| 2 | [Project 2 — Hybrid Search for OSS Docs](./projects/02-hybrid-search-kb/) + [Project 4 — `evalgate` v1](./projects/04-llm-eval-harness/) | Pick target OSS project early |
| 3 | [Project 3 — MCP Server Suite](./projects/03-mcp-server-suite/) | Project 4 (`evalgate`) push to 50+ stars |
| 4 | [Project 5 — On-Device Legal Translator](./projects/05-finetuned-domain-llm/) | Portfolio polish + applications |

See each project's README for detailed specs, recruiter signals, and target metrics.
