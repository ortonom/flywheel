# Git Troubleshooting

```
┌─────────────────────────────────────────────────────────────┐
│                  COMMON GIT PROBLEMS                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Problem → Fix                                             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Push Rejected

```
error: failed to push some refs
hint: Updates were rejected because the remote contains work
```

**Fix:**
```bash
git pull --rebase origin <branch>
git push
```

## No Upstream Branch

```
fatal: The current branch has no upstream branch
```

**Fix:**
```bash
git push -u origin <branch-name>
```

## Permission Denied / 403

```
remote: Permission to repo denied
fatal: unable to access
```

**Fix:**
- Check branch name matches required pattern (e.g., `claude/*`)
- Verify you're on the correct branch: `git branch --show-current`

## Branch Disappeared from GitHub

Claude Code web branches can vanish from GitHub UI.

**Fix:**
```bash
git push -u origin <branch-name> --force
```

Branch still exists locally—re-pushing restores it.

## gh CLI Not Available

```
gh: command not found
```

**Fix:** Use git API directly or tell user:
```
PR ready. Push completed to branch: <branch-name>
Create PR at: https://github.com/<owner>/<repo>/compare/<branch>
```

Only use this fallback if `gh` genuinely doesn't exist.

## Detached HEAD

```
HEAD detached at <commit>
```

**Fix:**
```bash
git checkout -b <new-branch-name>
git push -u origin <new-branch-name>
```

## Nothing to Commit

```
nothing to commit, working tree clean
```

**Not an error.** No changes to save. Move on.

## Merge Conflicts

```
CONFLICT (content): Merge conflict in <file>
```

**Fix:**
1. Open the file
2. Look for `<<<<<<<`, `=======`, `>>>>>>>`
3. Keep the correct version, delete markers
4. `git add <file>`
5. `git commit -m "Resolve merge conflict"`
6. `git push`

## Rule

Read the error message. It usually tells you what to do.
