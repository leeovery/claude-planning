# Output: Tasks.md

*Output adapter for **[technical-planning](../SKILL.md)***

---

Use this output format when you want a **local Kanban board** backed by markdown files. Tasks.md provides a visual UI while keeping everything version-controlled.

## About Tasks.md

Tasks.md is a self-hosted Kanban application that uses your filesystem:
- **Lanes** = directories
- **Tasks** = individual markdown files
- Web UI for drag-and-drop task management
- Files stay in your repo, version-controlled

See: https://github.com/BaldissaraMatheus/Tasks.md

## Output Location

```
docs/specs/plans/{topic-name}/
├── _overview.md              # Plan summary and metadata
├── 1-{phase-name}/           # Lane = Phase
│   ├── 01-{task-name}.md     # Task card
│   ├── 02-{task-name}.md
│   └── 03-{task-name}.md
├── 2-{phase-name}/
│   ├── 01-{task-name}.md
│   └── 02-{task-name}.md
└── done/                     # Completed tasks move here
```

## File Structure

### Overview File (`_overview.md`)

```markdown
# Plan: {Feature Name}

**Discussion**: `docs/specs/discussions/{topic-name}/`
**Created**: {DATE}
**Status**: Draft | Ready | In Progress | Completed

## Goal
{One sentence description}

## Done When
- {Measurable outcome 1}
- {Measurable outcome 2}

## Key Decisions
- {Decision 1}: {Rationale}
- {Decision 2}: {Rationale}

## Phases
1. **{Phase 1 Name}**: {Goal}
2. **{Phase 2 Name}**: {Goal}

## Notes
{Any additional context}
```

### Phase Directory

Directory name format: `{N}-{phase-name}` (e.g., `1-core-authentication`)

The number prefix ensures phases display in order.

### Task File

Each task is a separate markdown file within its phase directory.

Filename format: `{NN}-{task-name}.md` (e.g., `01-add-login-endpoint.md`)

```markdown
---
tags: [api, auth]
---

# {Task Name}

## Goal
{What this task accomplishes}

## Implementation
{The "Do" - specific files, methods, approach}

## Tests
- `it does expected behavior`
- `it handles edge case X`

## Edge Cases
- {Specific edge cases for this task}

## Context
See: `docs/specs/discussions/{topic-name}/discussion.md`

## Acceptance
- [ ] Test written and failing
- [ ] Implementation complete
- [ ] Test passing
- [ ] Committed
```

## Task Movement

When implementation completes a task:
1. Check off acceptance items in the task file
2. Move the file to the `done/` directory (or Tasks.md UI handles this)

This provides visual progress tracking in the Kanban board.

## When to Use

- When you want visual Kanban tracking
- Solo or small team development
- Everything stays local and version-controlled
- No external service dependencies
- Works well with Obsidian or other markdown tools

## Resulting Structure

After planning:

```
docs/specs/
├── discussions/
│   └── {topic-name}/
│       └── discussion.md
└── plans/
    └── {topic-name}/
        ├── _overview.md
        ├── 1-setup/
        │   └── 01-configure-auth.md
        ├── 2-core-features/
        │   ├── 01-login-endpoint.md
        │   └── 02-session-management.md
        ├── 3-edge-cases/
        │   └── 01-handle-expired-tokens.md
        └── done/
```

## Implementation Reading

Implementation will:
1. Read `_overview.md` for context
2. Process phases in directory order (1-, 2-, 3-)
3. Process tasks within each phase in file order (01-, 02-)
4. Move completed task files to `done/` directory

## Integration with Tasks.md UI

If using the Tasks.md web interface:
- Point Tasks.md to `docs/specs/plans/{topic-name}/`
- Phase directories become lanes
- Task files become cards
- Drag cards between lanes to track progress
- Changes sync to filesystem automatically
