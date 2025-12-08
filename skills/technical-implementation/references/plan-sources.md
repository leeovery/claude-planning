# Plan Sources

*Reference for **[technical-implementation](../SKILL.md)***

---

Plans can be stored in different formats. Check `docs/specs/plans/{topic}/` for the format indicator file.

## Detecting Plan Format

Look for these files in the plan directory:

| File Found | Format | How to Read |
|------------|--------|-------------|
| `plan.md` | Local Markdown | Read file directly |
| `linear.md` | Linear | Query Linear via MCP |
| `_overview.md` | Tasks.md | Read directory structure |

## Local Markdown (`plan.md`)

**Detection**: `docs/specs/plans/{topic}/plan.md` exists

**Reading**:
1. Read `plan.md` directly
2. Phases and tasks are inline in the document
3. Follow phase order as written
4. Tasks have micro acceptance in the file

**Structure**:
```
## Phase 1: {Name}
**Tasks**:
1. **{Task Name}**
   - **Do**: What to implement
   - **Test**: `"it does expected behavior"`
```

## Linear (`linear.md`)

**Detection**: `docs/specs/plans/{topic}/linear.md` exists

**Reading**:
1. Read `linear.md` to get project ID and team
2. Query Linear MCP for project milestones (phases)
3. Query Linear MCP for issues within each milestone (tasks)
4. Process in milestone order

**Pointer file contains**:
```markdown
## Linear Integration
- **Project**: {PROJECT_NAME}
- **Project ID**: {ID}
- **Team**: {TEAM_NAME}
```

**MCP Queries**:
- Get milestones for project (phases)
- Get issues per milestone (tasks)
- Issue description contains: Goal, Implementation, Tests, Edge Cases

**Updating Progress**:
- After completing each task, update issue status in Linear via MCP
- User sees real-time progress in Linear UI

**Fallback**:
If Linear MCP is unavailable:
- Inform the user
- Cannot proceed without MCP access
- Suggest checking MCP configuration

## Tasks.md (`_overview.md`)

**Detection**: `docs/specs/plans/{topic}/_overview.md` exists

**Reading**:
1. Read `_overview.md` for plan context
2. List phase directories (numbered: `1-{name}/`, `2-{name}/`)
3. For each phase, read task files in order (`01-{task}.md`, `02-{task}.md`)

**Structure**:
```
docs/specs/plans/{topic}/
├── _overview.md              # Plan summary
├── 1-{phase-name}/           # Phase 1
│   ├── 01-{task}.md          # Task 1
│   └── 02-{task}.md          # Task 2
├── 2-{phase-name}/           # Phase 2
│   └── 01-{task}.md          # Task 1
└── done/                     # Completed tasks
```

**Task file contains**:
```markdown
## Goal
{What to accomplish}

## Implementation
{What to do}

## Tests
- `it does expected behavior`

## Acceptance
- [ ] Test written and failing
- [ ] Implementation complete
- [ ] Test passing
- [ ] Committed
```

**Updating Progress**:
- Check off acceptance items in task file
- Optionally move completed task files to `done/` directory

## Priority Order

If multiple format indicators exist (unusual), prefer:
1. `linear.md` (external source of truth)
2. `_overview.md` (structured directory)
3. `plan.md` (single file)

## Common Patterns

Regardless of format, you'll find:
- **Phases** with acceptance criteria
- **Tasks** with micro acceptance (test names)
- **Discussion reference** for context

Execute the same TDD workflow for all formats:
1. Derive test from micro acceptance
2. Write failing test
3. Implement to pass
4. Commit
5. Repeat
