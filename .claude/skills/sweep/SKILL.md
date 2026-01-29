---
name: sweep
description: This skill should be used at the end of a session or when the user wants to clean up temporary artifacts. It reviews session changes, prompts for temporary file marking, removes obsolete temps, and updates CLAUDE.md. Triggers on session end, "/sweep", or requests like "clean up" or "what did we create".
---

# Sweep

Review session changes. Mark new temps. Remove obsolete ones. Update project memory.

## When to Use

- End of session (before committing)
- After completing a significant piece of work
- When user asks to clean up or review changes
- Manually via `/sweep`

## Workflow

### 1. Review Session Changes

Check what changed this session:

```bash
git status
git diff --name-only
```

Identify:
- New files created
- Files with `// TEMPORARY:` comments added
- Changes to existing temporary artifacts

### 2. Prompt for Temporary Marking

For each new file that might be temporary:

Ask: "Is `[filename]` temporary or permanent?"

If temporary, ask: "What's the removal condition?"

Suggested conditions based on common patterns:
- "Remove when database is built"
- "Remove when API integration is complete"
- "Remove when [feature] ships"
- "Remove when tests are added"

### 3. Check Existing Temps

Read the Temporary Artifacts section from CLAUDE.md.

For each tracked artifact:
- Check if the removal condition is now met
- Look for evidence in the codebase
- If condition met, mark for removal

### 4. Update CLAUDE.md

If changes needed:

**Add new temps:**
```markdown
Remove when [condition]:

- `path/to/new/file` — description
```

**Remove obsolete temps:**
Delete the line from Temporary Artifacts section.

**Update status line** if project state materially changed.

### 5. Report Changes

Output:

```
SWEEP COMPLETE

Added to tracking:
- `path/to/file` — remove when [condition]

Removed (conditions met):
- `path/to/old/file` — [condition] is now satisfied

Still tracked: [N] temporary artifacts

Status: [Updated/Unchanged]
```

## In-Code Markers

When adding temporary code, include a marker:

```typescript
// TEMPORARY: Remove when database sync is built
const cachedData = require('./data/reservations.json');
```

The sweep process should find these markers:

```bash
grep -r "// TEMPORARY:" --include="*.ts" --include="*.js"
```

And ensure corresponding entries exist in CLAUDE.md.

## Examples

**Clean session:**
```
SWEEP COMPLETE

Added to tracking: None
Removed (conditions met): None
Still tracked: 2 temporary artifacts
Status: Unchanged
```

**With changes:**
```
SWEEP COMPLETE

Added to tracking:
- `data/sample_listings.json` — remove when API integration complete

Removed (conditions met):
- `scripts/manual_calc.ts` — "remove when lib/allocation.ts built" is now satisfied

Still tracked: 2 temporary artifacts
Status: Updated ("Pipeline layer not built" → "Pipeline layer in progress")
```

## Edge Cases

**No CLAUDE.md exists:**
Create minimal structure with Temporary Artifacts section.

**No Temporary Artifacts section:**
Add the section to CLAUDE.md.

**User declines to mark file as temporary:**
Do not add to tracking. File is considered permanent.

## Contract

This skill follows the shared contract in `references/contract.md`. The CLAUDE.md structure, temporary artifact format, and in-code markers are defined there.
