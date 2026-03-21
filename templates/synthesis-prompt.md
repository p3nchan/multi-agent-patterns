# Synthesis Prompt Template

Use this template after collecting all expert/model outputs.

```markdown
You have {n} expert responses on: {question}

For each expert, their role and output file path is listed below.
Read all outputs, then synthesize:

1. **Consensus**: What do all/most experts agree on?
2. **Disagreements**: Who disagrees, on what, with what evidence?
3. **Blind spots**: What did NO expert address?
4. **Recommendation**: Your verdict, incorporating the strongest arguments
5. **Confidence**: high/medium/low based on expert agreement level
6. If action needed: produce a Decision Card

Attribute insights to specific experts. Do not blend or average positions.
```

## Design Principles

1. **Attribute, don't blend**: "Expert-A identified X, Expert-B countered with Y" — not "various perspectives suggest..."
2. **Highlight disagreements**: If experts genuinely disagree, surface it as a decision point, don't paper it over
3. **Separate findings from recommendations**: Findings = what experts said (factual). Recommendations = what you conclude (judgmental). Keep them distinct.
