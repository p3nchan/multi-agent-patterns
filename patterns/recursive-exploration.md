# Recursive Exploration Tree

> Agents spawn sub-agents to explore sub-topics in tree structure.

**Status**: Killed | **Cost**: 4-20x single agent | **Complexity**: Very High

---

## The Pattern

```
Root Agent: "Research DeFi lending protocols"
  |-- spawn Child A: "Protocol X mechanisms"
  |-- spawn Child B: "Protocol Y risk model"
  |-- spawn Child C: "Emerging protocols"
        |-- spawn Grandchild C1: "Protocol M deep dive"
        |-- spawn Grandchild C2: "Protocol N deep dive"
```

## Why Killed

### Zero demand

No active project required tree-structured exploration. Research tasks were broad-but-flat (parallel experts), not depth-first recursive.

### Cost explosion

A tree of depth 3 with branching factor 2 = 7 agent sessions minimum. Branching factor 3 = 13 sessions. Without aggressive pruning, costs grow exponentially.

### Architectural violation

Most orchestration frameworks enforce single-layer subagents for safety. Allowing recursive spawn requires:
- Hard depth limit (recommend 3 max)
- Total spawn budget pool (not per-agent)
- Orchestrator-validated depth counter (agents cannot self-report depth)
- Deduplication (children may independently explore the same subtopic)

### The simpler alternative

Iterative deepening in a single session: agent explores topic A, decides if it needs deeper investigation, explores subtopic A1, then A2. Depth-first in one context window. Gets ~75% of the benefit without any recursive spawning.

## When to Reconsider

If you have a task where:
- The problem space is genuinely **tree-structured** (not just broad)
- Parallel breadth-first exploration would save significant wall-clock time
- You can implement strict depth limits and budget caps
- The value of the task justifies 10-20x cost

Academic research, patent landscape mapping, and comprehensive market analysis are the most likely use cases.
