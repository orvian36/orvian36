# Engineering Writing

> Your primary identity. The other two paths (OSS PRs, paper implementations) exist to give every post something concrete to point at.

## The audience-first mental model

You are not writing "for the internet." You are writing for **one person** — a mid-to-senior engineer at a real AI company, 9pm on a Thursday, who saw your post linked in a Slack channel or on Hacker News. They have 90 seconds. They've read a hundred LLM blog posts. They are tired of:

- Bullet-pointed "top 10 AI tools"
- "Beginner's guide to RAG"
- Tutorial code copy-pasted from the docs
- Vague claims with no measurements

Imagine them. Write for them. **Every post.**

If a post wouldn't make that person stop scrolling, don't ship it.

## What to write — 5 post archetypes that consistently work

| # | Archetype | Example title |
|---|---|---|
| 1 | **Build write-up with numbers** | *"How I built an agentic due-diligence researcher on AWS for $0.10/brief"* |
| 2 | **Benchmark / measurement** | *"Hybrid search misses 42% of Discord questions on Tauri's docs — and the pgvector setup that doesn't"* |
| 3 | **Paper implementation + ablation** | *"I implemented Self-RAG on Chinese legal corpus and it lost to vanilla — here's why"* |
| 4 | **OSS launch / patch write-up** | *"Why your CI should fail when RAG faithfulness drops 3% — and the 50-line decorator I built"* |
| 5 | **Opinionated experience post** | *"Stop reaching for fine-tuning first. The escalation framework I use after 1 year shipping LLM systems."* |

### What NOT to write (deliberately skip)

- ❌ "Top N AI tools" listicles — saturated, recruiters dismiss
- ❌ "Beginner's guide to LangChain / RAG / agents" — saturated, no signal
- ❌ Marketing-tone breathless hype ("AI is changing everything!")
- ❌ Hot takes without evidence or numbers
- ❌ Posts that recap a paper without adding your own measurement
- ❌ Code dumps with no narrative

## The anatomy of a working post

Working posts have the same structure:

```
1. OPENING (≤ 100 words)
   - Counterintuitive claim, OR
   - Specific number, OR
   - Concrete user pain
   → Reader decides whether to continue HERE.

2. HOOK (1 short paragraph)
   - Why this, why now?
   - Connect to a recent event/release/conversation if possible.

3. BODY (60-80% of total length)
   - Problem → approach → measurement → mechanism
   - Numbers, screenshots, code excerpts (real, runnable)
   - Show failure cases honestly

4. WHAT YOU CAN STEAL (1 section)
   - Concrete things the reader can apply Monday morning
   - Links to your repo/PR/data

5. CLOSING (≤ 200 words)
   - Honest tradeoffs ("when this doesn't work")
   - What's next / open questions
   - Single CTA (subscribe, link to repo, "I'm open to remote AI roles")
```

### Length

- **Sweet spot: 1500–3500 words**. Long enough to have substance; short enough to read in one sitting.
- **Don't go > 4000 unless density is high**. A 5000-word post that could be 2500 will lose readers.
- **Don't go < 1000** unless it's a "TIL"-style observation. New writers don't earn the right to short posts.

### Opening — the most important 100 words

Three opening patterns that work (steal these):

**1. The specific-number opener.**
> *"Algolia DocSearch misses 42% of the questions developers ask in Tauri's Discord. I indexed Tauri's docs with pgvector + BM25 + Cohere Rerank, mined 150 real Discord queries as the eval set, and ran the benchmark. Here's the breakdown."*

**2. The contrarian opener.**
> *"Most production RAG advice is wrong for non-English corpora. After a year shipping a 500-page Chinese legal translation pipeline at Makebell, here's what changed when I stopped following the LangChain tutorials."*

**3. The escalation-failure opener.**
> *"I tried 4 ways to make Claude 3.5 consistent on legal terminology across a long document. Three of them failed. Here's what I learned about when small fine-tuned models actually win."*

