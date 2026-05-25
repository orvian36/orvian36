# Project 4 — `evalgate`: An OSS Eval-Gate I Built Because I Needed It

> **Tagline**: A pytest-native eval harness that wraps Ragas + DeepEval + Langfuse into a CI quality gate — built because none of the existing tools cleanly handled "fail the PR if faithfulness drops more than 3%."

## The use case (the story you actually tell)

While building Project 1 (agentic RAG due-diligence) and Project 2 (hybrid OSS docs search), I kept needing the **same** CI gate:

> "Run the RAG eval set. If faithfulness drops more than 3% from the baseline, or if any test scores below an absolute threshold, fail the PR. Post a Markdown comment summarizing what changed."

I had Ragas (great metrics, no CI plumbing). I had DeepEval (pytest plugin, but doesn't talk to Ragas natively or persist runs). I had Langfuse (gorgeous dashboards, no CI gate). Stitching all three together for *every* project meant ~200 lines of boilerplate per repo — none of it reusable, all of it brittle.

So I built the 50-line decorator layer that ties them together, and open-sourced it. It became `evalgate`.

This is your **OSS contribution play** — the scratched-your-own-itch credibility story is the cheapest and strongest moat in 2026 OSS portfolios. Per the [Atlan eval frameworks guide](https://atlan.com/know/llm-evaluation-frameworks-compared/):

> "Teams with established CI/CD pipelines, versioned prompt management, and regression testing discipline will get the most out of DeepEval."

`evalgate` is the missing layer that connects those teams to Ragas + Langfuse with one decorator.

## Why a new package (defended)

| Alternative | Why it doesn't fit |
|---|---|
| **Use Ragas directly** | Metrics are great. No baseline comparison, no CI exit codes, no PR comments. 200 lines of glue per project. |
| **Use DeepEval directly** | Pytest plugin is great. But its own metric set duplicates Ragas; mixing them needs glue. |
| **Promptfoo** | YAML-first; great for prompt comparisons but not for code-first pytest workflows. Different audience. |
| **Langfuse evaluators** | Dashboards are excellent. CI gate is not their product. |
| **Write 200 lines of boilerplate in each repo** | Doesn't scale across portfolio; doesn't accumulate community benefit |

The positioning, stated explicitly in the README:

> Ragas gives you the metrics. DeepEval gives you the pytest plumbing. Langfuse gives you the dashboards. None gives you a clean way to **gate CI on RAG quality** without writing the same boilerplate every time. `evalgate` is that thin layer.

That's a specific, defensible slot. It's also a good interview answer to "when would you build vs adopt?"

## What you build

A Python package + pytest plugin that turns this:

```python
from evalgate import eval_case, ragas

@eval_case(dataset="legal-qa-100")
def test_faithfulness():
    return ragas.faithfulness(min_score=0.85)

@eval_case(dataset="legal-qa-100")
def test_no_regression_in_answer_relevancy():
    return ragas.answer_relevancy(regression_tolerance=0.03)
```

Into this CI behavior:
1. Generates predictions (or loads cached ones)
2. Computes metrics via Ragas / DeepEval / custom
3. Persists every run to Langfuse with PR-number tag
4. Compares against `evals/baseline.json`
5. Exits non-zero if any test fails
6. Posts a Markdown report as a PR comment

**Eats its own dogfood**: Projects 1, 2, and 5 all use `evalgate` for their CI eval gates. That's three downstream users on day one — the strongest possible signal for an OSS tool.

## Tech stack

| Layer | Tech | Why |
|---|---|---|
| Language | **Python 3.11+** | Standard for eval tooling |
| Metric backends | **Ragas** + **DeepEval** (pluggable) | Two best-known OSS frameworks; users pick |
| Tracing backend | **Langfuse self-hosted** (default), cloud optional | OSS, ClickHouse-acquired Jan 2026 |
| Test runner | **pytest plugin** (`pytest11` entry point) | Fits existing CI; senior-signal |
| Judge models | OpenAI / Anthropic / Bedrock — pluggable | No vendor lock-in |
| Report format | Markdown + JSON | GitHub PR comment friendly |
| Distribution | **PyPI** + GitHub releases | Real OSS distribution |
| Docs | mkdocs-material on GitHub Pages | Standard, professional |

## Target metrics (tied to outcomes)

| Metric | Target | Outcome story |
|---|---|---|
| **Lines of CI-glue eliminated per downstream project** | ~200 → ~50 | The headline efficiency win |
| **Used in your own Projects 1, 2, 5** | Yes, prominently | Dogfood credibility on day one |
| **GitHub stars (3 months in)** | ≥ 50 | "People care" signal |
| **PyPI downloads (month 1)** | ≥ 500 | Achievable with a Show HN |
| **One upstream PR merged** to Ragas/Langfuse/DeepEval | Yes | The OSS-contribution checkbox |
| **Issues closed in first month** | At least 5 | Maintainer signal |
| **Documented CI integration** for GitHub Actions + GitLab CI | Yes, copy-paste-ready | Friction removal |
| **Boots up in any repo with one `pip install` + one decorator** | Yes | The "I'd use this" signal |

## What recruiters will look for

Per [DataCamp's RAG interview Q&A](https://www.datacamp.com/blog/rag-interview-questions) and the [Atlan eval frameworks guide](https://atlan.com/know/llm-evaluation-frameworks-compared/):

### Tier 1 signals

- ✅ **A real "why does this exist?" story** — leads with the boilerplate problem, not "I made a tool"
- ✅ **You use it in your own work** — Projects 1, 2, 5 all wire it in
- ✅ **Polished README** with hero GIF showing pytest output + Langfuse dashboard
- ✅ **At least one external user** (issue opened, PR submitted, referenced in another repo)
- ✅ **Clear positioning vs. Ragas / DeepEval / Promptfoo** — the slot it fills
- ✅ **Senior-engineering polish** — semantic versioning, changelog, docs site, type hints

### Tier 2 signals

- ✅ **Show HN posted** (even if it doesn't hit the front page)
- ✅ **Upstream PR merged** to Ragas / Langfuse / DeepEval
- ✅ **CI gate visibly used** in Projects 1 + 2 PRs (linked from `evalgate` README)
- ✅ **Documentation site** with mkdocs
- ✅ **Meta-tests** — the harness tests itself
- ✅ **Statistical-significance helpers** — most candidates don't think about this

### Red flags

- ❌ Reinventing what Ragas already does — be a *thin wrapper* with UX, not a competitor
- ❌ No CI integration — defeats the purpose
- ❌ Single-vendor judge model
- ❌ No clear "when to use this vs Ragas directly" answer
- ❌ Released without dogfooding it in your own projects first

## The 60-second pitch

> "While shipping my agentic RAG and hybrid-search projects I kept writing the same 200 lines of glue — Ragas for metrics, DeepEval for pytest, Langfuse for dashboards, custom code to compare against baselines and post PR comments. I extracted the decorator layer that ties them together, called it `evalgate`, and open-sourced it. Three of my own projects use it in their CI. 50+ stars, one merged PR to Ragas upstream, and the position in the README is explicit: 'Ragas gives you metrics, DeepEval gives you pytest, Langfuse gives you dashboards — `evalgate` gives you the CI gate.'"

## Folder structure

```
evalgate/
├── README.md                       ← positioning + hero GIF + quickstart
├── WHY.md                          ← the boilerplate-elimination story
├── docs/                           ← mkdocs site
├── src/evalgate/
│   ├── plugin.py                   ← pytest plugin entry point
│   ├── decorators.py               ← @eval_case
│   ├── adapters/
│   │   ├── ragas.py
│   │   ├── deepeval.py
│   │   └── langfuse.py
│   ├── datasets/                   ← loaders + caching
│   ├── reporting/                  ← markdown + JSON reports + PR-comment
│   └── baselines/                  ← regression comparison logic
├── tests/                          ← meta-tests of evalgate itself
├── examples/
│   ├── basic_faithfulness/
│   ├── rag_eval_in_ci/
│   ├── multi_dataset/
│   └── used_by_project1/           ← link back to Project 1's PR using evalgate
└── .github/workflows/
    ├── ci.yml
    └── publish-pypi.yml
```

## Blog post outline

Title: **"Why your CI should fail when your RAG's faithfulness drops by 3% — and the 50-line decorator I extracted to make it easy"**

Sections:
1. The "vibes-driven LLM dev" problem
2. What an eval-gated CI actually looks like (with screenshots)
3. The 200-line glue problem — why Ragas + DeepEval + Langfuse don't combine cleanly
4. The decorator layer (10 lines of code, real)
5. Dogfooding it: how Project 1's PR template now runs evals as a quality gate
6. Show-HN-style "here's what I shipped, here are the three things I haven't figured out yet, please tear into it"
7. What's next (statistical significance, A/B prompts, multi-judge consensus)

This is the *vulnerable* OSS post — practical, opinionated, honest about open questions. That tone gets cited and reposted.

## Stretch features

- **A/B testing** — compare two prompt versions across a dataset, with statistical significance
- **Cost tracking** alongside quality metrics in the report
- **LLM-as-judge calibration** — measure your judge's reliability against human-labeled samples
- **Promptfoo YAML import** — let users migrate from Promptfoo cleanly

## Checklist

- [ ] Phase 2 Week 5 — v0.1.0 on PyPI; wired into Project 1's CI
- [ ] Phase 2 Week 8 — wired into Project 2's CI; second downstream user
- [ ] Phase 3 Week 12 — Show HN posted, blog post live, 50+ stars goal
- [ ] One upstream PR merged
- [ ] mkdocs site deployed
- [ ] At least one external contributor (issue or PR)
- [ ] Used in Project 5 (Phase 4) — third dogfood user
