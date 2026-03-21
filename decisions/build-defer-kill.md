# Decision Record: Build / Defer / Kill

> Results of a 10-agent cross-model design review conducted 2026-03-21.

## Context

8 multi-agent orchestration patterns were proposed. A 10-agent panel across 3 model providers (Claude Opus, GPT-5.4, Gemini) evaluated each pattern across two phases.

## Phase 1: Five Parallel Experts

| Expert | Model | Role |
|--------|-------|------|
| System Architect | Claude Opus | Infrastructure mapping, composability, state management |
| Prompt Engineer | Claude Opus | Agent brief design, convergence criteria, synthesis prompts |
| Skeptic | Claude Opus | Cost analysis, feasibility challenge, priority ordering |
| External Expert | GPT-5.4 (Codex CLI) | Framework comparison (AutoGen/CrewAI/LangGraph/CAMEL/MetaGPT) |
| External Expert | Gemini 2.5 (Gemini CLI) | Research backing, hardware constraints, missing patterns |

## Phase 2: Four More Experts + Re-run

| Expert | Model | Role |
|--------|-------|------|
| Meta-Skeptic | Claude Opus | Audited the Phase 1 process itself |
| Use-Case Mapper | Claude Opus | Mapped patterns to 6 real projects |
| MVE Designer | Claude Opus | Stripped proposal to minimum viable |
| DX Designer | Claude Opus | Designed invocation interface and error UX |
| External Expert (re-run) | GPT-5.4 | Completed the lost Phase 1 output with web research |

## Decisions

### BUILD

| Pattern | Why |
|---------|-----|
| **Panel** | Already in use ad-hoc (8-expert audit, 3-expert strategy review). 4/6 projects have demand. Standardized template eliminates per-use redesign. Subsumes lightweight debate and council patterns. |
| **Tournament Lite** | 4/6 projects have demand for cross-model verification. Strongest hallucination reduction pattern. Blind judging prevents model-preference bias. |

### DEFER

| Pattern | Trigger Condition |
|---------|------------------|
| **Adversarial Debate** | Panel single-round produces a documented bad decision |
| **Advisory Council** (persistent) | Same expert set reused 5+ times in one project |
| **Async Pipeline** (formalization) | Panel/Tournament used successfully 5+ times |

### KILL

| Pattern | Why |
|---------|-----|
| **Blackboard Convergence** | 0/6 projects had demand. Shared mutable prose fails. Existing orchestrator-controlled blackboard is sufficient. |
| **Recursive Exploration** | 0/6 projects had demand. Cost 4-20x. Violates single-layer subagent rule with insufficient justification. |

### RECLASSIFIED

| Pattern | New Status | Why |
|---------|-----------|-----|
| **Watchdog + Worker** | Infrastructure (not a mode) | It's a reliability primitive, not a collaboration pattern |
| **Cross-Time Relay** | Shared primitive | It's persistence + scheduling, not a standalone mode |

## Key Findings

### Cross-expert consensus (all 10 agreed)

- Pipeline and Watchdog are the most proven patterns across all frameworks
- 2-4 agents + supervisor beats 6-10 agents almost always
- Context engineering matters more than agent count

### Key disagreements resolved

| Topic | Disagreement | Resolution |
|-------|-------------|------------|
| Debate | Architect/Prompt Engineer designed it in detail; Skeptic said don't build | **Skeptic wins**: no current failure case justifies the cost. Panel covers 80%. |
| Advisory Council | Gemini recommended building it; Skeptic called it a trap | **Skeptic wins**: infrastructure already exists (context files + checkpoint). Council = better role prompts, not new systems. |
| Panel vs 8 separate modes | MVE Designer proposed building only Panel | **MVE wins**: Panel subsumes lightweight versions of Debate, Council, and Tournament. |

### The Meta-Skeptic's key insight

> "This entire 10-agent design discussion was conducted using Claude Code's 'limited' single-round subagent model — and it produced substantive results. The gap between single-round and multi-round is ~30% quality at ~300% cost. For most tasks, that tradeoff favors simplicity."

## Process Assessment (Meta-Skeptic)

**Grade: C+**

Strengths:
- Good expert role diversity
- Parallel execution efficient

Weaknesses:
- Experts operated without usage history data
- No iteration between experts (Skeptic never rebutted Architect)
- One expert's output was lost (Cortex Phase 1)
- Experts weren't given the platform's failure history

These process flaws are themselves evidence for why multi-round debate might improve quality — but at significant cost.
