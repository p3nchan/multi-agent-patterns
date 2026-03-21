# How We Used Multi-Agent Patterns to Design Multi-Agent Patterns

> A meta-record of the design review process itself.

## The Setup

We used a real multi-agent orchestration session — running on Claude Code's subagent model — to evaluate and design orchestration patterns for a more capable custom platform. The irony was intentional: testing the limits of single-round hub-and-spoke by using it to design something better.

## Phase 1: Five-Expert Parallel Panel

**Pattern used**: Panel (single-round, 5 experts, parallel)

```
Orchestrator (Claude Opus, main session)
  |-- Agent: System Architect (Claude Opus subagent)
  |-- Agent: Prompt Engineer (Claude Opus subagent)
  |-- Agent: Skeptic (Claude Opus subagent)
  |-- Agent: External Expert (GPT-5.4 via Codex CLI)
  |-- Agent: External Expert (Gemini via Gemini CLI)
```

This is a **cross-platform, multi-model panel** — 3 Claude subagents + 2 external models invoked via CLI. All ran in parallel. Total time: ~3 minutes for Claude subagents, ~5 minutes for external models.

### What worked
- Five distinct perspectives with genuine disagreements
- The Skeptic challenged the other four's enthusiasm
- External models (GPT, Gemini) brought framework-specific knowledge Claude didn't have

### What didn't work
- GPT-5.4 background task was lost (had to re-run)
- Gemini 2.5 Pro hit capacity limits (429 errors, fell back to Flash)
- No expert could challenge another's specific claims (hub-and-spoke limitation)

## Phase 2: Four More Experts + Re-run

**Pattern used**: Same panel pattern, but with the Phase 1 findings as context.

```
Orchestrator
  |-- Meta-Skeptic (audited Phase 1's process quality)
  |-- Use-Case Mapper (mapped patterns to real projects)
  |-- MVE Designer (stripped to minimum viable)
  |-- DX Designer (designed user-facing interface)
  |-- Cortex re-run (GPT-5.4, with Phase 1 context)
```

### What this added
- The Meta-Skeptic graded the Phase 1 process C+ and identified missing perspectives
- The Use-Case Mapper produced a demand heat map showing 2 patterns had zero demand
- The MVE Designer argued for building ONE thing, not eight
- The DX Designer produced actionable skill commands and auto-trigger heuristics

## What We Learned About the Process

### Single-round panels are surprisingly good

10 experts across 2 phases produced:
- A clear BUILD/DEFER/KILL decision for all 8 patterns
- Detailed template designs for the 2 built patterns
- A demand heat map across 6 real projects
- Auto-trigger heuristics and error handling specs

This is ~70-80% of what a full multi-round adversarial process would have produced, at ~20% of the cost.

### The missing 30%

What single-round couldn't do:
- The Skeptic never got to rebut the Architect's composability argument
- The Prompt Designer's scoring rubric was never tested against the Architect's mode definitions
- No expert refined their position based on another's challenge
- Contradictions were resolved by the orchestrator's judgment, not by expert dialogue

### The cost-quality tradeoff

| Approach | Quality | Cost | Time |
|----------|---------|------|------|
| Single agent analysis | ~60% | 1x | 2 min |
| 5-expert panel (Phase 1) | ~75% | 5x | 5 min |
| 10-expert panel (Phase 1+2) | ~80% | 10x | 10 min |
| Multi-round debate (estimated) | ~90% | 30x | 30 min |

For a design discussion, 80% quality was sufficient. For a $100K capital allocation decision, the extra 10% from multi-round debate might be worth 3x the cost.

## Reproducibility

Anyone can replicate this process:

1. Define your design question
2. Spawn 3-5 experts with distinct roles (at least one Skeptic)
3. If available, include external models via CLI for cross-model perspective
4. Synthesize Phase 1 results
5. Spawn Phase 2 experts with Phase 1 findings as context (at least one Meta-Skeptic)
6. Final synthesis → decisions

The templates in this repo were designed to make this repeatable without reinventing the wheel each time.
