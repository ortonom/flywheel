# Flywheel Skills Contract

This document defines the shared conventions for sitrep, preflight, and sweep skills.
All three skills MUST adhere to these conventions to prevent drift.

## Source of Truth

`CLAUDE.md` in the project root is the single source of truth for:
- Project status (first non-empty line after frontmatter or first heading)
- Blocking issues (referenced files in `projects/` directory)
- Temporary artifacts (dedicated section)

## CLAUDE.md Structure

```markdown
# Project Name

**Status:** [status line here]

---

## [Other sections...]

---

## Temporary Artifacts

**Tracked here. Swept on session end. Use `// TEMPORARY:` comment in code.**

Remove when [condition]:

- `path/to/file` — description of what it is

Remove when [different condition]:

- `path/to/other/file` — description
```

## Temporary Artifact Format

Each temporary artifact entry follows this pattern:

```
- `path/to/file` — description (optional details)
```

Artifacts are grouped under their removal condition:

```
Remove when [condition]:

- `file1` — desc
- `file2` — desc
```

## In-Code Markers

Temporary code should be marked with:

```
// TEMPORARY: [removal condition]
```

Example:
```typescript
// TEMPORARY: Remove when database sync is built
const cachedData = require('./data/reservations.json');
```

## Projects Directory

The `projects/` directory contains project files. Each file's first heading indicates the project name.

Files prefixed with numbers indicate priority/sequence (e.g., `01-blocking.md`).

## SITREP Output Format

```
SITREP [project name]
- Status: [quote from CLAUDE.md status line]
- Blocked: [list from projects/ or "None"]
- Awaiting: [open questions or "Nothing"]
- Temps: [count] tracked, [count] may be ready for removal
```

## Preflight Questions

Three questions, answered Yes/No or Clear/Assumed:

1. Can we reproduce existing output from inputs?
2. Do we have verified data to test against?
3. Are requirements clear or assumed?

If any answer is concerning: STOP. Clarify first.

## Sweep Actions

On session end (or manual trigger):

1. Review git diff for new files
2. Prompt for any that should be marked temporary
3. Check existing temps against current state
4. Update CLAUDE.md Temporary Artifacts section
5. Update status line if project state changed
