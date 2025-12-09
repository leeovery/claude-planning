# Plan Sources

*Reference for **[technical-implementation](../SKILL.md)***

---

Plans are always stored in `docs/specs/plans/{topic}/plan.md`. The file's frontmatter declares the format.

## Detecting Plan Format

Always read `plan.md` first and check the `format` field in frontmatter:

```yaml
---
format: local-markdown | linear | backlog-md
---
```

| Format | Meaning | How to Proceed |
|--------|---------|----------------|
| `local-markdown` | Full plan is in this file | Read content directly |
| `linear` | Plan managed in Linear | Query Linear via MCP |
| `backlog-md` | Tasks in Backlog.md | Query via MCP or read `backlog/` |

## Local Markdown (`format: local-markdown`)

**Frontmatter**:
```yaml
---
format: local-markdown
---
```

**Reading**:
1. Read `plan.md` - all content is inline
2. Phases and tasks are in the document
3. Follow phase order as written
4. Tasks have micro acceptance in the file

**Structure**:
```markdown
## Phase 1: {Name}
**Tasks**:
1. **{Task Name}**
   - **Do**: What to implement
   - **Test**: `"it does expected behavior"`
```

## Linear (`format: linear`)

**Frontmatter**:
```yaml
---
format: linear
project: PROJECT_NAME
project_id: abc123-def456
team: Engineering
---
```

**Reading**:
1. Extract `project_id` and `team` from frontmatter
2. Query Linear MCP for project milestones (phases)
3. Query Linear MCP for issues within each milestone (tasks)
4. Process in milestone order

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

## Backlog.md (`format: backlog-md`)

**Frontmatter**:
```yaml
---
format: backlog-md
project: TOPIC_NAME
---
```

**Reading**:
1. If Backlog.md MCP is available, query tasks via MCP
2. Otherwise, read task files from `backlog/` directory
3. Filter tasks by label (e.g., `phase-1`) or naming convention
4. Process in priority order (high → medium → low)

**Structure**:
```
backlog/
├── task-1 - [P1] Configure auth.md
├── task-2 - [P1] Add login endpoint.md
├── task-3 - [P2] Session management.md
└── completed/                        # Done tasks
```

**Task file contains**:
```markdown
---
status: To Do
priority: high
labels: [phase-1, api]
---

# Task Title

{Description}

## Plan
{What to do}

## Acceptance Criteria
1. [ ] Test written: `it does expected behavior`
2. [ ] Implementation complete
3. [ ] Tests passing
4. [ ] Committed

## Notes
- Discussion: docs/specs/discussions/{topic}/discussion.md
```

**Updating Progress**:
- Update task status to "In Progress" when starting
- Check off acceptance criteria items
- Update status to "Done" when complete
- Backlog.md auto-moves to completed folder via CLI

**MCP Tools** (if available):
- Query tasks by status/label
- Update task status
- Add notes to tasks

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
