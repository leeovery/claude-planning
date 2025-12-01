# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Claude Code skills package for structured technical discussion and planning workflows. Distributed via Composer as `leeovery/claude-technical-workflows`.

## Three-Phase Workflow

1. **Discussion** (`technical-discussion` skill): Capture WHAT and WHY - decisions, architecture, edge cases, debates
2. **Planning** (`technical-planning` skill): Define HOW - phases, structure, code examples
3. **Implementation**: Actual coding (outside this package's scope)

## Structure

```
skills/
  technical-discussion/    # Phase 1: Document discussions
  technical-planning/      # Phase 2: Create implementation plans
commands/
  start-discussion.md      # Slash command to begin discussions
```

## Key Conventions

- Discussion docs: `docs/specs/discussions/<topic-name>/discussion.md`
- Plan docs: `docs/specs/plans/<topic-name>/`
- Commit discussion docs frequently (natural breaks, before context refresh)
- Skills capture context, don't implement
