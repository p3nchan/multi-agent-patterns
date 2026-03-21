# Tournament Entry Template

Send this identical prompt to all model entrants in [Tournament Mode](../patterns/tournament.md).

```markdown
## Tournament Entry
- **Question**: {question}
- **Context**: {paths + summaries}

### Output Format (MANDATORY — do not deviate)
1. **Verdict**: One sentence recommendation/answer
2. **Confidence**: 1-10, where 10 = certain
3. **Key Arguments**: 3 bullets, <=2 sentences each
4. **Risks/Caveats**: 2 bullets
5. **What Would Change My Mind**: One sentence
```

## Notes

- Every entrant gets the **exact same** question and context bundle
- Keep the prompt neutral — do not leak preferred answers
- The structured section is the normalization layer; free-form reasoning can go in a separate optional section
