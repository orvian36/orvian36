# Project 5 — On-Device Legal Translator Assistant (Fine-Tuned Small LM)

> **Tagline**: A 2B-parameter bilingual legal terminology assistant that runs entirely on a translator's laptop, hits sub-200ms latency, costs $0/query, and beats Claude on domain terminology — built because cloud APIs aren't an option when client documents can't leave the building.

## The use case (the story you actually tell)

Bilingual legal translators working at law firms, in-house counsel teams, or regulated industries hit a wall in 2026:

- Clients hand them **NDAs, M&A contracts, court filings, and regulatory submissions** that **cannot legally be sent to Claude or GPT-5** under their engagement letters or under GDPR / Hong Kong PDPO / Bangladesh DPA / EU AI Act provisions.
- Existing CAT tools (Trados, MemoQ) ship translation memory but **don't generate fluent reformulations** or surface bilingual terminology suggestions in context.
- General-purpose offline LLMs (base Gemma 2B, base Qwen 1.5B) **don't know legal vocabulary** — they hallucinate or paraphrase legal terms into vernacular, which is a malpractice risk.
- The hardware constraint is real: a translator's laptop is **8–16 GB RAM, no dedicated GPU, often offline on a train or in a court antechamber**.

You build a small, fine-tuned model that solves this — and it ships as a real desktop tool (VS Code extension or Tauri app) translators can install today.

This is the story:

> "At Makebell I built a cloud translation pipeline hitting ~98% alignment on 500-page legal docs. But every one of our customers eventually asked the same question: *can this run without sending our documents to your servers?* I went home and built the on-device version — a tuned 2B model that runs on a laptop CPU, hits 200ms response time, and stays within 10–15% of Claude on legal terminology. Here are the numbers."

## Why fine-tune (defended explicitly)

