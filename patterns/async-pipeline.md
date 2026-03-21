# Async Pipeline

> Sequential stages with specialist handoff via shared workspace. Each stage reads the prior output and produces the next.

**Status**: Deferred (already used informally) | **Cost**: varies | **Complexity**: Low

---

## When to Use

- Task naturally decomposes into **sequential, independent stages**
- Each stage benefits from a different specialist prompt or model
- You need per-stage retry and error isolation

## Why Deferred

This pattern is already the most proven in production (CrewAI Flows, LangGraph workflows, AutoGen GraphFlow, MetaGPT SOPs). Many teams use it without formalizing it as a "mode." Formal template creation is deferred until Panel/Tournament adoption proves the template approach works.

## Topology

```
                  Timeline -->
Stage 1 (Researcher)  ########..............  --> findings.md
Stage 2 (Analyst)     ........########......  --> analysis.md
Stage 3 (Critic)      ................######  --> review.md

Orchestrator validates at each handoff point.
```

## Key Design Principle: Context Compression

The orchestrator — not the agents — summarizes between stages:

```
[Original Question: 100 tokens]
     |
  Stage 1: Researcher --> artifact: research.md (full output)
     |
  Orchestrator summarizes: 300 tokens
     |
  Stage 2: Analyst gets [Question: 100] + [Summary: 300] + [path to research.md]
     |
  Orchestrator summarizes: 400 tokens (cumulative)
     |
  Stage 3: Critic gets [Question: 100] + [Cumulative: 400] + [path to analysis.md]
```

Never let Stage 2 summarize Stage 1's output — it hasn't been prompted to preserve what Stage 3 needs.

## Failure Handling

| Scenario | Response |
|----------|----------|
| Stage fails | Retry that stage once. Per-stage TTL applies. |
| Upstream produces bad output | Validation gate between stages catches it before cascading |
| Downstream blocked | Per-stage timeout, not per-pipeline |

## When Formalized

Will include: stage definition schema, inter-stage validation gates, parallel branch support, and integration with watchdog monitoring.
