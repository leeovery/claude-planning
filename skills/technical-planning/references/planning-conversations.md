# Planning Conversations

*Reference for **[technical-planning](../SKILL.md)***

---

## The Purpose of Draft Planning

Draft planning exists to produce a **standalone specification** containing everything needed to construct formal phases and tasks. It is NOT a summary. It is NOT conversation notes. It is the complete, unambiguous foundation for formal planning.

The draft is where you collaborate with the user to ensure:
1. **Nothing is missing** - the WHAT is clearly defined with enough detail
2. **Nothing is hallucinated** - only real, validated information remains
3. **The right level of detail** - not so vague it's useless, not so prescriptive it constrains implementation unnecessarily

### Two Scenarios That Require Draft Planning

**Scenario A: Enrichment (Adding Detail)**

Source materials (discussion docs, etc.) may capture WHAT to build and WHY, but haven't fully rounded out HOW to approach it. The draft process adds this detail through collaborative discussion - not hallucination.

- What's the actual sequence of work?
- What are the specific constraints?
- What patterns or approaches will we use?
- What edge cases need explicit handling?

**Scenario B: Filtering (Removing Noise)**

Source materials may contain too much detail, tangential discussion, speculative content, or hallucinated information. The draft process filters this through collaborative review.

- What's actually in scope vs mentioned but not committed to?
- Which discussed options were rejected?
- What was speculation vs decision?
- What information is validated vs assumed?

Most real planning involves **both** - enriching some areas while filtering others.

### The Draft as Deliverable

At the end of draft planning, `draft-plan.md` must be a **standalone file** containing:
- Clear definition of WHAT we're building and WHY
- Edge cases the implementation needs to be aware of
- Testing ideas and acceptance criteria
- Optionally, guidance on HOW to approach it (when helpful, not prescriptive)
- Only validated information, not summaries or assumptions

A good draft defines the feature clearly - what it does, why it exists, what edge cases matter, how we'll know it's complete. Implementation can then take charge or collaborate to figure out the approach.

## Two-Phase Planning

### Phase A: Draft Planning (Build the Specification)

Collaborative discussion that produces a complete specification:
- What exactly are we building?
- How exactly will we approach it?
- What specific constraints and edge cases exist?
- What's in scope, what's out?

**Output**: `draft-plan.md` - standalone specification ready for structuring

### Phase B: Formal Planning (Structure the Specification)

Convert the complete draft into structured format:
- Phases with acceptance criteria
- Tasks with micro acceptance
- Ready for implementation

**Output**: `plan.md` (or Linear/Backlog.md depending on destination)

## When to Use Draft Planning

**Use draft planning when**:
- Source materials need enrichment (missing HOW details)
- Source materials need filtering (contains noise, speculation, hallucination)
- Complex feature with unclear approach
- Multiple valid ways to structure the work
- User wants to collaborate on the specification

**Skip to formal planning when**:
- Source materials already contain complete specification
- Small feature with obvious structure
- User has already worked out all the detail elsewhere

## Critical Rule: Immediate Capture

> **After each user response, IMMEDIATELY update the draft document before asking your next question.**

This is non-negotiable. Context windows refresh without warning. Three hours of planning discussion can vanish.

### Capture Frequency

- Update after **every natural break** in discussion
- **Never let more than 2-3 exchanges pass** without writing
- When in doubt, write it down NOW

### What to Capture

Record **what the user said AND why**, not just conclusions. Capture the reasoning, not just the decision.

### Capture the Reasoning Journey

- Why this phase before that one
- Trade-offs considered
- Alternative structures rejected and why
- Concerns raised and how addressed
- Scope decisions (what's in, what's out, why)

## Draft Document Format

The draft has two sections: a **Planning Log** (running capture) and a **Specification** (the deliverable). The log feeds the specification.

```markdown
# Draft Plan: [Topic Name]

**Status**: Draft - building specification
**Created**: [date]
**Last Updated**: [timestamp]

---

## Specification

This section is the deliverable. Update it continuously as clarity emerges.

### What We're Building

[Specific, concrete description of the feature - not a summary. What does it do? What problem does it solve? Include enough detail that someone understands the scope without reading source materials.]

### Why We're Building It

[The motivation. What user need or technical need does this address? This context helps implementation make good decisions.]

### Approach (Optional)

[Include when there's a specific approach that should be followed. Leave out or keep light when implementation should figure this out. This section can range from "use the existing X pattern" to detailed guidance - whatever level is appropriate for this feature.]

### Scope Boundaries

**In scope**:
- [Specific item with enough detail to be unambiguous]
- [Specific item]

**Out of scope** (explicitly excluded):
- [Item and why excluded]
- [Item]

### Technical Decisions

| Decision | Choice | Reasoning |
|----------|--------|-----------|
| [decision point] | [what we chose] | [why - be specific] |

### Edge Cases to Handle

- [Edge case that implementation must be aware of]
- [Another edge case - what makes this tricky]

### Constraints

- [Technical constraint that limits options]
- [Business constraint that affects scope]

### Testing and Acceptance

How we'll know this is complete:
- [Acceptance criterion]
- [Another criterion]

Testing ideas:
- [Test scenario to cover]
- [Edge case that needs a test]

### Open Questions (Blocking)

- [ ] [Question that MUST be answered before formal planning]

---

## Planning Log

Running capture of discussion. Use this to build the specification above.

### [timestamp] Initial Discussion

[What was discussed, decisions made, reasoning. Use this to populate the Specification section above.]

### [timestamp] [Topic]

[Continue capturing. After each exchange, update BOTH this log AND the Specification section.]
```

### Key Principle: The Specification Section is the Deliverable

The Planning Log is working notes. The Specification section is what matters.

As you discuss with the user:
1. Capture in the Planning Log
2. **Immediately** distill validated information into the Specification
3. The Specification grows more complete with each exchange
4. When Specification is complete, formal planning can begin

## Commit Frequently

**Commit the draft document**:
- After each significant exchange
- At natural breaks in discussion
- When structure becomes clearer
- **Before any context refresh**
- When creating the initial file

Commits are your safety net. A context refresh with uncommitted work = lost work.

## Transitioning to Formal Plan

Draft planning is complete when the **Specification section** contains:
- [ ] Clear description of WHAT we're building and WHY
- [ ] Scope boundaries (in/out)
- [ ] Edge cases implementation needs to handle
- [ ] Testing ideas and acceptance criteria
- [ ] Approach guidance (if needed - this varies by feature)
- [ ] No blocking open questions remaining

The right level of detail varies. Some features need detailed HOW guidance; others just need clear WHAT/WHY and can leave implementation to figure out the approach. The draft process is about finding that line.

**Transition process**:
1. Review the Specification section together
2. User confirms it contains everything needed
3. Create formal `plan.md` by structuring the Specification into phases/tasks
4. Keep `draft-plan.md` for reference (the "why" behind decisions)

**The Specification is standalone** - formal planning should not require going back to source discussion documents. Everything needed is in the draft.

## Building Through Collaboration

During draft planning, you are **building** the specification through discussion. When information is missing, ask.

After context refresh:
- **If the draft exists**: Read it. Trust it. The Specification section has the detail you've lost.
- **If no draft exists**: You've lost the planning conversation. Be honest about that and ask again for what you need.
