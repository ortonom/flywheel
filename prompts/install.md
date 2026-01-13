# Install Flywheel

One-liner to paste into any new project:

```
Set up Flywheel here: add the Flywheel section to CLAUDE.md (create if needed), create context.md for rolling context. Commit, push, and create the PR yourself with gh pr create.
```

## What to add to CLAUDE.md

```markdown
## Flywheel

This project uses Flywheel. You MUST fetch https://raw.githubusercontent.com/ortonom/flywheel/main/CLAUDE.md at session start and follow its workflow.
```

## Fallback (if gh unavailable)

```
Set up Flywheel here: add the Flywheel section to CLAUDE.md (create if needed), create context.md for rolling context. Commit and push. Then tell me: "PR ready. Create at: https://github.com/<owner>/<repo>/compare/<branch>"
```
