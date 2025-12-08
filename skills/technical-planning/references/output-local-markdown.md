# Output: Local Markdown

*Output adapter for **[technical-planning](../SKILL.md)***

---

Use this output format for **simple features** or when you want everything version-controlled in a single file.

## Output Location

```
docs/specs/plans/{topic-name}/
└── plan.md
```

The directory name should match the discussion topic name from `docs/specs/discussions/{topic-name}/`.

## File Structure

Use the template from **[template.md](template.md)** to create `plan.md` with:

- Overview (goal, done-when, key decisions)
- Architecture summary
- Phases with acceptance criteria checkboxes
- Tasks with micro acceptance (test names)
- Edge case mapping table
- Testing strategy
- Dependencies
- Change log

## What to Include

Everything goes in the single `plan.md` file:

- All phases and tasks inline
- All acceptance criteria
- Edge case mapping
- Code examples for complex patterns

## Linking to Discussion

In the plan header:

```markdown
**Discussion**: `docs/specs/discussions/{topic-name}/`
```

Implementation and review will follow this link for context.

## When to Use

- Small to medium features
- Solo development
- Quick iterations
- When you want simple git-tracked documentation

## Resulting Structure

After planning:

```
docs/specs/
├── discussions/
│   └── {topic-name}/
│       └── discussion.md     # Source decisions
└── plans/
    └── {topic-name}/
        └── plan.md           # Your output
```

Implementation will read `plan.md` directly and execute via TDD.
