# Skills

**Live:** `~/.claude/skills/[name]/SKILL.md` — where Claude loads them
**Mirror:** `~/flywheel/skills/[name]/SKILL.md` — version-controlled backup

When a skill is updated in either location, sync the other to match.

# Workflow

Before non-trivial implementation → run `/preflight`.
End of session → run `/sweep`.

## Pacing

Default: execute plans autonomously.

If the user says **"one by one"**, **"step by step"**, or **"walk me through it"**:
- Do one step only
- Explain what you did and why
- Wait for approval before the next step

This applies even in bypass permissions mode. The user controls pacing regardless of permission settings.

To resume autonomous execution: **"run it"**, **"go"**, or **"YOLO"**.