### Closing — honest tradeoffs > grandstanding

The end of a senior-engineer post is *not* "and now AI will revolutionize everything." It's:

> *"This works at <10M vectors and Chinese-EN bilingual corpora. It does not work for Japanese — the tokenizer breaks BM25 weighting. I haven't tested on Bengali. Here's the failure case I'm still chasing: …"*

Honest tradeoffs are what senior practitioners share with you. Reproduce that voice.

## Distribution — where to publish

**Own your list. Don't outsource the relationship.**

| Tier | Platform | Why |
|---|---|---|
| **Primary** | **Substack** or **Beehiiv** | Owns email list; SEO; portable RSS |
| **Mirror 1** | **dev.to** | Free distribution to a real developer audience; tag well |
| **Mirror 2** | **LinkedIn** | Recruiter funnel; mirror summary + link, not full text |
| **Mirror 3** | **`habib36.dev/blog`** | SEO accumulates on your own domain |
| **Cross-post** | **Hacker News**, **r/MachineLearning**, **r/LocalLLaMA**, **X/Twitter** | One-time spike per post; HN front-page is the lottery ticket |

**Posting time**: Tuesday–Thursday, 9–11 AM US Eastern. That's when HN gets the highest engineering-engaged traffic.

**Titles matter more than you think**. Two titles for the same post can have 10× difference in clicks:
- ❌ "RAG benchmarking on Chinese data" (boring)
- ✅ "Hybrid search loses to dense on Chinese legal text — here's the eval that showed me why" (specific, numbers, has a story)

## Getting attention — the cold start

The first 10 posts get ~30 reads each from your network. That's normal. Don't take it personally.

What moves the needle in months 0–6:

1. **Comment substantively on one well-known voice per week.** Eugene Yan, Hamel Husain, Jason Liu, Simon Willison, Lilian Weng, Chip Huyen, Sebastian Raschka. Engineering specifics, not "great post!" One comment per week, for 6 months, on the *same* names. They start to recognize you.

2. **Reply to engineers in your niche on X.** When someone posts about RAG/evals/MCP and you have concrete experience to add — reply with the experience, not just an opinion. Same names, repeat exposure.

