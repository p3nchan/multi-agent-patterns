# Cross-Time Relay

> Agents pass work forward through time via scheduled triggers. No two agents coexist.

**Status**: Primitive (not a standalone mode) | **Cost**: ~$0.05-0.50/handoff | **Complexity**: Low

---

## When to Use

- Work that spans **multiple sessions or days**
- Tasks that benefit from "sleeping on it" (waiting for data to settle, market close, etc.)
- Overnight batch processing where no human is available

## Why It's a Primitive, Not a Mode

Cross-Time Relay is **persistence + scheduling**, not a collaboration pattern. GPT-5.4 noted: "LangGraph threads/checkpoints and AutoGen save/load state make this practical — treat it as shared infrastructure, not a flashy standalone mode."

If your platform has cron + checkpoint files, you already have relay capability.

## Topology

```
Day 1 15:00  Agent A --> writes findings.md + relay-note.md
Day 1 23:00  Cron triggers Agent B --> reads relay-note.md --> continues work
Day 2 09:00  Operator arrives --> reads synthesized results
```

## Key Design Principle: Structured Handoff

The departing agent writes a structured relay note, not free-form:

```markdown
## Relay Handoff
- **Conclusion so far**: [2-3 sentences]
- **Open questions**: [numbered list]
- **Key artifacts**: [file paths]
- **What NOT to re-investigate**: [already ruled out, with reasons]
- **Suggested next step**: [specific action]
```

The "What NOT to re-investigate" field is crucial — without it, the next agent wastes half its budget re-deriving what the previous agent already explored.

## Failure Handling

| Scenario | Response |
|----------|----------|
| Relay state file missing/corrupt | Alert operator, do not proceed blindly |
| Cron skips a trigger (machine sleeping) | Check expected_next_run timestamp; if gap > 2x interval, log and decide |
| Agent produces incomplete handoff | Next agent should flag the gap, not silently continue |