Per [Kumar Gauraw's 2026 guide](https://www.gauraw.com/fine-tuning-llm-lora-dpo-guide-2026/), 80% of "we need fine-tuning" use cases should actually use RAG or prompting. You need to be the candidate who can defend *why this one is in the other 20%*:

| Alternative | Why it fails for this use case |
|---|---|
| **Prompt Claude/GPT** | Violates client confidentiality, compliance, and engagement letters. Cost at translator's volume (~2K queries/day) is unbearable for an individual practitioner. |
| **RAG over termbase** | Works for "lookup this exact phrase" but cannot produce **fluent reformulations** or **contextual bilingual completions**. RAG retrieves; it doesn't draft. |
| **Use Gemma 2B base model unchanged** | Doesn't know "boilerplate," "indemnification," "force majeure," 抵押 (mortgage/pledge), 不可抗力 (force majeure). It paraphrases legal terms into colloquial ones. Tested in eval — fails on ~40% of legal terminology. |
| **Use a 70B model locally** | Doesn't fit in the laptop RAM budget. Pulls 30W+ continuous, kills battery in 90 min. |

That's the corner where **a small, domain-tuned model is the right answer**. You're not competing with Claude on raw IQ. You're competing on **deploy where Claude can't go**.

## What you build

1. **Curate** ~2,000–4,000 EN↔ZH (matches your Makebell experience) **or** EN↔BN bilingual legal/financial pairs. Sources:
   - Hong Kong bilingual legislation corpus (public, legally usable)
   - UN parallel corpus, legal subset
   - Synthetic pairs from existing termbases (IATE, UN MULTI-TERM)
   - 200–500 hand-curated gold-quality pairs for the eval set
2. **Fine-tune** a small base model with **QLoRA** via **Unsloth** on a single Colab T4 or modest cloud GPU — total cost under $5.
3. **Quantize** to **GGUF q4_k_m** for `llama.cpp` / `ollama` (runs on laptop CPU).
4. **Compare** head-to-head against base model, Claude 3.7 Sonnet, GPT-5 on a 200-sample held-out set using Project 4's eval harness.
5. **Ship a real desktop tool**: a Tauri-packaged app or a VS Code extension that translators can install and use offline.
6. **Integrate** as a selectable "private mode" backend in Project 1's agent.

## Tech stack

| Layer | Tech | Why |
|---|---|---|
| Base model — primary | **Gemma 2 2B Instruct** (or **Gemma 3 1B/4B** if released) | Best Unsloth support, Google-backed, strong English baseline |
| Base model — alt for EN↔ZH | **Qwen 2.5 1.5B Instruct** or **Qwen 2.5 3B Instruct** | Strongest small open multilingual; ideal for Chinese |
| Base model — alt for EN↔BN | **Llama 3.2 3B** or **Qwen 2.5 3B** | Decent Bengali tokenization |
| Fine-tuning method | **QLoRA (4-bit)** via **Unsloth** | The 2026 default; 2× faster on consumer GPUs |
| Training framework | **Unsloth** primary, **Axolotl** YAML alt | Both worth namedropping |
| Training compute | **Colab T4 / Kaggle T4** (free) → **Modal A10G** ($0.50/hr) | Cost transparency is the point |
| Quantization | **GGUF q4_k_m** for CPU; **AWQ** if GPU available | Standard 2026 stack |
| Local serving | **llama.cpp** + **Ollama** for the demo | CPU-first |
| Cloud serving (optional) | **vLLM on Modal serverless** | If a JD asks for vLLM, you have it |
| Desktop app | **Tauri 2** + Next.js front-end | Cross-platform, ~5MB binary, ties to your existing Next.js skills |
| Alt distribution | **VS Code extension** wrapping the same model | Translators already live in editors |
| Experiment tracking | **Weights & Biases** (free) | Industry-standard |
| Eval | **Project 4 harness** + domain-specific metrics | Self-reuse |
| Dataset versioning | **HuggingFace Datasets** | Public, reproducible |

## Target metrics (what you achieve — these go in the README hero)

| Metric | Target | Why it matters |
|---|---|---|
| **Domain terminology accuracy vs. base model** | **+25–35%** on 200-sample legal eval | The headline win — *why* tune at all |
| **Domain terminology accuracy vs. Claude 3.7 Sonnet** | **Within 10–15%** | Honest framing: you don't beat Claude on IQ, you trade ~12% accuracy for compliance + offline + free |
| **Inference latency (laptop CPU, M1 / Ryzen 5)** | **< 200 ms** for a 100-token completion | Fast enough for editor autocomplete |
| **Inference latency vs Claude API (round-trip)** | **4–8× faster** | The "feels instant" demo moment |
| **Cost per query** | **$0** (vs ~$0.003 for Sonnet) | At 2K queries/day = $180/mo saved per translator |
| **Disk footprint** | **< 1 GB** (q4_k_m, 2B) | Fits in any laptop |
| **RAM at inference** | **< 2 GB** | Runs while Word + Trados are open |
| **Works fully offline** | Yes, demoed | Compliance story is real |
| **Training cost** | **< $5** | Proves QLoRA accessibility |
| **Reproducibility** | `make dataset && make train && make eval && make app` | Senior engineering discipline |
| **Published artifacts** | Model on HF Hub, dataset card, eval results, signed Tauri builds for macOS/Linux/Windows | Public-by-default |

## What recruiters will look for

Per [appscale's 2026 fine-tuning guide](https://appscale.blog/en/blog/llm-fine-tuning-lora-qlora-full-fine-tuning-compared-2026), the [Red Hat / Unsloth piece](https://developers.redhat.com/articles/2026/04/01/unsloth-and-training-hub-lightning-fast-lora-and-qlora-fine-tuning), and [Kumar Gauraw's "when to fine-tune" guide](https://www.gauraw.com/fine-tuning-llm-lora-dpo-guide-2026/):

### Tier 1 signals

- ✅ **A real use case stated up front** — "translators need offline" beats "I wanted to fine-tune"
- ✅ **A clear "why not RAG / why not prompting / why not 70B" defense** — shows escalation discipline
- ✅ **Honest head-to-head numbers** — base vs tuned vs Claude, with the losses called out
- ✅ **A real shipped app**, not just a HuggingFace card — translators can `brew install` or download a `.dmg`
- ✅ **Dataset card** — source, license, biases, sample count, eval split
- ✅ **Model card on HuggingFace** with intended use + limitations
- ✅ **Cost table** — training $ + per-query $ vs Claude/GPT, broken down for individual practitioner and 50-seat firm

### Tier 2 signals

- ✅ **DPO or ORPO follow-up** after SFT for instruction-following polish (stretch)
- ✅ **Integrated into Project 1's agent** as a "private mode" backend (privacy story across portfolio)
- ✅ **W&B project public-linked** — anyone can audit your training runs
- ✅ **Eval includes adversarial cases**: out-of-domain inputs, prompt-injection attempts, mixed-language inputs
- ✅ **Explicit failure-mode section** — "model loses on negation-heavy clauses, novel jurisdictions, post-2024 case law"
- ✅ **Telemetry-free distribution** — explicit "no data leaves your machine" in README + verified by network sandbox test

### Red flags

- ❌ "Fine-tuned beats GPT-5" headlines without an eval set — recruiters know
- ❌ No comparison to base model — can't separate tuning gain from base capability
- ❌ Training data with no provenance or license
- ❌ "Black-box" runs — no W&B logs, no training config committed
- ❌ Reaching for fine-tuning *before* showing you considered RAG and prompting (this is THE classic interview red flag)
- ❌ Leading your CV with this — it's a capstone showing judgment, not a flagship (Project 1 leads)

## The narrative arc (memorize this for interviews)

Walk an interviewer through this in 90 seconds:

> "I started by trying prompt engineering on Gemma 2B base — failed on ~40% of legal terminology, so prompting wasn't enough. I tried RAG over a termbase — that handled lookups but couldn't produce fluent reformulations, which translators actually need. I considered Claude with a system prompt — works great but violates client confidentiality on every M&A engagement. So I fine-tuned the same Gemma 2B on 3K legal pairs. Final eval: +28% domain accuracy over base, within 12% of Claude, runs at 180ms on my MacBook Air CPU, ships as a 600MB Tauri app, zero per-query cost. The escalation was prompt → RAG → fine-tune, in that order, with measurements at each step."

That is what a senior interviewer wants to hear.

## Folder structure

```
on-device-legal-translator/
├── README.md                       ← problem → solution → numbers → install
├── PROBLEM.md                      ← the use case, the constraints, the alternatives considered
├── MODEL_CARD.md                   ← HF-style model card
├── DATASET_CARD.md
├── EVALS.md                        ← methodology + held-out set + adversarial cases
├── data/
│   ├── raw/                        ← scraped legal corpora (excluded if too large)
│   ├── synthetic/                  ← generation scripts
│   └── processed/                  ← train/val/test splits
├── training/
│   ├── configs/                    ← Unsloth + Axolotl YAMLs
│   ├── train_gemma.py
│   ├── train_qwen.py               ← alt for EN↔ZH
│   └── dpo_polish.py               ← stretch: instruction-following polish
├── eval/
│   ├── benchmark.py                ← uses Project 4 harness
│   ├── compare_to_claude.py        ← honest head-to-head
│   ├── adversarial.py
│   └── results/                    ← committed CSVs + plots
├── serving/
│   ├── llama_cpp_local/            ← CPU serving for the desktop demo
│   ├── ollama_demo/                ← `ollama pull` instructions
│   └── modal_vllm/                 ← optional GPU serving
├── app/
│   ├── tauri/                      ← Tauri 2 desktop app (cross-platform)
│   └── vscode-extension/           ← VS Code extension wrapping same model
├── notebooks/
│   ├── 01-data-prep.ipynb
│   ├── 02-finetune-gemma2-2b.ipynb
│   ├── 03-eval-comparison.ipynb
│   └── 04-quantize-and-serve.ipynb
└── Makefile                        ← make dataset / train / eval / app
```

## Blog post outline

Title: **"A 2B model on my laptop beat Claude on legal terminology — by losing on everything else. When small models actually win."**

Sections:
1. The translator-shaped hole in 2026 LLMs (the user need)
2. Why I ruled out prompting, RAG, and cloud APIs (the escalation, with numbers at each step)
3. Picking a base model: Gemma 2 2B vs Qwen 2.5 1.5B vs Phi-3.5 mini (with reasoning)
4. The data: where 3K bilingual legal pairs actually come from, legally
5. QLoRA in $4: the Unsloth + Colab workflow
6. The eval: where the tuned model won, where it lost (with the embarrassing failure cases shown)
7. Shipping a real app: Tauri + llama.cpp packaging gotchas
8. When small models actually win — and when they don't

This is the *unique* post — most fine-tuning content is "look how good it is." The senior-engineer version is "here's exactly when this beats the alternatives and exactly when it doesn't." That tone gets cited by practitioners and recruiters notice.

## Stretch features

- **DPO follow-up** on instruction-following polish using ~500 preference pairs
- **Hot-swappable LoRA adapters** — same Gemma 2B base, separate adapters for EN↔ZH, EN↔BN, EN↔FR — show multi-domain serving
- **Continual pre-training** on raw bilingual legal text before SFT
- **Real-user evaluation**: 5 working translators try the Tauri app for a week, return survey + transcripts (massive credibility boost; takes 2 weeks)
- **WebAssembly build** via `llama.cpp` WASM — runs in-browser, no install at all
- **MCP server wrapping** — your tuned model exposed via Project 3's pattern, so Claude Desktop can call it for private-mode tasks

## Checklist

- [ ] Phase 4 Week 13 — Dataset curated + first Gemma 2 2B QLoRA run done + W&B logs public
- [ ] Phase 4 Week 14 — GGUF quantized + Ollama instructions + cost/latency comparison done
- [ ] Phase 4 Week 14 — Integrated into Project 1's agent as "private mode" backend
- [ ] Phase 4 Week 15 — Tauri app v0.1 (macOS at minimum) + model card + dataset card on HF Hub + blog post live
- [ ] Phase 4 Week 16 — Optional: DPO polish, VS Code extension, real-translator survey
- [ ] CV bullet drafted: "Shipped on-device legal translator (Gemma 2 2B fine-tuned via QLoRA): +28% domain accuracy over base, 180ms p95 on laptop CPU, $0/query — eliminates cloud-API compliance risk for sensitive client documents."
