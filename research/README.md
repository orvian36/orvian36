# Research & Publishing Strategy

> Personal playbook for the publishing-and-research career arm. Sits alongside [`/todos`](../todos) (the project-building arm). Where `/todos` answers "what do I build?" — `/research/` answers "how do I get noticed for what I build?"

## The three paths

You picked engineering writing as the **primary identity** (70% of effort), with paper implementation and OSS contributions as **content sources** that feed it (15% each).

| # | File | What it covers |
|---|---|---|
| 1 | [`01-engineering-writing.md`](./01-engineering-writing.md) | What to write, how to write it, where to publish, how to get attention from a cold start |
| 2 | [`02-oss-contribution.md`](./02-oss-contribution.md) | Picking target projects, finding mergeable issues, anatomy of a PR that gets accepted, the 90-day playbook |
| 3 | [`03-paper-implementation.md`](./03-paper-implementation.md) | Where to find papers in your 5 niches (RAG / LLM / agentic AI / evals / security), how to pick paper #1, the 2-week timebox, what to write |

## Your identity statement

Write this down. Put it on `habib36.dev/about`. Use it in your X bio.

> **"I write about production bilingual and on-device LLM systems, with a focus on retrieval and small-model adaptation. Each post points at a paper I implemented or an OSS patch I shipped."**

Specific enough to own. Broad enough for ~25 posts a year. Connected to your Makebell + KUET thesis backstory.

## The cadence

Run **2-week sprints**, alternating "ship" weeks and "write" weeks. Over 12 months that produces ~26 posts, ~13 paper repos, ~13 OSS PRs — one identity, three artifact types feeding it.

| Sprint type | Week 1 (ship) | Week 2 (write) | Output |
|---|---|---|---|
| A | Implement paper on your bilingual/domain data | Write up implementation + numbers + failures | 1 repo + 1 post |
| B | Substantive PR to LangGraph / Langfuse / Unsloth | Post about what you patched + why | 1 PR + 1 post |
| C | New `/todos` project ships | Build write-up with metrics | 1 project + 1 post |

Alternate A → B → A → B → C → A → B → A → B → C → … (4 A's, 4 B's, 2 C's per 10 sprints).

## How this combines with `/todos`

| Artifact source | Feeds writing how |
|---|---|
| `/todos` Project 1 (Agentic RAG due-diligence) | Build write-up post (Sprint C) |
| `/todos` Project 2 (Hybrid Search) | Benchmark post + paper-impl posts on retrieval papers |
| `/todos` Project 3 (MCP servers) | Build write-up + posts on the MCP spec evolution |
| `/todos` Project 4 (`evalgate`) | OSS launch post + posts on eval frameworks generally |
| `/todos` Project 5 (On-Device Translator) | Small-LM posts, fine-tuning papers, QLoRA implementations |

The `/todos` artifacts give you "something to point at" in every post — the difference between blogging into the void and building evidence that compounds.

## Single failure mode to watch

If by month 3 you have:
- Written about your `/todos` projects, but
- Zero papers implemented and zero substantive OSS PRs merged

…the combo has collapsed back to pure writing. **Cure**: skip the next build write-up; force yourself to do a paper-impl Sprint A or an OSS Sprint B instead.

## Files in this folder

Treat each file as a working document. Update them as you learn what works for *you* specifically. The advice here is starting calibration — your actual experience will refine it within a quarter.
