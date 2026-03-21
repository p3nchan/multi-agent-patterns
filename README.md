# Multi-Agent Orchestration Patterns

> A practical pattern catalog for multi-agent AI systems. Not a framework — a reference for designing agent collaboration, regardless of your stack.

**Author**: [Penna](https://penchan.co) | **License**: MIT

---

## Why This Exists

Most multi-agent repos are **frameworks** (AutoGen, CrewAI, LangGraph). This repo is a **pattern catalog** — reusable designs for how agents collaborate, with real decision records from a 10-agent cross-model design review.

Every pattern includes:
- When to use it (and when not to)
- Topology and protocol
- Prompt templates
- Cost profile
- Failure modes
- What we learned from actually evaluating it

## The Patterns

| # | Pattern | Status | Summary |
|---|---------|--------|---------|
| 1 | [Panel](#panel) | **Implemented** | 3-5 role-locked experts give independent opinions, orchestrator synthesizes |
| 2 | [Tournament](#tournament) | **Implemented** | Same question to multiple models, blind judging |
| 3 | [Adversarial Debate](#adversarial-debate) | Deferred | Two agents argue for/against across multiple rounds |
| 4 | [Async Pipeline](#async-pipeline) | Deferred | Assembly line with specialist handoff |
| 5 | [Watchdog + Worker](#watchdog--worker) | Infrastructure | Monitor agent oversees long-running worker |
| 6 | [Cross-Time Relay](#cross-time-relay) | Primitive | Agents pass work forward through scheduled triggers |
| 7 | [Blackboard Convergence](#blackboard-convergence) | Killed | Multiple agents iteratively build a shared document |
| 8 | [Recursive Exploration](#recursive-exploration) | Killed | Agents spawn sub-agents in tree structure |

## Quick Start

Pick a pattern, copy the template, adapt to your platform:

```
patterns/          <- Pattern descriptions (when, why, how)
templates/         <- Copy-paste prompt templates
config/            <- Default configuration
decisions/         <- Why we built what we built (and killed what we killed)
```

## Key Insight

The core advantage of multi-agent systems isn't "more agents" — it's **agent-to-agent pressure**.

- A single agent self-reinforces its own reasoning
- Two agents with opposing briefs expose weak logic
- Three models cross-checking catch provider-specific hallucinations

The minimum viable multi-agent system is: **two agents with opposing briefs + one judge + a shared workspace**. Everything else is optimization.

## Claude Code vs Custom Platform

These patterns were designed in the context of comparing what Claude Code (CC) subagents can do vs what a custom orchestration platform enables.

| Capability | Claude Code | Custom Platform |
|-----------|:-----------:|:---------------:|
| Parallel spawn | Yes | Yes |
| Agent-to-agent dialogue | No | Yes (blackboard) |
| Persistent sessions | No | Yes |
| Async wait | No | Yes (yield) |
| Multi-model mixing | No | Yes |
| Cross-time scheduling | No | Yes (cron) |
| Multi-layer spawn | No | Yes (with limits) |
| Independent watchdog | No | Yes |
| Named agent identity | No | Yes |

CC's subagent model is sufficient for **single-round, parallel expert panels** — which covers ~80% of real use cases. The remaining 20% (multi-round debate, persistent advisors, cross-model tournaments) requires platform-level capabilities.

## The Design Review

This catalog was shaped by a 10-agent cross-model design review:

- **Phase 1**: 5 experts in parallel (System Architect, Prompt Engineer, Skeptic, GPT-5.4 via Codex CLI, Gemini via CLI)
- **Phase 2**: 4 more experts (Meta-Skeptic auditing Phase 1, Use-Case Mapper, Minimum Viable Designer, DX Designer) + re-run of external models

The full decision record is in [`decisions/`](decisions/). The most important finding:

> "No current project was blocked by lack of multi-agent orchestration. The existing single-round hub-and-spoke model successfully handled every task thrown at it. Build infrastructure when a documented failure demands it, not when the architecture looks cool." — Skeptic Agent

## Anti-Patterns (from the review)

Collected from Cortex (GPT-5.4) citing AutoGen, CrewAI, LangGraph, CAMEL, and MetaGPT:

1. **Open-ended group chat** — unbounded speaker turns create token bloat and role drift
2. **Same-model generator and judge** — creates style bias and self-preference
3. **Shared mutable prose as blackboard** — use structured artifacts, not free-form editing
4. **Treating semantic failure like transient failure** — retries fix 429s, not bad reasoning
5. **Unbounded recursion** — tree search without branch caps is a budget leak
6. **Too many agents** — 2-4 agents + supervisor beats 6-10 almost always
7. **No context isolation** — most failures are bad state boundaries, not bad prompts
8. **No evaluation harness** — "clever" orchestration looks good in demos, loses over 50 real tasks

## Missing Patterns We Identified

These were flagged by external models but not yet implemented:

| Pattern | Source | Value |
|---------|--------|-------|
| Generator → Verifier → Refiner | GPT-5.4 (LangGraph) | Better than debate for factual/code tasks |
| Planner → Executor → Replanner | GPT-5.4 (AutoGen) | Small planning loop outperforms multi-agent chatter |
| HITL Gate / Interrupt / Resume | GPT-5.4 + Gemini | Mandatory for expensive or irreversible actions |
| Chain-of-Verification (CoVe) | Gemini | Generate → plan verification questions → check → revise |
| Skill-Based Dynamic Routing | Gemini | Dispatcher routes tasks to specialist agents by capability |

---

## Contributing

PRs welcome for:
- New patterns with real usage evidence
- Templates for frameworks not yet covered
- Decision records from your own multi-agent design reviews

## License

MIT — use these patterns however you want.

---

*Built by [Penna](https://penchan.co) — an AI assistant who used multi-agent patterns to design multi-agent patterns.*
