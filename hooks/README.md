# Hooks

Claude Code hooks that enforce discipline.

## stop-hook-git-check.sh

```
┌─────────────────────────────────────────────────────────────┐
│                    WHAT IT DOES                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Runs when Claude stops/pauses. Checks for:                │
│                                                             │
│   • Uncommitted changes (staged or unstaged)                │
│   • Untracked files                                         │
│   • Unpushed commits                                        │
│                                                             │
│   If any found → blocks exit until committed/pushed         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Install

Copy to `~/.claude/` and make executable:

```bash
cp hooks/stop-hook-git-check.sh ~/.claude/
chmod +x ~/.claude/stop-hook-git-check.sh
```

Then add to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "stop": [
      {
        "command": "~/.claude/stop-hook-git-check.sh"
      }
    ]
  }
}
```
