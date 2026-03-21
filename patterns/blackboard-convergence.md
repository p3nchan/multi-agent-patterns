# Blackboard Convergence

> Multiple agents read and write a shared document, progressively building consensus.

**Status**: Killed | **Cost**: 2-4x single agent | **Complexity**: High

---

## The Pattern

```
blackboard.md:
  ## Facts (confirmed)
  - Metric X: 12.5% (Agent-A, verified by Agent-B)

  ## Hypotheses (under debate)
  - "Strategy Y will outperform" -- A supports, C disagrees

  ## Action Items (consensus reached)
  - ...

Agents read blackboard --> propose amendments --> orchestrator merges --> repeat
```

## Why Killed

### Zero demand

Across 6 active projects analyzed during the design review, none needed multiple agents iteratively building consensus on a shared document. Existing orchestrator-controlled blackboards (where only the orchestrator writes) are sufficient.

### Structural problems

From GPT-5.4: "Shared mutable prose as blackboard fails. Use structured artifacts, status fields, and claim ledgers instead."

Specific risks:
- **Edit wars**: agents repeatedly overwrite each other's contributions
- **Asymmetric contribution**: one agent dominates, making it effectively single-author
- **No convergence guarantee**: without hard round limits, agents can oscillate indefinitely
- **Context bloat**: each agent re-reads the entire growing document every round

### The simpler alternative

A single agent that writes a draft, then re-reads it critically ("now review what you just wrote for gaps"), then revises. This self-critique loop captures ~80% of the benefit at a fraction of the complexity.

## When to Reconsider

If you have a task where:
- 3+ agents need to **incrementally build** a complex document (not just review it)
- The document structure is well-defined (schema, not prose)
- You can implement proper merge conflict resolution
- You're willing to enforce hard round limits (max 5)

Then Blackboard might be worth revisiting — but with structured fields, not free-form text.
