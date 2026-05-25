# Project 3 — MCP Server Suite (Three Workflows You Actually Hit)

> **Tagline**: Three production-grade Model Context Protocol servers — each one solving a workflow I kept running into — listed in the public registry and composable in Claude Desktop.

## The use case (the story you actually tell)

You don't build MCP servers in the abstract. You build them because there's a workflow you keep hitting and the existing tooling doesn't help. Each of these three was a real friction point — turning them into MCP servers also turns them into portfolio + OSS distribution.

### Server 1 — `mcp-pg-rag`: "I want Claude to search my hybrid-search KB *while I code*"

**Pain**: You built Project 2's hybrid-search KB. Great. But to actually use it during research, you have to (a) leave your editor, (b) open a browser tab, (c) type your query in the demo UI, (d) copy/paste the results back into wherever you were writing. That's 4 context switches per query. Every time.

**Fix**: an MCP server wraps Project 2's hybrid search behind two tools (`search`, `ingest_document`). Now Claude Desktop, Cursor, ChatGPT, and Windsurf can query it natively. You write a draft, Claude pulls the relevant docs in-context, you keep typing.

### Server 2 — `mcp-pg-explorer`: "I want to safely poke production Postgres from Claude without DBA fear"

**Pain**: Half the time you ask Claude a "how should I model this?" question, the right answer depends on what's *actually* in your production database — current schema, sample rows, table sizes, foreign keys. You don't want to copy-paste schema. You also don't want to give an LLM write access to a prod DB.

**Fix**: an MCP server with a strictly **read-only** Postgres explorer. Tools: `list_tables`, `describe_table`, `sample_rows`, `explain_query`, `run_query` (SELECT-only, statement timeout, row cap, query EXPLAIN preview, sensitive-column redaction). Production-safety properties most demo MCP servers skip.

### Server 3 — `mcp-termbase`: "I want Claude to keep terminology consistent across a long bilingual translation"

**Pain**: This one ties directly to your **Makebell experience**. A translator working on a 500-page legal document needs terminology consistency — "force majeure" must be rendered the same way every time across 47 occurrences. Existing CAT-tool termbases don't expose to LLM chat clients. The result: Claude paraphrases inconsistently across long docs, translators have to manually post-edit.

**Fix**: an MCP server wrapping a bilingual termbase (you can seed it from your Makebell domain knowledge — public legal/financial bilingual term lists exist). Tools: `lookup_term`, `suggest_translation`, `check_consistency` (scans a draft against the termbase), `record_decision` (translator commits a choice for the rest of the document). This is a workflow tool that doesn't exist anywhere else in the MCP registry — your domain wedge.

## Why MCP, not REST APIs (defended)

| Alternative | Why it fails for these workflows |
|---|---|
| **Three REST APIs + custom plugins per client** | You'd have to write Claude Desktop plugin + Cursor extension + ChatGPT custom GPT + Windsurf integration separately. MCP solves all four at once. |
| **One big "AI gateway" service** | Couples three independent workflows, hard to version, hard to give away as OSS |
| **Skip and just paste content into Claude** | The 4-context-switch problem; doesn't scale beyond a handful of queries |

