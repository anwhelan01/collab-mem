# The Gkisokay LLM Model Stack
## 12 models, 4 tiers — Hermes / OpenClaw / Claude Code / Codex
### A multi-model routing guide for agent builders — local to frontier
Source: Graeme @gkisokay, April 2026

---

## Tier 1 — FRONTIER (Complex reasoning, strategy, planning — external dev only)

| Model | Cost (per 1M) | Context | Why |
|-------|--------------|---------|-----|
| Claude Opus 4.6 | $5 in / $25 out | 1M | #1 agentic terminal coding. Catches own errors earlier than long ext. API only. |
| GPT-5.4 | $2.50 in / $15 out | 1.05M | Multi-hour autonomous execution with real planning. Premium when task complexity genuinely needs frontier. |
| Gemini 3.1 Pro | $2 in / $12 out | 1M | 7.5x cheaper than Opus on input. Native multimodal. Best price/intelligence at frontier. |

## Tier 2 — AGENT EXECUTION (Tool calls, long task chains, multi-step pipelines)

| Model | Cost (per 1M) | Context | Why |
|-------|--------------|---------|-----|
| MiniMax M2.7 | $0.30 in / $1.20 | 205K | Best price-to-agent-capability. 97% skill adherence. Critical for OpenClaw. $10 plan is absurdly good value. |
| Kimi K2.5 | $0.60 in / $3.00 | 256K | Best long-context stability for extended tool chains. Caution: 4+ more output tokens than peers — budget carefully. |
| DeepSeek V3.2 | $0.27 in / $0.41 | 164K | 80% of GPT-5.4 performance at 1/50th the cost. Best price-performance in this tier via OpenRouter. |

## Tier 3 — BALANCED (Content, code, research, day-to-day tasks)

| Model | Cost (per 1M) | Context | Why |
|-------|--------------|---------|-----|
| Claude Sonnet 4.6 | $3 in / $15 out | 1M | 98% of Opus coding at 1/5 the cost. API only — no $100/mo plan exists for this model. |
| GPT-5.4 mini | $0.75 in / $4.50 | 400K | 93.4% tool-call reliability. Smart enough to run your entire system. ChatGPT Plus = no API billing needed. |
| Qwen3.6 Plus | $0 in / $0 out | 1M | Best free model available. 1M context. Near-frontier coding. Use it always — free-until-proven-wrong choice. |
| Llama 4 Maverick | $0.19-$0.49 | 1M | Only serious open-weight option at this level. Self-host = zero marginal cost. Best for data sovereignty. |

## Tier 4 — LOCAL / $0 (Summaries, routing, classification, always-on loops)

| Model | Cost | Context | Why |
|-------|------|---------|-----|
| Qwen3.5-9B | $0 | 262K | Only model in the stack that costs nothing. Runs 24/7. Beats GPT-OSS-120B at 13x the size. Volume is unlimited. |
| Qwen3.5-27B | $0 | 262K | Step up from 9B. 32GB RAM. Notably better instruction following for complex micro tasks. |
| Gemma 4 (31B) | $0 | 256K | Dramatic leap over Gemma 3. Apache 2.0 for commercial use. Stronger reasoning than Qwen3.5-27B. Tradeoff: slower inference. |
| DeepSeek R1 distill | $0 | 128K | Best reasoning at $0 via OpenRouter free slot. MATH-500 — remarkable for a free model. MIT license. |
| GLM-4.5-Air | Low | 128K | Purpose-built for agent tool use and web browsing from scratch. Best OSS option for this specific job. |

---

## Key Principles
- The goal is to choose the right models that best fit your agents' needs for as little cost as possible.
- Do this and you can build a proficient agent that will never die.
- Route simple turns to Tier 4, execution to Tier 2, only escalate to Tier 1 when genuinely needed.

*Prices as of April 2026. Qwen3.5-9B benchmarks from live stack testing. GLM-4.5-Air agent rankings from SiliconFlow leaderboard. Benchmarks sourced from official model cards & Artificial Analysis.*
