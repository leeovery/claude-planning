# Output: Linear

*Output adapter for **[technical-planning](../SKILL.md)***

---

Use this output format when you want **Linear as the source of truth** for plan management. The user can update tasks directly in Linear's UI, and implementation will query Linear for the current state.

## Prerequisites

- Linear MCP server must be configured
- User must specify which Linear team to use

## Linear Structure Mapping

| Planning Concept | Linear Entity |
|------------------|---------------|
| Plan | Project |
| Phase | Milestone |
| Task | Issue |

## Output Process

### 1. Create Linear Project

Via MCP, create a project with:

- **Name**: Match the discussion topic name
- **Description**: Brief summary + link to discussion file
- **Team**: As specified by user

### 2. Create Milestones for Phases

For each phase, create a milestone:

- **Name**: `Phase {N}: {Phase Name}`
- **Description**: Phase goal + acceptance criteria

Example:
```
Name: Phase 1: Core Authentication
Description:
Goal: Implement basic login/logout flow

Acceptance:
- [ ] User can log in with email/password
- [ ] Session persists across page refresh
- [ ] Logout clears session
```

### 3. Create Issues for Tasks

For each task, create an issue within the appropriate milestone:

**Title**: Clear action statement

**Description** (use this structure):
```markdown
## Goal
[What this task accomplishes - one sentence]

## Implementation
[The "Do" from planning - specific files, methods, approach]

## Tests (Micro Acceptance)
- `it does expected behavior`
- `it handles edge case X`

## Edge Cases
- [Specific edge cases for this task]

## Context
Discussion: `docs/specs/discussions/{topic-name}/discussion.md`
[Optional: link to specific decision if relevant]
```

**Labels** (optional):
- `edge-case` - for edge case handling tasks
- `foundation` - for setup/infrastructure tasks
- `refactor` - for cleanup tasks

### 4. Create Local Pointer File

Create `docs/specs/plans/{topic-name}/linear.md`:

```markdown
# Plan Reference: {Topic Name}

## Linear Integration

- **Project**: {PROJECT_NAME}
- **Project ID**: {ID from MCP response}
- **Team**: {TEAM_NAME}
- **Created**: {DATE}

## Source

- **Discussion**: `docs/specs/discussions/{topic-name}/discussion.md`

## How to Use

This plan is managed in Linear. Implementation will:
1. Read this file to find the Linear project
2. Query Linear for current issues and milestones
3. Work through tasks in milestone order
4. Update issue status as tasks complete

To modify the plan, edit directly in Linear.
```

## Issue Content Guidelines

Issues should be **self-contained for execution**:

**Include directly**:
- What to implement (specific files/methods)
- Test names (micro acceptance)
- Edge cases for this specific task
- Any code examples for complex patterns

**Link to (don't copy)**:
- Discussion document (for "why" context)
- Specific decision sections if particularly relevant

The goal: anyone (Claude or human) could pick up the issue and execute it.

## When to Use

- Larger features needing visual tracking
- Team collaboration
- When you want to update plans without editing markdown
- Projects already using Linear for issue tracking

## Resulting Structure

After planning:

```
docs/specs/
├── discussions/
│   └── {topic-name}/
│       └── discussion.md     # Source decisions
└── plans/
    └── {topic-name}/
        └── linear.md         # Pointer to Linear project

Linear:
└── Project: {topic-name}
    ├── Milestone: Phase 1
    │   ├── Issue: Task 1
    │   └── Issue: Task 2
    └── Milestone: Phase 2
        └── Issue: Task 3
```

Implementation will read `linear.md`, query Linear via MCP, and execute tasks.

## Fallback Handling

If Linear MCP is unavailable during implementation:
- Implementation should inform the user
- Suggest running `/sync-plan-from-linear` when available
- Or switch to local markdown approach

## MCP Tools Used

Planning uses these Linear MCP capabilities:
- Create project
- Create milestone
- Create issue
- Assign issue to milestone

Implementation uses:
- Query project issues
- Query milestones
- Update issue status
