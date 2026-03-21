# Watchdog + Worker

> A lightweight monitoring agent oversees a long-running worker agent.

**Status**: Infrastructure (not a mode) | **Cost**: ~$0.01-0.05/check | **Complexity**: Low

---

## When to Use

- Any task running **>30 minutes** that might silently hang
- Background data collection, batch processing, long-running API calls
- Tasks where silent failure = missed data or broken downstream

## Why It's Infrastructure, Not a Mode

Watchdog is a **reliability primitive**, not an orchestration pattern. It doesn't change how agents collaborate — it ensures they stay alive. It belongs in your platform's health-check layer, not in a "multi-agent mode" framework.

## Topology

```
Worker Agent: Executes long-running task
  |
  |--> writes progress to status file every N minutes
  |
Watchdog (lightweight model, every 10 min):
  - Is Worker alive? (check session liveness)
  - Making progress? (check status file timestamp)
  - Stuck? (>10min stale --> nudge)
  - Dead? (>20min --> kill + restart from checkpoint)
```

## Key Design Principle: Three States, Not Two

The watchdog must distinguish:
1. **No progress file** → Worker is dead
2. **Progress file stale** → Worker might be stuck (nudge first)
3. **Progress file recent** → Worker is alive

"Alive or dead" binary leads to false-positive kills.

## Cost

Use the cheapest available model for watchdog checks (e.g., Haiku, Flash). Checks are simple: read a timestamp, compare to now, decide action. $0.01-0.05 per check.

## Failure Handling

| Scenario | Response |
|----------|----------|
| Worker stale >10 min | Nudge (send a prompt to check if stuck) |
| Worker stale >20 min | Kill + restart from last checkpoint |
| Watchdog itself dies | Higher-level shell watchdog (cron, no LLM) catches this |
| False positive kill | Worker should write progress at least every 5 minutes to prevent this |
