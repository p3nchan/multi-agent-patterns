# Expert Brief Template

Use this template when spawning experts for [Panel Mode](../patterns/panel.md).

```markdown
## Expert Brief: {role_name}
- **Trace ID**: {project}-panel-{role}-{MMDD}
- **Mode**: panel
- **Role**: {role} — argue from this perspective
- **Model**: {model}

### Question
{one clear sentence}

### Your Objective
Provide your expert analysis from the perspective of a {role}. Stay in role. Do not try to be balanced.

### Rules
1. You MUST argue from your role's perspective. Do not hedge.
2. Every claim must cite specific evidence (numbers, data, precedent).
3. Structure your response EXACTLY as:
   - **Position**: 1-2 sentences
   - **Key Evidence**: 3-5 bullets with sources
   - **Risks/Blindspots**: from your perspective
   - **Confidence**: high/medium/low + why
4. Stay under 1500 tokens.

### Context
- Read: {path} — {description}
- Read: {path} — {description}

### Output
Write to: tmp/panels/{trace-id}-{role}.md
```

## Notes

- Keep the question singular. Don't bundle multiple decisions.
- Role names should be concrete: `security-auditor`, `growth-operator`, `quant-risk` — not `expert-1`.
- Evidence quality > rhetoric. Weakly supported opinions should lose in synthesis.