Per the [MCP 2026 roadmap](https://blog.modelcontextprotocol.io/posts/2026-mcp-roadmap/) and [MCP adoption stats](https://www.digitalapplied.com/blog/mcp-adoption-statistics-2026-model-context-protocol):
- **78% of enterprise AI teams** had an MCP-backed agent in production by April 2026
- Public registry: **1,200 → 9,400+ servers** in 12 months
- **97M monthly SDK downloads** by March 2026 (970× growth in 18 months)
- Supported natively by Claude, ChatGPT (Apps SDK), Gemini, Cursor, Windsurf, Zed, JetBrains, Vercel AI SDK, OpenAI Agents SDK

Recruiters keyword-search "MCP" and "Model Context Protocol." Almost no candidates ship one. Three composable servers — including one in a real domain (legal/financial bilingual) — is exceptional.

## What you build (three independent packages)

Each server is independently installable (npm/PyPI), shipped to the public registry, and documented for at least **Claude Desktop + Cursor**.

```
Composition example (in your README):
  Claude Desktop ─┬─→ mcp-pg-rag      (search private knowledge base)
                  ├─→ mcp-pg-explorer  (introspect Postgres schema)
                  └─→ mcp-termbase     (terminology consistency)

  Used together: "Draft me a regulatory memo on Section 5.6 of our M&A 
  agreement, in English and Bengali, with consistent terminology and 
  citing our internal legal-precedent KB."
```

## Tech stack

| Layer | Tech | Why |
|---|---|---|
| MCP SDK | **TypeScript SDK** (servers 2, 3) + **Python SDK** (server 1) | Mixed stack mirrors real-world |
| Transport | **stdio** for local; **SSE** for hosted server 1 | Both transports = senior signal |
| Auth | **API key** + **scoped tokens** | Production-grade |
| Hosting (server 1) | **AWS Lambda + API Gateway WebSocket** | Reuses Project 1 infra |
| CI / registry | GitHub Actions auto-publish to npm + PyPI + MCP registry | Distribution discipline |
| Testing | MCP Inspector + integration tests in Claude Desktop | Real client validation |
| Termbase data (server 3) | Hong Kong bilingual legal corpus + IATE legal subset | Public, legally usable |

## Target metrics (tied to outcomes)

| Metric | Target | Outcome story |
|---|---|---|
| **Servers shipped to public registry** | 3 | Verifiable third-party fact; rare in 2026 portfolios |
| **Context-switches eliminated per query** | 4 → 0 | Tangible UX win for `mcp-pg-rag` |
| **Postgres "schema-paste" messages avoided** | 100% (for server 2 users) | Real productivity story |
| **Terminology consistency uplift on test doc** | +35% (for `mcp-termbase`) vs Claude without it | Measurable on a held-out 50-page sample |
| **npm/PyPI downloads** (month 1 after Show HN) | ≥ 200 total | Achievable with one decent launch post |
| **GitHub stars combined** | ≥ 50 | Useful proxy |
| **Mean latency per tool call** | < 500ms (excluding LLM time) | Cache where possible |
| **Documented Claude Desktop config snippet** | Yes, in each README | Removes friction |

## What recruiters will look for

Per the [MCP 2026 roadmap](https://blog.modelcontextprotocol.io/posts/2026-mcp-roadmap/) and [thenewstack production analysis](https://thenewstack.io/model-context-protocol-roadmap-2026/):

### Tier 1 signals

- ✅ **Servers in the public registry** — verifiable third-party fact
- ✅ **Each server tied to a real workflow** — not generic "tools collection"
- ✅ **Both transports** (stdio + SSE) demonstrated
- ✅ **Real auth**, not "set this env var" — OAuth or scoped tokens
- ✅ **Safe Postgres** in server 2 — production safety thinking
- ✅ **Composability example** — Claude Desktop screenshot using all 3 together
- ✅ **Domain wedge** — `mcp-termbase` is something no one else has built

### Tier 2 signals

- ✅ **Versioning + changelog** per server
- ✅ **Rate limits + retries** visible
- ✅ **MCP-conventional error responses**
- ✅ **CI publishes to npm/PyPI** on tag
- ✅ **Telemetry opt-in** with privacy notice

### Red flags

- ❌ Tools that wrap an LLM internally (MCP exposes data + tools, not models)
- ❌ Mutating Postgres operations without explicit safety
- ❌ Server only works in your local environment
- ❌ No installation instructions for at least Claude Desktop + Cursor
- ❌ Generic "research tools" with no real workflow story

## The 60-second pitch

> "I kept hitting the same three friction points: copy-pasting between my docs search and my editor, copy-pasting Postgres schema into Claude, and inconsistent legal terminology across long translations. I turned each into a Model Context Protocol server, shipped them to the public registry, and demoed them composed inside Claude Desktop. The termbase server uses Hong Kong bilingual legal data — direct extension of the Makebell pipeline I built. All three on npm/PyPI, three-letter blog post, integration tests run in Claude Desktop in CI."

## Folder structure

```
mcp-server-suite/
├── README.md                       ← overview + composition examples
├── USE_CASES.md                    ← the three pains, the three fixes
├── packages/
│   ├── mcp-pg-rag/                 ← server 1 (Python)
│   │   ├── README.md               ← workflow story + Claude Desktop config
│   │   ├── pyproject.toml
│   │   ├── src/mcp_pg_rag/
│   │   └── tests/
│   ├── mcp-pg-explorer/            ← server 2 (TypeScript)
│   │   ├── README.md
│   │   ├── package.json
│   │   ├── src/
│   │   └── tests/
│   └── mcp-termbase/               ← server 3 (TypeScript)
│       ├── README.md
│       ├── data/                   ← bilingual term seeds
│       └── ...
├── examples/
│   ├── claude-desktop-config.json
│   ├── cursor-config.json
│   └── composition-demo.md         ← screenshot + transcript using all 3
└── .github/workflows/
    ├── publish-npm.yml
    ├── publish-pypi.yml
    └── registry-submit.yml
```

## Blog post outline

Title: **"Three Model Context Protocol servers I built because I kept needing them — and why MCP changed how I think about tools"**

Sections:
1. MCP in two paragraphs (the primer)
2. The three friction points (with screenshots of the "before")
3. Server 1: RAG-as-MCP — what to expose, what to hide
4. Server 2: making read-only Postgres safe enough to ship
5. Server 3: a domain MCP — extending my Makebell translation work
6. Composition is the killer feature — three servers in one Claude Desktop session
7. Publishing to the registry: what worked, what didn't
8. What I'd build next

## Stretch features

- **OAuth flow** in one server (on the 2026 MCP roadmap)
- **Gateway pattern** — one router server proxying to the three behind it
- **Audit trail** logging tool calls (roadmap priority)
- **Submit to the official Anthropic-curated MCP list** (very high signal)

## Checklist

- [ ] Phase 3 Week 9 — `mcp-pg-rag` + `mcp-pg-explorer` shipped to registry
- [ ] Phase 3 Week 10 — `mcp-pg-rag` hosted on AWS with SSE
- [ ] Phase 3 Week 11 — `mcp-termbase` shipped (the domain wedge)
- [ ] Phase 3 Week 11 — Composition example with all 3 documented (screenshot + transcript)
- [ ] Phase 3 Week 12 — Blog post + Show HN posted
- [ ] All 3 on npm/PyPI
- [ ] All 3 in MCP registry
- [ ] Claude Desktop + Cursor config in each README
