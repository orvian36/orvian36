# OSS Contribution

> Every "AI engineer portfolio" claims OSS contributions. Almost none mean it. The bar is "merged, substantive PRs to projects that are visibly used in production." This file is the playbook for getting there.

## The reality check

What recruiters *actually* see when they click your GitHub:

- ❌ 47 contribution-graph squares from "Update README.md" commits → noise
- ❌ A handful of starred forks with one commit → tutorial completion, not contribution
- ❌ Trivial typo/lint PRs in 20 different repos → spray-and-pray
- ✅ **3–5 merged PRs in 2 projects, each with real diff, discussion, and review** → senior signal

The goal is *not* high contribution count. It's **recognized contributor status in 2 projects in your niche**, with PRs that took real work and earned maintainer trust.

## The contribution funnel

Most engineers want to start at "feature." The right progression is:

```
1. Lurker (read code, watch issues)
        ↓
2. Triager (clarify issues, ask for repro, validate bugs)
        ↓
3. Docs PR (find an actual gap, fix it)
        ↓
4. Small bug fix (< 100 LoC, clear repro)
        ↓
5. Substantive bug fix (clear root-cause analysis)
        ↓
6. Small feature (something maintainers already want)
        ↓
7. Larger feature (with prior discussion)
        ↓
8. Recognized contributor (cited in release notes)
```

**Most new contributors skip 1–3 and land on a "feature" PR that gets ignored or rejected.** The lurker / triager phases (~2 weeks per project) are what makes the difference. They earn you trust before you ask for code review time.

## Picking your target projects

You picked **engineering writing + paper-impl + OSS** as your three paths. For OSS specifically, concentrate on **2 primary + 1 secondary** projects. Quality of relationship > quantity of repos.

### Criteria for picking projects

