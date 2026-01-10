# Git Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                    HOW TO COMMIT                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   1. Stage files:     git add <files>                       │
│   2. Commit:          git commit -m "what you did"          │
│   3. Push:            git push -u origin <branch>           │
│                                                             │
│   That's it. Don't overthink it.                            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## When to Commit

- After meaningful changes
- Before stopping/ending session
- When switching focus to different task

## If Push Fails

```
┌─────────────────────────────────────────────────────────────┐
│                    TROUBLESHOOTING                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   "no upstream branch"                                      │
│   └── git push -u origin <branch-name>                      │
│                                                             │
│   "rejected - non-fast-forward"                             │
│   └── git pull --rebase origin <branch> && git push         │
│                                                             │
│   "permission denied"                                       │
│   └── Check branch name matches expected pattern            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## On Session Start

Make a small test commit to verify git works:

```bash
git status
git add -A && git commit -m "Session start" && git push
```

If this works, you're good. If not, fix it before doing anything else.

## How to Create PRs

```
┌─────────────────────────────────────────────────────────────┐
│                    CREATE PR YOURSELF                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Don't tell user to create PR. Do it:                      │
│                                                             │
│   gh pr create --title "What this does" --body "Details"    │
│                                                             │
│   Then share the PR URL with the user.                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

Never say "Create PR at: <url>". Just create it.

## Rule

Don't get stuck on git. If something fails, read the error, fix it, move on.
