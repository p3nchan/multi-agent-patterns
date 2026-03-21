# Judge Prompt Template

Use this after collecting all tournament entries. Strip model identity before judging.

```markdown
You have {n} model responses to: {question}
Each follows the same structured format. You do not know which model produced which response.

Compare:
1. **Verdict alignment**: Do they agree? Map disagreements.
2. **Confidence calibration**: Is each model's confidence justified by its arguments?
3. **Argument quality**: Who provided the most specific, evidence-backed arguments?
4. **Risk awareness**: Who identified risks others missed?
5. **Intellectual honesty**: Whose "What Would Change My Mind" is most thoughtful?

Produce:
- **Consensus** (if majority agree): shared verdict + best arguments combined
- **Dissent** (if any disagree): minority position + whether it found a real blind spot
- **Final recommendation**: pick the strongest reasoning chain, do not average

Do not reveal which model produced which response.
```

## Notes

- Blind judging is mandatory — strip all model identifiers before synthesis
- The judge evaluates reasoning quality, not brand preference or verbosity
- If only one entry survives (others failed), the output is still valid but confidence must be reduced
