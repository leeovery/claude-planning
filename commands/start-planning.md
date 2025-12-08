---
description: Start a planning session from an existing discussion. Discovers available discussions, asks where to store the plan, and invokes the technical-planning skill.
---

Invoke the **technical-planning** skill for this conversation.

Before beginning, discover existing work and gather necessary information.

## Step 1: Discover Existing Work

Scan the codebase for discussions and plans:

1. **Find discussions**: Look in `docs/specs/discussions/*/discussion.md`
   - Note each topic name
   - Check status (Concluded | Deciding | Exploring)

2. **Find existing plans**: Look in `docs/specs/plans/*/`
   - Check for `plan.md` (local markdown)
   - Check for `linear.md` (Linear integration)
   - Check for `_overview.md` (Tasks.md format)

3. **Identify gaps**: Discussions without corresponding plans

## Step 2: Present Options to User

Show what you found:

```
📂 Discussions found:
  ✅ {topic-1} - Concluded (no plan yet)
  ✅ {topic-2} - Concluded (no plan yet)
  ⚠️  {topic-3} - Exploring (may not be ready)
  ✓  {topic-4} - Concluded (plan exists)

Which discussion would you like to create a plan for?
```

- Highlight "Concluded" discussions without plans as ready
- Warn about "Exploring" discussions (may not be ready for planning)
- Note discussions that already have plans

Ask: **Which discussion would you like to plan?**

## Step 3: Choose Output Destination

Ask: **Where should this plan live?**

1. **Local Markdown** - Simple `plan.md` file in `docs/specs/plans/`
   - Best for: Small features, solo work, quick iterations
   - Everything in one version-controlled file

2. **Linear** - Project with milestones and issues (requires MCP)
   - Best for: Team collaboration, visual tracking, larger features
   - Update tasks directly in Linear's UI
   - Requires: Linear MCP server configured

3. **Tasks.md** - Directory of task files for Kanban board
   - Best for: Visual tracking without external services
   - Works with Tasks.md UI or Obsidian
   - Files stay local and version-controlled

**If Linear is selected**: Check if Linear MCP is available. If not, inform the user and suggest alternatives.

## Step 4: Gather Additional Context

**For Linear destination**:
- Which team should own this project?

**For all destinations**:
- Any additional context or priorities to consider?
- Any constraints since the discussion concluded?

## Step 5: Invoke Planning Skill

Pass to the technical-planning skill:
- Discussion path: `docs/specs/discussions/{topic-name}/`
- Output destination: (local-markdown | linear | tasks-md)
- Additional context gathered

Example handoff:
```
Planning session for: {topic-name}
Discussion: docs/specs/discussions/{topic-name}/discussion.md
Output destination: Linear
Team: Engineering

Begin planning using the technical-planning skill.
Reference: output-linear.md for output format.
```

The skill will:
1. Read the discussion document
2. Create phases and tasks
3. Output in the specified format
4. Create appropriate files (plan.md, linear.md, or task directory)

## Notes

- Ask questions clearly and wait for responses before proceeding
- If the user wants to plan something without a discussion doc, that's okay - just note there's no prior discussion to reference
- Commit the plan files when complete
