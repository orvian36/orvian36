# Paper Implementation

> The hardest of the three paths, and the highest-signal. A senior engineer who implements 12 papers in a year and writes about each one looks fundamentally different from one who only writes opinion pieces.

## Why this is the hard leg

| Skill | Why it's hard | What it earns you |
|---|---|---|
| **Reading research papers efficiently** | Most engineers freeze on dense math | Distinguishes you from tutorial-completers |
| **Reproducing results** | Papers underspecify; details are missing | Demonstrates real depth |
| **Running on your own data** | Most papers test on standard benchmarks; your data is the moat | This is where the *post* lives |
| **Writing honestly about failures** | Cultural pressure to claim wins | Honest failures get more reads than fake wins |

The leverage: **your data + the paper's idea + your honest measurement = something nobody else can write.**

## Your 5 niches — paper sources per niche

You said your niches are **RAG, LLM, agentic AI, evals, security**. Here's where to find papers in each.

### Universal sources (cover all niches)

| Source | Why |
|---|---|
| **[arXiv](https://arxiv.org)** — `cs.CL`, `cs.IR`, `cs.AI`, `cs.LG`, `cs.CR` | Primary source |
| **[arxiv-sanity-lite](https://arxiv-sanity-lite.com)** — Karpathy's filter | Personal-relevance ranking |
| **[Hugging Face Daily Papers](https://huggingface.co/papers)** | Community-vetted, recent, discussion threads |
| **[Papers with Code](https://paperswithcode.com)** | Has implementations / leaderboard |
| **[alphaXiv](https://www.alphaxiv.org)** | Comments + threads on papers |
| **[Semantic Scholar](https://www.semanticscholar.org)** | Citation graphs to find follow-ups |
| **[Anthropic Research](https://www.anthropic.com/research)** | Frontier-lab papers, often deeply engineering-relevant |
| **[OpenAI Research](https://openai.com/research)** | Same |
| **[Google DeepMind Publications](https://deepmind.google/research/)** | Same |
| **[Meta AI Research](https://ai.meta.com/research/)** | Strong on RAG and small models |

### Conference proceedings to watch

| Conference | When | Niche fit |
|---|---|---|
| **NeurIPS** | Dec | LLM, agents, retrieval |
| **ICLR** | May | LLM, training, evaluation |
| **ACL / EMNLP / Findings** | Jul / Nov | RAG, NLP, multilingual |
| **EMNLP Industry Track** | Nov | Production-relevant |
| **SIGIR** | Jul | Pure retrieval; very relevant for your niche |
| **AAAI** | Feb | Agentic systems |
| **CoLM** | Oct | LLM-specific, newer venue |
| **AI Engineering Summit** (industry) | Mar/Oct | Not papers but talk recordings |

### Niche-specific sources

**RAG**
- [Ragas team's reading list](https://docs.ragas.io)
- [LlamaIndex blog](https://www.llamaindex.ai/blog) — practical RAG
- [Vespa blog](https://blog.vespa.ai) — retrieval depth
- [Pinecone learn](https://www.pinecone.io/learn/) — even if you don't use Pinecone, their explainers are good

**LLM (architecture / training)**
- [EleutherAI blog](https://www.eleuther.ai)
- [Together AI research](https://www.together.ai/research)
- [Mistral / Cohere / Stability research pages](https://mistral.ai/news/)
- [Sebastian Raschka's papers digest](https://magazine.sebastianraschka.com)

**Agentic AI**
- [LangChain State of Agents](https://www.langchain.com/state-of-agent-engineering) — quarterly market view
- [Anthropic agents posts](https://www.anthropic.com/research)
- [Princeton SWE-Bench / agent benchmarks](https://www.swebench.com)
- [Microsoft AutoGen research](https://microsoft.github.io/autogen/)
- [LlamaIndex agents blog](https://www.llamaindex.ai/blog)

**Evals**
- [Confident AI blog](https://www.confident-ai.com/blog) — DeepEval team
- [Arize AI blog](https://arize.com/blog/) — Phoenix team
- [HELM (Stanford)](https://crfm.stanford.edu/helm/) and the BIG-bench papers
- [LMSYS Chatbot Arena papers](https://lmsys.org)
- [Anthropic alignment research](https://www.anthropic.com/research) — evals appear in many

**Security**
- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [Lakera AI blog](https://www.lakera.ai/blog)
- [Palo Alto Networks Unit 42](https://unit42.paloaltonetworks.com)
- arXiv: `cs.CR` cross-listed with `cs.CL` and `cs.AI`
- [Adversarial ML community](https://adversarial-ml.github.io)

## How to read a paper efficiently (3-pass technique)

Adapted from S. Keshav's classic "How to Read a Paper":

### Pass 1: 10 minutes — survival pass

- Read title + abstract + intro + conclusion + figure captions only
- Ask: **what problem does it solve, and is the claim plausible?**
- Decide: **continue, or skip?**

### Pass 2: 30 minutes — comprehension pass

- Read all body sections; skim equations; study figures and tables carefully
- Note all references you don't know — these are your future reading list
- Ask: **what's the key idea? Can I state it in 3 sentences?**

### Pass 3: 2–4 hours — implementation pass

- Re-read methods section line by line
- Reconstruct the algorithm on paper
- Note every place where the paper is vague — these will bite you in implementation
- Look at the code (if released) for clues to the missing details
- Ask: **could I implement this without contacting the authors?**

If you can't finish Pass 3, the paper is either above your level or under-specified. Either way, pick a different paper for #1.

## Picking paper #1 — criteria

| Criterion | Why it matters |
|---|---|
| **Recency: last 3–6 months** | Relevant; community still cares; not yet over-replicated |
| **Niche fit** | RAG / LLM / agents / evals / security — your 5 |
| **No good public implementation, OR a weak one** | Your impl has a reason to exist |
| **Scope: fits in 2-week timebox** | No multi-GPU pretraining, no proprietary data |
| **Your data adds something** | Bilingual / Chinese / legal / on-device angle is your moat |
| **The paper has runnable claims** | Some papers are pure theory — skip those |

### Anti-criteria (don't pick these)

- ❌ Papers requiring 8× A100 multi-day training runs
- ❌ Papers on closed datasets (proprietary corpora you can't access)
- ❌ Pure theoretical / convergence-proof papers without an experiment
- ❌ Papers already implemented well by Hugging Face / official repos (you'd just be reproducing)
- ❌ Papers from > 12 months ago in fast-moving areas

## The 2-week implementation timebox (day-by-day)

Treat this as a sprint. Drift will eat you alive.

### Days 1–2: Read + plan
- Pass 1, then Pass 2 of the paper
- Sketch the implementation as pseudocode
- Open a fresh repo with the paper's title in the name
- Write a `PLAN.md` listing the components

### Days 3–5: Build the core
- Implement the algorithm; resist optimization
- Get the simplest configuration working end-to-end
- Don't worry about full reproduction yet

### Days 6–9: Reproduce on the paper's benchmark
- Run on whatever public dataset the paper used
- Compare your numbers to the paper's
- If you're > 10% off: debug. Common culprits: tokenization, sequence length, eval split, hyperparameters
- **If you can't reproduce, document it carefully — this is often the most interesting post**

### Days 10–11: Run on YOUR data
- This is the post's reason to exist
- Bilingual legal corpus / on-device hardware / your hybrid-search KB — whatever angle fits the paper
- Run an ablation: full method vs. baseline vs. a missing component

### Days 12–13: Write up
- Outline: paper summary → why I picked it → implementation gotchas → reproduction notes → MY ablation → when it works vs fails
- Don't recap the paper; link to it
- Lead with what *you* added or measured
- Include the failure cases honestly

### Day 14: Publish + share
- Cross-post per `01-engineering-writing.md`
- Share with the paper's authors on X / email — many will retweet a thoughtful implementation
- File any bugs you found in upstream repos

## Writing up the implementation — the post anatomy

Different from a build write-up. Structure:

```
1. WHAT THE PAPER CLAIMS (1 paragraph)
   - Link to arxiv
   - One-sentence summary of the contribution

2. WHY I PICKED IT (1 paragraph)
   - Your motivation; how it ties to your niche
   - This is the personal-stake hook

3. THE KEY IDEA IN PLAIN ENGLISH (2-3 paragraphs)
   - No equations unless they're load-bearing
   - Use one good diagram

4. THE IMPLEMENTATION GOTCHAS (substance section)
   - What the paper was vague on
   - Where you went wrong before getting it right
   - Code excerpts (real, runnable)

5. REPRODUCTION NOTES
   - Did the numbers match? If yes/no, why?
   - Hardware / wall-clock / cost actually used

6. MY ABLATION ON MY DATA (substance section)
   - Numbers on your bilingual / domain / on-device setup
   - Side-by-side table vs the paper's baseline

7. WHEN IT WORKS / WHEN IT FAILS
   - Honest tradeoffs
   - The failure cases are often more memorable than the wins

8. WHAT'S NEXT
   - Open questions
   - Link to your repo
```

## When the paper doesn't reproduce

This happens 30–40% of the time on real papers. **Document it carefully.** Three rules:

1. **Triple-check first.** Most "didn't reproduce" turns out to be a tokenizer or eval-split issue. Burn 2 more days verifying.
2. **Be respectful.** Frame as "I couldn't reproduce; here are the configurations I tried; possibly I'm missing something." Not "this paper is fake."
3. **Email the authors.** Many reply. Some have follow-up code that wasn't published.

The "honest failed reproduction" post is one of the most-engaged-with kinds of writing. It's also rare — that's why it works.

## Specific paper candidates by niche (May 2026 sampler)

These are starting points. Run them through the picking criteria; some will be obsoleted by newer work — verify recency before picking.

### RAG
- **CRAG (Corrective RAG)** — Yan et al. 2024 — adaptive retrieval with self-correction. Tractable. Likely already has implementations; you'd add bilingual angle.
- **Self-RAG** — Asai et al. 2023 — adaptive retrieval. Older; good candidate to revisit with newer base models.
- **LongRAG** — 2024 — long-context vs many-shot retrieval tradeoffs.
- **HippoRAG / HippoRAG 2** — Gutiérrez et al. — hippocampus-inspired memory for RAG.
- **GraphRAG** (Microsoft, 2024) — entity graphs for global queries.
- **RAFT** — Berkeley, 2024 — retrieval-aware fine-tuning.

### LLM
- **DPO / ORPO improvements** — many recent — alignment without RM.
- **DoRA** — Liu et al. 2024 — weight-decomposed LoRA. Tractable; ties to Project 5.
- **MoE distillation papers** — frontier-lab releases.
- **Long-context recipes** — RULER benchmark + various position-encoding papers.

### Agentic AI
- **ReAct** — Yao et al. 2022 — old but foundational; still worth reimplementing with current models.
- **Reflexion** — Shinn et al. 2023 — reflection loops.
- **Voyager** — Wang et al. 2023 — open-ended agent learning.
- **SWE-agent** / **OpenHands** papers — recent autonomous coding agents.
- **AgentBench, GAIA, SWE-bench Verified** — benchmark papers; reimplementing the agents is the work.

### Evals
- **LLM-as-judge calibration papers** — many; pick one that includes biases analysis.
- **G-Eval** — Liu et al. 2023 — chain-of-thought judging.
- **Prometheus / Prometheus 2** — Kim et al. — fine-tuned eval models.
- **AlpacaEval / MT-Bench / Arena-Hard methodology papers** — the evals themselves.
- **HELM** subareas — Stanford CRFM.

### Security
- **Indirect prompt injection** — Greshake et al. 2023 — the foundational paper.
- **Universal adversarial suffixes** — Zou et al. 2023 — jailbreaks.
- **Spotlighting** — Hines et al. — a defense technique.
- **Constitutional AI / Constitutional Classifiers** — Anthropic.
- **OWASP LLM Top 10 references** — many production-focused recent papers.

For paper #1 specifically, **a RAG paper applied to your Chinese legal corpus is the highest-ROI choice** — biggest niche overlap, clearest "your data adds something" angle.

## Building the paper-impl portfolio over 12 months

Goal: 12 paper implementations in 12 months. Each lives in its own repo under a topic tag.

```
github.com/orvian36/
├── paper-crag-bilingual           ← CRAG on Chinese legal
├── paper-rerank-multilingual      ← rerank methods, multilingual
├── paper-dora-gemma2              ← DoRA on Gemma 2 2B
├── paper-react-with-mcp           ← ReAct over MCP tools
├── paper-prompt-injection-suite   ← prompt-injection defenses tested
├── paper-self-rag-replication
├── ...
```

Use a GitHub topic like `paper-implementation` so they cluster. Cross-link each from the corresponding blog post.

After ~6 implementations, you have a defensible "I read and reproduce papers" portfolio that puts you above 99% of mid-level candidates.

## Anti-patterns

| Anti-pattern | Symptom | Fix |
|---|---|---|
| **Picking too-recent papers** | Within last 2 weeks; dust hasn't settled; might be hyped | Stick to 3–12 months old |
| **Picking too-old papers** | > 2 years in a fast-moving area; already done by everyone | Match niche recency norms |
| **Skipping the ablation on your data** | Just re-running paper's benchmark | The post is the ablation — don't skip it |
| **Claiming you "beat" the paper** | When you didn't carefully control for everything | Compare like-for-like, then add caveats |
| **Trying to combine 3 papers in one impl** | Scope explosion; never ships | 1 paper at a time |
| **Treating it as research** | Writing for academic publication | You're writing engineering posts; different audience, different rigor norms |

## What month-6 success looks like

- 6 paper implementations shipped (1 per ~4 weeks accounting for everything else)
- Each has: a public repo + a blog post + numbers on your data
- 2 of them got noticed by the paper's authors (reshare on X, or email exchange)
- 1 of them found a real reproduction issue you flagged upstream

That puts you at a level of public technical signal that very few mid-level engineers achieve in a year, let alone 6 months.

## The single discipline that makes this work

**The 2-week timebox is non-negotiable.** Drift kills this path. If a paper isn't running by end of week 1, ship the partial result as a "here's where I got stuck" post. That's still content. Quitting the sprint is what kills the cadence.
