# Discussion Document Template

*Part of **[technical-discussion](../SKILL.md)** | See also: **[meeting-assistant.md](meeting-assistant.md)** · **[guidelines.md](guidelines.md)***

---

Standard structure for `docs/specs/discussions/<topic-name>/discussion.md`. Each discussion gets its own directory with a single markdown file. DOCUMENT only - no plans or code.

## Template

```markdown
# Discussion: {Topic}

**Date**: YYYY-MM-DD
**Status**: Exploring | Deciding | Concluded

## Context

What this is about, why we're discussing it, the problem or opportunity, current state.

### References

- [Related spec or doc](link)
- [Prior discussion](link)

## Questions

- [ ] How should we handle X?
      - Sub-question about edge case
      - Context: brief note if needed
- [ ] What's the right approach for Y?
- [ ] Should Z be separate or combined with W?
      - Related: how does this affect A?
- [ ] ...

---

*Each question above gets its own section below. Check off as concluded.*

---

## How should we handle X?

### Context
Why this question matters, what's at stake.

### Options Considered
The approaches we looked at and their trade-offs.

### Journey
The back-and-forth exploration. What we initially thought. What changed our thinking. False paths - "We considered A but realised B because C." The "aha" moments. Small details that mattered.

### Decision
What we chose, why, the deciding factor, trade-offs accepted, confidence level.

---

## What's the right approach for Y?

*(Same structure: Context → Options → Journey → Decision)*

---

## Summary

### Key Insights
1. Cross-cutting learning from the discussion
2. Something that applies broadly

### Current State
- What's resolved
- What's still uncertain

### Next Steps
- [ ] Research X
- [ ] Validate Y
```

## Usage Notes

**When creating**:
1. Create directory: `docs/specs/discussions/<topic-name>/`
2. Create file: `discussion.md`
3. Fill header: date, status
4. Start with context: why discussing?
5. List questions: what needs deciding?

**During discussion**:
- Work through questions one at a time
- Document options, journey, and decision for each
- Check off questions as concluded
- Keep journey contextual - false paths, debates, and "aha" moments belong with the question they relate to

**Per-question structure**:
- **Context**: Why this specific question matters
- **Options Considered**: Approaches explored and their trade-offs
- **Journey**: The exploration - what we thought, what changed, false paths, insights
- **Decision**: What we chose, why, the deciding factor

**Optional sections**: Not every question needs all four sub-sections. Context and Decision are most important. Options and Journey depend on complexity.

**Anti-patterns**:
- Don't pull false paths into a separate top-level section - keep them with the question they relate to
- Don't turn into plan (no implementation steps)
- Don't write code
- Don't summarise the journey - document it

**Complete when**:
- Major questions concluded with rationale
- Trade-offs understood
- Path forward clear
