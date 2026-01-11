# Flywheel

Personal intelligence layer for Claude Code.

## Prime Directive

Eliminate recurring friction. Each unit of work should make the next unit easier.

## On Session Start (Mandatory)

1. Pull latest from this Flywheel repo
2. Read `rules.md` and `conventions/`
3. Look for `context.md` in the project you're working on
4. If it exists, read it. If not, create it.

## Rules

**Rule 1:** Keep a rolling context file—patterns, decisions, ways of working.

**Rule 2:** Log what you fix and why. Synthesize, don't journal. Once the root cause is solved, clear the log.

## Before Ending Work (Mandatory)

Before creating a PR or ending a session:

1. **Update context.md** — Add any new patterns, decisions, or ways of working discovered
2. **Log fixes** — If you solved a recurring problem, add it under `## Fixes` with root cause
3. **Clear solved fixes** — If a logged fix's root cause is now resolved, remove it
4. **Keep it lean** — Synthesize, don't append. Context should stay under 100 lines.

If no meaningful changes, skip. But default to capturing knowledge.

## context.md Template

When creating a new context.md:

```markdown
# Context

## Patterns
<!-- How things work in this codebase -->

## Decisions
<!-- Why things are the way they are -->

## Fixes
<!-- Active issues being tracked → remove when root cause solved -->
```

## Communication

- Use visual text docs (diagrams > prose)
- Be direct and concise
- You architect, I implement

## Git Workflow

See `conventions/git.md` and `conventions/git-troubleshooting.md`.

Don't tell user to create PR. Do it yourself with `gh pr create`.
