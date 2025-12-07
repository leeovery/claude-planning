# Implementation Plan Template

*Reference for **[technical-planning](../SKILL.md)** | See also: **[guidelines.md](guidelines.md)***

---

Copy this structure to `docs/specs/plans/{topic}/plan.md`:

```markdown
# Implementation Plan: {Feature/Project Name}

**Date**: YYYY-MM-DD
**Status**: Draft | Ready | In Progress | Completed
**Discussion**: `docs/specs/discussions/{topic-name}/`

## Overview

**Goal**: What we're building and why (one sentence)

**Done when**:
- Measurable outcome 1
- Measurable outcome 2

**Key Decisions** (from discussion):
- Decision 1: Rationale
- Decision 2: Rationale

## Architecture

- Components
- Data flow
- Integration points

## Phases

---

### Phase 1: {Name}

**Goal**: What this phase accomplishes

**Acceptance**:
- [ ] Criterion 1
- [ ] Criterion 2

**Tasks**:

1. **{Task Name}**
   - **Do**: What to implement
   - **Test**: `"it does expected behavior"`
   - **Edge cases**: (if any)

2. **{Task Name}**
   - **Do**: What to implement
   - **Test**: `"it does expected behavior"`

**Code example** (if pattern is non-obvious):
```pseudocode
// Structure, not production code
```

---

### Phase 2: {Name}

(Same structure - repeat for each phase)

---

## Edge Cases

| Edge Case | Solution | Phase.Task | Test |
|-----------|----------|------------|------|
| {From discussion} | How handled | 1.2 | `"it handles X"` |

## Testing Strategy

**Unit**: Per-component coverage (from task tests)
**Integration**: Cross-component flows
**Manual**: (if needed)

## Data Models (if applicable)

Tables, schemas, API contracts

## Dependencies

- Prerequisites for Phase 1
- Phase dependencies
- External blockers

## Rollback (if applicable)

Triggers and steps

## Log

| Date | Change |
|------|--------|
| YYYY-MM-DD | Created from discussion |
```