| Signal | Good | Bad |
|---|---|---|
| **Activity** | 5+ commits/week | < 1 commit/week (dying) |
| **Maintainer responsiveness** | Median PR-to-first-review < 5 days | Months-old open PRs (overwhelmed) |
| **Code quality** | Tests, types, CI | None of those |
| **Issue triage health** | Issues get labels, replies, closes | Open issues from 2 years ago, no replies |
| **Niche fit** | Tied to your `/todos` projects | Adjacent only |
| **PR acceptance norms** | "Good first issue" labels, contributing guide | No norms, maintainer caprice |
| **Size of community** | Small enough to get noticed (50–500 contributors) | Massive (10K+ contributors — you'll be invisible) |
| **Foundation backing or scrappy startup** | Either is fine | Single-author abandonware to avoid |

### Recommended targets for you (May 2026)

**Primary — pick both:**

- **LangGraph** ([langchain-ai/langgraph](https://github.com/langchain-ai/langgraph))
  - Why: your daily driver for `/todos` Project 1; recruiters keyword-search it; active LangChain team
  - PR difficulty: medium; LangChain team reviews carefully
  - Subareas to focus: checkpointing edge cases, state-graph debugging tools, integration with Langfuse/LangSmith

- **Langfuse** ([langfuse/langfuse](https://github.com/langfuse/langfuse))
  - Why: you'll use it for `/todos` Project 4 (`evalgate`); ClickHouse-acquired Jan 2026 → well-funded, active dev
  - PR difficulty: medium; TS-heavy frontend, Python SDK accessible
  - Subareas: Python SDK improvements, dataset/eval workflow features, OTel exporter compatibility

**Secondary — pick one:**

- **Ragas** ([explodinggradients/ragas](https://github.com/explodinggradients/ragas))
  - Why: tied to `/todos` Project 4; very accessible team; high RAG-eval relevance
  - PR difficulty: low–medium

- **Unsloth** ([unslothai/unsloth](https://github.com/unslothai/unsloth))
  - Why: tied to `/todos` Project 5; small team, accessible; aligned to your on-device niche
  - PR difficulty: medium–high (CUDA/Triton kernels need care)

**Skip these (despite the temptation):**

- **llama.cpp** — Gerganov-tier maintainers; PR queue is brutal; very hard to break in unless you're a CUDA/kernel specialist
- **LangChain core** — too big, too saturated; your PR will sit
- **Anthropic / OpenAI SDKs** — minimal community PRs accepted; mostly internal

## Where to look for opportunities

| Source | What to look for |
|---|---|
| **GitHub Issues tab** | `good first issue`, `help wanted`, `bug` (with `confirmed`) labels |
| **Recent issues with no reply** | Triage opportunity — clarify, ask for repro |
| **Recent PRs that stalled** | Maintainer-feedback-pending; you can iterate or pick up |
| **Maintainer-tagged issues** | `pinned`, `roadmap`, `needs help` |
| **Project Discord / Slack** | Early signal on what's being discussed |
| **Release-notes "known issues" section** | Maintainer-blessed work waiting to happen |
| **Recent test failures in CI** | Flaky test fixing earns instant credit |
| **`TODO` / `FIXME` comments in code** | Look for ones with "help welcome" near them |

### The "stale-issue" technique

Many active repos have issues with thoughtful reproductions that nobody's claimed. Triaging these — running the repro, confirming behavior, asking the right follow-ups — is **free maintainer goodwill**, and 50% of the time you'll find a fix you can submit. Two weeks of doing this on LangGraph + Langfuse gives you a deep enough mental model that your first feature PR will land cleanly.

## Pre-PR groundwork (skip this and your PR will fail)

- [ ] **Read `CONTRIBUTING.md`** end to end — most contributors skip it; maintainers notice
- [ ] **Read 5 recently merged PRs** to learn the project's PR-description style, test patterns, commit-message convention
- [ ] **Run the full test suite locally** — `make test` or equivalent. If you can't, fix that *first*.
- [ ] **Match the project's style exactly** — even if you disagree with it
- [ ] **Check linting / type-checking config** runs clean on your branch
- [ ] **Sign the CLA** if there is one (LangChain has one; Langfuse doesn't)

## Anatomy of a mergeable PR

| Property | Rule |
|---|---|
| **Size** | < 200 LoC for first PRs; < 500 for substantive |
| **Scope** | One concern per PR. If you fixed 2 things, open 2 PRs. |
| **Tests** | Include them. If the project doesn't have tests for this area, add one — maintainers love that. |
| **Docs** | If you changed user-visible behavior, update docs in the same PR. |
| **Commits** | Squash to logical commits with good messages. Avoid `wip` and `fix typo` commits. |
| **PR description** | Follow the template (see below) |
| **Reviewer assignment** | If conventions allow, tag the maintainer who owns this area |

### PR description template (steal this)

```markdown
## Problem
[1–2 sentences: what's broken or missing, with a link to the issue if any]

## Approach
[3–6 sentences: what you changed and why this approach over alternatives]

## Alternatives considered
- [Alt 1]: rejected because…
- [Alt 2]: rejected because…
(only if there are real alternatives — don't pad)

## Testing
- [What you ran locally]
- [What new tests this PR adds]
- [What edge cases were verified]

## Screenshot / log (if UI or output change)
[paste]

## Open questions / things to discuss
[any uncertainty you want the reviewer to weigh in on]
```

Maintainers reading 30 PRs a week love this format. It cuts their review time in half.

## Code-review etiquette

| Situation | Right move |
|---|---|
| Reviewer asks a question | Answer in < 24 hours, even if just "looking into it" |
| Reviewer requests a change | Push as new commit, not force-push (until ready to merge) |
| Reviewer disagrees with approach | Ask clarifying questions; don't argue. Often there's context you lack. |
| Reviewer is silent for > 7 days | Polite nudge with @ in a comment, once. Don't double-nudge. |
| PR gets closed without merge | Thank them; ask if there's a way to address concerns; don't sulk in comments |
| You realize your PR is wrong | Close it yourself with a comment explaining; respect saves goodwill |

Maintainers remember tone. Be the contributor they want more PRs from.

## The 90-day playbook

### Weeks 1–2: Lurker phase (both primary projects)

- [ ] Clone both repos; set up dev env; run full test suites
- [ ] Read `CONTRIBUTING.md` and last 10 merged PRs in each
- [ ] Watch the issues tab; turn on notifications
- [ ] Comment on 1 issue per week per project — substantive (repro, clarification, "I see this on X version")
- [ ] Track which subareas you find yourself most able to debug

### Weeks 3–4: First docs / small PRs

- [ ] One PR per project: smallest possible substantive change. Examples:
  - A typo-fix in docs that also clarifies the surrounding paragraph (not pure typo)
  - A test that catches a bug you found while triaging
  - A type-hint fix that revealed an actual bug
- [ ] Goal: get one merged PR in each project's history under your name. That's the entry ticket.

### Weeks 5–6: First substantive PR (one project)

- [ ] Pick one issue from your subarea expertise; comment "I'll take this if no one else is" and wait 24h
- [ ] Open a draft PR early; let maintainer see direction; iterate
- [ ] < 200 LoC. Tests included. Description follows the template above.
- [ ] **Write the blog post about it** in parallel (use that PR as the artifact)

### Weeks 7–8: First substantive PR (other project)

- Same as 5–6 but for the second project.

### Weeks 9–12: Iterate + start small feature

- [ ] Two more substantive PRs across both projects
- [ ] First small feature contribution: something the maintainers have publicly said they want (look at roadmap issues)
- [ ] Each PR gets a corresponding write-up post

## Becoming a recognized contributor

Concrete milestones you can aim for:

| Milestone | Typical timeline | What it signals |
|---|---|---|
| First merged PR | Week 4 | "You can navigate the codebase" |
| Cited in release notes | Month 3 | "Your contribution mattered" |
| Assigned an issue by maintainer | Month 4 | "They trust you to deliver" |
| Granted triage rights | Month 6 | "You're part of the team" |
| Offered maintainer status | Month 9–18 | "You ARE the team" (only accept if you actually want this) |

You don't need to chase maintainer status. **Cited in 3+ release notes of one repo** is already a strong portfolio + interview signal — proof that your work shipped to real users.

## Anti-patterns (do not do these)

| Anti-pattern | Why it's bad |
|---|---|
| **Drive-by PRs** to projects you've never used | Maintainers can smell tutorial farms |
| **2000-LoC architectural rewrites** without prior discussion | Wastes everyone's time, gets closed |
| **Pure formatting / lint-only PRs** | Treated as noise; sometimes auto-closed |
| **Comment storms on unrelated PRs** | "Looks good!" on 20 PRs is spam |
| **Tone-deaf debate in comments** | Even when you're right, lose tone, lose goodwill |
| **Submitting without running tests** | First-impression killer |
| **Force-pushing during review** | Breaks review history; reviewer has to re-read |
| **Disappearing after the PR is opened** | If you can't see a PR through, don't open it |
| **Asking "can someone review this?" repeatedly** | Maintainers are not your employees |

## Tracking your OSS work

Maintain a list — somewhere in `/todos` or `/research` — of every merged PR with date, project, LoC, and link. By month 6 this list is your single most credible artifact on your CV. Concrete suggestion: a `research/oss-log.md` you append to per PR.

## What month-6 success looks like

- 6+ merged PRs across LangGraph + Langfuse + (Ragas or Unsloth)
- At least 1 substantive PR (> 200 LoC) in each primary project
- Cited in 1–2 release notes
- 1 inbound DM from a maintainer asking if you'd take on a larger issue
- 4 blog posts directly tied to OSS work

That's the realistic floor for someone executing this plan with the cadence in [`README.md`](./README.md). It already outperforms 95% of "OSS portfolios" recruiters see.
