---
name: technical-planning
description: "Transform technical discussion documents into actionable implementation plans with phases, tasks, and acceptance criteria. Second phase of discussion-plan-implement workflow. Use when: (1) User asks to create/write an implementation plan, (2) User asks to plan implementation of discussed features, (3) Converting discussion documents from docs/specs/discussions/ into implementation plans, (4) User says 'plan this' or 'create a plan' after discussions, (5) Need to structure how to build something with phases and concrete steps. Creates plans in docs/specs/plans/{topic-name}/ that implementation phase executes via strict TDD."
---

# Technical Planning

Act as **expert technical architect** translating discussion decisions into TDD-ready implementation plans. Break complex features into testable phases and atomic tasks that implementation can execute without ambiguity.

**Input**: Discussion doc from `docs/specs/discussions/{topic}/`
**Output**: Plan in `docs/specs/plans/{topic}/plan.md`

## Rules

1. **Write plan documents only** - Do not write code or modify project files
2. **Reference discussion, don't re-debate** - Decisions are made. Plan how to execute them
3. **Every phase needs acceptance criteria** - Checkboxes that verify completion
4. **Every task needs a test name** - The test that proves the task works

## Structure

```
Phase (milestone)
├── Goal: What this phase accomplishes
├── Acceptance: [ ] Checkboxes to verify completion
└── Tasks (TDD units)
    ├── Task 1: Do X | Test: "it does X"
    ├── Task 2: Do Y | Test: "it does Y"
    └── Task 3: Do Z | Test: "it does Z"
```

**Phase** = independently testable milestone, 3-7 tasks
**Task** = one TDD cycle = one test = one commit

## Steps

1. **Read discussion doc** - Extract decisions, architecture, edge cases
2. **Define phases** - Foundation → Core → Edge cases → Refinement
3. **Break into tasks** - Each task: what to do + test name + edge cases
4. **Map edge cases** - Every edge case from discussion → task with test
5. **Add code examples** - For non-obvious patterns only
6. **Write to** `docs/specs/plans/{topic}/plan.md`

## Task Format

```markdown
1. **{What to build}**
   - **Do**: Specific implementation
   - **Test**: `"it does expected behavior"`
   - **Edge cases**: (if any)
```

## Bad Tasks → Good Tasks

| Bad | Problem | Good |
|-----|---------|------|
| "Implement caching" | Too big | "Implement CacheManager.get()" with test |
| "Handle errors" | Too vague | "Handle Redis timeout" with test |
| "Update the code" | Meaningless | Specific method + behavior + test |

## What Implementation Expects

Implementation will execute your plan via TDD:
1. Read phase acceptance criteria
2. For each task: write failing test from your test name → implement → commit
3. Verify phase acceptance met
4. Ask user before next phase

**Your test names become their first tests. Be specific.**

## Checklist Before Done

- [ ] Each phase has acceptance criteria (checkboxes)
- [ ] Each task has a test name
- [ ] All edge cases from discussion have tasks
- [ ] Code examples for complex patterns
- [ ] No "TBD" or vague items

## References

- **[template.md](references/template.md)** - Full plan template
- **[guidelines.md](references/guidelines.md)** - Sizing, examples, edge cases
