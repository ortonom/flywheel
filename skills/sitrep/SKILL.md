---
name: sitrep
description: Report project status by reading documented state. Do not summarize — quote what's documented.
---

# Sitrep

Report project status by reading documented state. Do not summarize — quote what's documented.

## Workflow

### 1. Read Project Memory

Read `CLAUDE.md` in the project root. Extract:

- **Status line**: First line containing "Status:" or the first substantive line after the title
- **Temporary Artifacts section**: The section listing tracked temporary files with removal conditions

### 2. Scan Projects Directory

List files in `projects/` directory. For each file:

- Read the first heading (project name)
- Note any status indicators (blocked, waiting, etc.)

### 3. Check Temporary Artifact Conditions

For each temporary artifact listed:

- Check if the removal condition might be satisfied
- Look for evidence in the codebase (e.g., "remove when database built" → check for migrations)
- Flag any that appear ready for removal

### 4. Output SITREP

Format output exactly as:

```
SITREP [project name from CLAUDE.md title]
- Status: [quote the status line exactly]
- Blocked: [list blocked items from projects/ or "None"]
- Awaiting: [open questions found, or "Nothing"]
- Temps: [N] tracked, [M] may be ready for removal
```

If temps may be ready for removal, list them:

```
Temps potentially ready:
- `path/to/file` — condition "[removal condition]" may be met because [evidence]
```

## Examples

**Minimal SITREP:**
```
SITREP CVLife Panama
- Status: "Formula layer complete (lib/). Pipeline layer not built."
- Blocked: 01-blocking.md (requires Brian's input)
- Awaiting: Nothing
- Temps: 2 tracked, 0 may be ready for removal
```

**With temp ready for removal:**
```
SITREP MyProject
- Status: "Database integration complete"
- Blocked: None
- Awaiting: Nothing
- Temps: 3 tracked, 1 may be ready for removal

Temps potentially ready:
- `data/mock_users.json` — condition "remove when database built" may be met because db/migrations/ contains user table migration
```

## Contract

This skill follows the shared contract in `references/contract.md`. The SITREP format, CLAUDE.md structure, and temporary artifact conventions are defined there.
