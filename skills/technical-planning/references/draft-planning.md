# Draft Planning

*Reference for **[technical-planning](../SKILL.md)** - Path A*

---

You are creating a draft plan - a collaborative workspace where you and the user refine reference material into a validated specification.

## Purpose

Draft planning is a **two-way process**:

1. **Filter**: Reference material may contain hallucinations, inaccuracies, or outdated concepts. Validate before including.

2. **Enrich**: Reference material may have gaps. Fill them through discussion.

The draft plan is an **interim document** - a workspace for collecting validated, refined content that will feed formal planning.

**The draft plan must be standalone.** It should contain everything formal planning needs - no references back to discussions or other source material. When complete, it draws a line: formal planning uses only this document.

## Source Materials

Before starting any topic, review ALL available reference material:
- Discussion documents (from technical-discussion phase)
- Any existing partial plans
- Related documentation

**Treat all source material as untrusted input.** Even discussion documents that went through a collaborative process may contain issues. Your job is to synthesize and present - the user validates.

## The Workflow

Work through the specification **topic by topic**:

### 1. Review
Read all reference material for the current topic. Understand what exists.

### 2. Synthesize and Present
Present your understanding to the user **in the format it would appear in the plan**:

> "Here's what I understand about [topic] based on the reference material. This is how I'd write it into the specification:
>
> [content as it would appear]
>
> Does this capture it? Anything to adjust, remove, or add?"

### 3. Discuss and Refine
Work through the content together:
- Validate what's accurate
- Remove what's wrong, outdated, or hallucinated
- Add what's missing through brief discussion
- **Course correct** based on knowledge from subsequent project work
- Refine wording and structure

This is a **human-level conversation**, not form-filling. The user brings context from across the project that may not be in the reference material - decisions from other topics, implications from later work, or knowledge that can't all fit in context.

### 4. Log When Approved
Only when the user approves ("yes, log it", "that's good", etc.) do you write it to the draft plan - **verbatim** as presented and approved.

### 5. Repeat
Move to the next topic.

## The Draft Document

Create `draft-plan.md` in `docs/specs/plans/{topic-name}/`

Structure is **flexible** - organize around phases and subject matter, not rigid sections. This is a working document.

Suggested skeleton:

```markdown
# Draft Plan: [Topic Name]

**Status**: Draft - building specification
**Last Updated**: [timestamp]

---

## Specification

[Validated content accumulates here, organized by topic/phase]

---

## Working Notes

[Optional - capture in-progress discussion if needed]
```

## Critical Rules

**Present before logging**: Never write content to the draft until the user has seen and approved it.

**Log verbatim**: When approved, write exactly what was presented - no silent modifications.

**Commit frequently**: Commit at natural breaks and before any context refresh. Context refresh = lost work.

**Trust nothing without validation**: Synthesize and present, but never assume source material is correct.

## After Context Refresh

Read the draft plan. It contains validated, approved content. Trust it - you built it together with the user.

If working notes exist, they show where you left off.

## Transitioning to Formal Planning

Draft is complete when:
- All topics/phases have validated content
- User confirms the specification is complete
- No blocking gaps remain

Then proceed to formal planning → Load [formal-planning.md](formal-planning.md)
