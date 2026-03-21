# Tournament Lite

> Same question sent to multiple model providers. Blind judging. Pick the strongest reasoning chain.

**Status**: Implemented | **Cost**: ~$1-3 per run | **Complexity**: Low

---

## When to Use

- A **single-verdict decision** where you want to compare reasoning quality across models
- Capital allocation, architectural choices, irreversible actions
- You suspect a single model might hallucinate on this specific topic

## When NOT to Use

- Routine questions with low stakes
- Creative/brainstorming tasks (use Panel with role diversity instead)
- Tasks where speed matters more than accuracy

## Topology

```
Orchestrator
  |-- send identical prompt to Model A (e.g., Claude Opus)
  |-- send identical prompt to Model B (e.g., GPT-5)      [parallel]
  |-- send identical prompt to Model C (e.g., Gemini Pro)
  |
  --> Orchestrator judges blind (model identity stripped)
```

- 2-3 model entrants (default: 2)
- All entrants receive **identical prompt and context**
- Judge does not know which response came from which model

## Protocol

| Step | Action | Notes |
|------|--------|-------|
| 1 | Write a single question + unified context bundle | Do not hint at preferred answers |
| 2 | Send identical prompt to all models | Forced structured output format |
| 3 | Collect responses, strip model identity | Blind judging is mandatory |
| 4 | Judge compares verdict, confidence, argument quality | Don't average — pick strongest chain |
| 5 | Produce consensus/dissent/final recommendation | Preserve minority positions if insightful |

## Why This Reduces Hallucination

Different models hallucinate differently. Claude might over-hedge, GPT might over-commit, Gemini might miss edge cases. When all three agree, confidence is high. When they disagree, the disagreement itself is signal — it points to where the truth is uncertain.

Cross-model verification catches errors that same-model debate cannot.

## Key Design Decisions

### Why blind judging?

Without blinding, the judge develops preferences for familiar model styles (verbosity, formatting, hedging patterns). Blinding forces evaluation of **reasoning quality**, not brand.

### Why 2 models, not 3+?

Two models from different providers already catch most provider-specific hallucinations. The third model adds ~20% marginal value at +50% cost. Default to 2, upgrade to 3 for high-stakes decisions.

### Why structured output?

Without forced structure, comparing outputs is apples-to-oranges. The mandatory schema normalizes responses:

1. **Verdict**: One sentence
2. **Confidence**: 1-10
3. **Key Arguments**: 3 bullets
4. **Risks/Caveats**: 2 bullets
5. **What Would Change My Mind**: One sentence

The judge compares these fields directly across models.

### Why "pick strongest" instead of voting?

Majority voting works for multiple-choice. For complex analysis, the best answer might be the minority position. The judge evaluates reasoning chains, not tallies.

## Cost Profile

- Typical: N × model cost + judge = $1-3
- Default entrants: 2 (one Claude-family, one external)
- Timeout: 5 min per entrant

## Failure Handling

| Scenario | Response |
|----------|----------|
| 1 model fails | Proceed with remaining, note reduced confidence |
| 2+ models fail | Cancel tournament, do not produce pseudo-blind judgment |
| All models agree | High confidence result (but note: correlated errors are possible) |
| All models disagree | Flag as "high uncertainty," escalate to human or deeper investigation |

## Practical Tips

From GPT-5.4 (citing AutoGen, LangGraph):

- **Normalize the playing field**: same prompt, same tools, same retrieval corpus, same time budget
- **Score on task success, not style**: tests and validators beat LLM judging when available
- **Separate generation from judging**: never let a model judge its own output
- **Track metadata**: latency, cost, tool failures, and refusal rate alongside quality
- **Prefer top-2 synthesis over winner-takes-all**: combine best elements when possible

## Example

**Question**: "Given 22 paper trades (15W/2L/5P), should $200 real capital be deployed?"

- Model A (Claude Opus): analyzes win rate confidence intervals
- Model B (GPT-5.4 via Codex): runs expected value calculations
- Responses are anonymized as "Entry 1" and "Entry 2"
- Judge compares: verdict alignment, confidence calibration, risk awareness
- Final output: consensus view + any dissenting insights

## Templates

See [`templates/tournament-entry.md`](../templates/tournament-entry.md) and [`templates/judge-prompt.md`](../templates/judge-prompt.md).