3. **Submit to curated newsletters / aggregators**:
   - [TLDR.tech](https://tldr.tech) (general)
   - [The Sequence](https://thesequence.substack.com) (ML)
   - [Ahead of AI](https://magazine.sebastianraschka.com) — Sebastian Raschka
   - [Latent Space](https://www.latent.space) — swyx & Alessio
   - [The Batch](https://www.deeplearning.ai/the-batch/) — Andrew Ng's
   - [Import AI](https://importai.substack.com) — Jack Clark
   - [Hugging Face Daily Papers](https://huggingface.co/papers)
   - [r/MachineLearning weekly digest](https://www.reddit.com/r/MachineLearning)

4. **Cross-post to dev.to, Hashnode, Medium** for SEO breadcrumbs.

5. **The one HN front-page post seeds everything.** It only takes one. Post a Show HN with a real repo, real numbers, vulnerable-but-confident voice. Optimize the title. Submit Tuesday-Thursday morning.

6. **Write something that one well-known voice will reshare.** This means: solving a problem they've publicly written about, with your own data. Hamel writes about evals — you write about eval gates in CI with your `evalgate` numbers. Jason writes about structured outputs — you write about structured output failures on bilingual data.

## Cadence and discipline

| Rule | Why |
|---|---|
| **One post every 2 weeks** | Sustainable; sets reader expectation; matches your build sprints |
| **Always ship — even imperfect** | The 7th post will be better than the 1st; the unpublished draft helps no one |
| **Edit ruthlessly: cut 30% from first draft** | Density wins |
| **Read aloud before publishing** | Catches voice and rhythm problems |
| **Schedule the next post before celebrating this one** | Momentum dies in the gap between |

## Voices to study (specific essays, not just blogs)

Read these *first*. Study structure, not opinions.

| Author | One essay to start with |
|---|---|
| **Eugene Yan** | [Patterns for Building LLM-Based Systems & Products](https://eugeneyan.com/writing/llm-patterns/) — masterclass in structure |
| **Hamel Husain** | [Your AI Product Needs Evals](https://hamel.dev/blog/posts/evals/) — opinionated, evidence-based |
| **Jason Liu** | [Pydantic is all you need](https://jxnl.co/writing/) — focused niche ownership |
| **Simon Willison** | [Anything from his weekly digest](https://simonwillison.net/) — the daily-cadence model |
| **Lilian Weng** | [LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/) — survey-paper-as-post |
| **Chip Huyen** | [Designing Machine Learning Systems](https://huyenchip.com/) excerpts — long-form engineering |
| **Sebastian Raschka** | [Ahead of AI](https://magazine.sebastianraschka.com/) — accessible deep dives |
| **Andrej Karpathy** | [Old neural-net posts](https://karpathy.github.io/) — narrative + intuition |
| **Anthropic engineering blog** | [Claude's Constitution](https://www.anthropic.com/news/claudes-constitution) — corporate-eng tone done well |

For each, pick one essay, **read it twice**. Once for content, once for structure. Note: opening, hook, section transitions, ending. You're studying the *shape* of writing that works.

## Anti-patterns to avoid

| Anti-pattern | Symptom | Fix |
|---|---|---|
| **Listicle hell** | "Top 10 X for 2026" | Replace with one post, one X, with measurements |
| **Saturated topic** | "Beginner's guide to RAG" | Pick a sub-niche with your data |
| **Marketing tone** | "Revolutionize", "game-changing", "unlock" | Delete. Read like a friend texting an engineering insight. |
| **No measurements** | "Improves quality significantly" | Replace with numbers or cut |
| **Hidden weakness** | Skip failure cases to look good | Lead with failures; they're more memorable than wins |
| **Cargo-cult structure** | TL;DR + intro + 5 H2s | Use structure that serves the post, not the template |
| **Bury the lede** | The number is in paragraph 8 | Lead with the number |

## 30-day startup checklist

- [ ] **Day 1**: Lock in identity statement, write it on `habib36.dev/about` and X bio
- [ ] **Day 2**: Set up Substack/Beehiiv with a clean visual identity (matches `habib36.dev`)
- [ ] **Day 3**: Set up dev.to + LinkedIn accounts under same identity
- [ ] **Day 4–7**: Read the 9 essays listed above; take notes on structure
- [ ] **Day 8–10**: Draft post #1 — pick whichever `/todos` artifact you'll ship first; outline a build write-up
- [ ] **Day 11–13**: Write post #1 (1500–3000 words)
- [ ] **Day 14**: Edit (cut 30%), read aloud, schedule
- [ ] **Day 15**: Publish + cross-post + share on X/LinkedIn
- [ ] **Day 16–28**: Start engagement loop — substantive comments on 2 voices/week
- [ ] **Day 29–30**: Plan posts #2 and #3; sketch next 6 weeks of writing calendar

## What "working" looks like at month 6

- 10–12 posts shipped
- 1 post that got >1000 reads (HN front page or known-voice share)
- 200–500 email subscribers
- 1 inbound DM from a recruiter at a real AI company
- 5+ engineers in your niche know your name on X

That's the realistic floor for someone who shipped consistently. Aim there. Anything more is upside.

## What "failing" looks like at month 6

- 4 posts shipped
- 0 posts above 200 reads
- 50 subscribers (most from friends)
- 0 inbound
- No engagement with known-voice community

If you hit this, **the problem is almost always cadence, not talent**. Diagnose: did you ship 1 per 2 weeks? If not, that's the cure. If yes — the posts are tutorial-tone or saturated-topic. Audit against the archetypes above.
