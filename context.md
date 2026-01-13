# Context

## Patterns

### File Structure
- `CLAUDE.md` - Prime directive and session workflow
- `conventions.md` - Git, code, and communication rules
- `prompts/install.md` - One-liner to bootstrap Flywheel in new projects
- `hooks/stop-hook-git-check.sh` - Git validation hook

### Code Conventions (from conventions.md)
- **Follow existing patterns**: Match style, naming, structure of codebase
- **Minimal changes**: Only touch what's necessary, smaller diffs
- **No gold-plating**: No unrequested features, abstractions, or "improvements"

## Decisions

- Flywheel is a learning loop: context.md accumulates knowledge across sessions
- Division of labor: User architects, Claude implements
- Keep context.md under 100 lines, synthesize don't append

## Fixes

(None yet)
