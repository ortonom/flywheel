# Example Learning: Session Continuity Pattern

## Context

Browser-based Claude Code doesn't maintain state between sessions. Each new session starts fresh with no memory of previous work.

## The Learning

Explicit handoff notes bridge this gap. At session end:
1. Summarize what was accomplished
2. Document current state
3. List concrete next steps
4. Note any context that isn't obvious from the code

The next session reads these notes and can pick up where you left off with minimal ramp-up time.

## Example

Instead of ending a session with just "fixed the bug", end with:

```
## Session Handoff - 2024-01-15

Fixed the race condition in user authentication (src/auth/login.ts:45-67).
Root cause: async token validation wasn't awaited before redirect.

Current state: Login flow works. Need to add similar fix to logout flow.

Next: Check src/auth/logout.ts for same pattern.
```

---

_Delete this example file once you have real learnings to add._
