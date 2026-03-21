# Adversarial Debate

> Two persistent agents argue for and against a proposition across multiple rounds. A judge synthesizes.

**Status**: Deferred | **Cost**: ~$2-8 per run | **Complexity**: Medium-High

---

## When to Use

- Binary decision with genuine trade-offs and high stakes
- Single-round Panel has produced a documented bad outcome
- You need agents to **respond to each other's specific claims**

## Why Deferred

Panel mode (single-round, 3-5 experts) captures ~80% of Debate's value at ~30% of the cost. Until a specific case demonstrates that single-round analysis led to a bad decision, the additional complexity and cost of multi-round debate is not justified.

**Build trigger**: Panel produces a documented bad decision — one where the losing side's argument, if properly challenged, would have changed the outcome.

## Topology

```
Orchestrator (Judge)
  |-- spawn Bull (advocate FOR)
  |-- spawn Bear (advocate AGAINST)
  |-- (optional) spawn Evidence Clerk (fact-checker)
  |
  |  Round 1: Independent opening briefs [parallel]
  |  Round 2: Cross-examination (each attacks the other's claims)
  |  Round 3: Steelman (each states the strongest opposing point)
  |
  --> Judge synthesis with fixed rubric
```

## Design Recommendations (from the review)

### Use 4 roles, not 2

Recommended by GPT-5.4 (citing debate research):
1. **Bull / Proposer** — argues FOR
2. **Bear / Skeptic** — argues AGAINST
3. **Evidence Clerk / Verifier** — fact-checks claims from both sides
4. **Judge / Synthesizer** — renders verdict

### Making agents actually disagree

LLMs default to agreement. Three mechanisms force genuine disagreement:

1. **Role-locked objective functions**: Each agent's success = arguing their side. Truth-finding is the judge's job.
2. **Asymmetric information priming**: Bull gets optimistic seed context, Bear gets risk-focused context
3. **Scoring rubric that penalizes hedging**: "Both sides have merit" scores zero

### Preventing degenerate equilibria

From GPT-5.4:
- Don't let both sides see each other's scratchpad before opening
- Use asymmetric prompts and sometimes different model providers
- Add an external verifier for citations and data
- **Reward concessions** — if "winning" is the sole objective, truth quality drops
- Force each side to state "what would change my mind"

### Round limits

Max 2 adversarial rounds + 1 steelman/revision round. More than that collapses into repetition. Stop early if no novel claims appear.

## Why Not Just "Debate Yourself"?

A single agent prompted to "argue both sides" gets ~70% of the benefit. The missing 30%:
- A single agent **won't genuinely challenge its own best argument**
- Two agents from different model providers have different blind spots
- The adversarial pressure forces evidence-backed claims, not just plausible-sounding ones

Whether that 30% gap justifies the cost depends on the stakes.

## Template

When built, will extend the [Expert Brief](../templates/expert-brief.md) with:
- Asymmetric seed framing per side
- Cross-examination round protocol
- Steelman prompt
- Judge rubric (evidence quality, not rhetoric)
