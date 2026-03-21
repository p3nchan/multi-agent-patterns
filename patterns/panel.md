# Panel Mode

> 3-5 role-locked experts give independent opinions in parallel. The orchestrator synthesizes.

**Status**: Implemented | **Cost**: ~$1-3 per run | **Complexity**: Low

---

## When to Use

- The question needs **3+ distinct perspectives** to see all trade-offs
- The goal is finding **disagreements and blind spots**, not consensus
- A single agent tends to over-balance and hedge

## When NOT to Use

- Pure execution tasks with clear acceptance criteria
- Questions answerable by a single factual lookup
- Tasks that exceed your budget or urgency window

## Topology

```
Orchestrator
  |-- spawn Expert A (role: risk analyst)
  |-- spawn Expert B (role: growth strategist)    [parallel]
  |-- spawn Expert C (role: technical architect)
  |
  --> Orchestrator reads all outputs, synthesizes
```

- 1 orchestrator + 3-5 experts (default: 3)
- All experts spawn in parallel
- Synthesis is always done by the orchestrator, never delegated

## Protocol

| Step | Action | Notes |
|------|--------|-------|
| 1 | Define a single-sentence question | Don't bundle multiple decisions |
| 2 | Assign 3-5 expert roles | Roles should be complementary, not overlapping |
| 3 | Spawn all experts with the brief template | Each expert is locked to their role |
| 4 | Collect structured outputs | Each expert responds once (single round) |
| 5 | Orchestrator synthesizes | Preserve disagreement, don't average |

## Key Design Decisions

### Why single-round?

Multi-round debate costs 3-5x more but only improves quality ~30%. Single-round panels capture ~80% of the value. If single-round proves insufficient for a specific case (documented failure), escalate to [Adversarial Debate](adversarial-debate.md).

### Why role-locking matters

LLMs default to balanced, hedged answers. The brief must make staying in role **mandatory**:

> "You MUST argue from your role's perspective. Do not hedge. Do not try to be balanced."

Balance is the **orchestrator's job**, not the expert's. This is the single most important prompt design principle.

### Why orchestrator-only synthesis

Delegating synthesis to another agent loses the orchestrator's global context. The orchestrator has seen all outputs and knows the original question's stakes. No subagent has that full picture.

## Cost Profile

- Model: Opus-tier for strategy/judgment, Sonnet-tier for execution/audit
- Typical: N × model cost ≈ $1-3 for a 3-expert panel
- Timeout: 10 min per expert

## Failure Handling

| Scenario | Response |
|----------|----------|
| 1 expert fails | Retry once, then synthesize with available results + disclaimer |
| 2+ experts fail | Panel is degraded; fall back to single-agent or retry later |
| All experts agree | That's fine — you have consensus. Optionally spawn a devil's advocate in v2 |
| One expert contradicts their role | Orchestrator notices and either ignores or re-prompts |

## How Panel Subsumes Other Patterns

Panel is a general framework. Special cases:

| Configuration | Effect |
|--------------|--------|
| 2 experts with opposing roles | ≈ Lightweight debate |
| Experts from different model providers | ≈ Lightweight tournament |
| "Red Team" as one of the roles | ≈ Adversarial review |
| Domain experts (e.g., quant + risk + market) | ≈ Advisory council |

This is why Panel was built first — it covers ~80% of use cases that Debate, Council, and Tournament would serve.

## Example

**Question**: "Should we allocate 30% of lending capital to the 15-day tier?"

**Experts**:
1. **Conservative Lender**: Focus on safety, minimize drawdown risk
2. **Aggressive Quant**: Maximize APY, tolerate variance
3. **Risk Manager**: Focus on tail risk and regime transitions

Each expert reads the backtest data and market context, then responds with:
- Position (1-2 sentences)
- Key Evidence (3-5 bullets with sources)
- Risks/Blindspots from their perspective
- Confidence: high/medium/low

Orchestrator synthesizes: where they agree (high confidence), where they disagree (needs investigation), and a recommendation.

## Template

See [`templates/expert-brief.md`](../templates/expert-brief.md) and [`templates/synthesis-prompt.md`](../templates/synthesis-prompt.md).
