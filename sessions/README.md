# Sessions

This directory maintains continuity between Claude Code sessions.

## Files

- **current.md** - Active working context. Read at session start, update at session end.
- **archive/** - Past session notes (optional, for reference)

## Workflow

### Starting a Session
1. Read `current.md`
2. Understand where things left off
3. Continue or start fresh as appropriate

### During a Session
- Update `current.md` as significant progress is made
- Add learnings to `../learnings/` as discovered

### Ending a Session
1. Update `current.md` with handoff notes
2. Use the template from `../prompts/session-handoff.md`
3. Be specific about next steps

## Tips

- Keep `current.md` focused on what's actionable
- Archive old context that's no longer relevant
- The goal is seamless pickup, not comprehensive history
