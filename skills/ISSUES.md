# Skills Analysis — Issues Found

Analysis of all five skills (`preflight`, `prep`, `proof`, `sitrep`, `sweep`) and shared `contract.md`.

---

## Critical — Structural problems that will cause confusion or incorrect behavior

### 1. Sweep: Frontmatter description contradicts the skill body

**File:** `skills/sweep/SKILL.md:4`

The frontmatter description says:

> "Audit project configuration for bloat, conflicts, and drift. **Do not change anything** — report what needs attention."

But the skill body actively modifies state:

- Step 4 says "Update CLAUDE.md" — adds new temps, deletes resolved temps, updates the status line
- Step 2 prompts the user to mark files as temporary (interactive mutation)
- The skill's own heading says "Mark new temps. Remove obsolete ones. Update project memory."

An AI following the frontmatter will refuse to make changes. An AI following the body will make changes. These are opposite instructions.

### 2. Prep: Name and title are misaligned

**File:** `skills/prep/SKILL.md:2-3`

- Frontmatter `name:` is **prep**
- Frontmatter `description:` says **"Pre-planning alignment between human and AI"**
- The `# heading` says **"Codebase Audit"**
- The body is structured as an 11-phase audit workflow

"Prep" implies preparation/alignment. "Codebase Audit" implies evaluation/assessment. The skill tries to be both, but the title that gets shown to the AI and user says "Codebase Audit" while the trigger keywords reference pre-planning. An AI matching on skill name (`prep`) will expect preparation, not a full audit.

### 3. Contract.md is triplicated across skills

**Files:**
- `skills/preflight/references/contract.md`
- `skills/sitrep/references/contract.md`
- `skills/sweep/references/contract.md`

All three copies are currently byte-identical. But the contract's own first line warns:

> "All three skills MUST adhere to these conventions **to prevent drift**."

Having three copies of the same document is the textbook setup for drift. When one copy gets updated, the others won't be. This is the exact risk the contract warns about, created by the contract's own file structure.

---

## Moderate — Gaps that reduce reliability or completeness

### 4. Prep references `/workflows:plan` which doesn't exist in this repo

**File:** `skills/prep/SKILL.md:3, 8`

Both the frontmatter description and the body reference `/workflows:plan`:

> "Use before /workflows:plan to ensure we're planning the right thing."

There is no `workflows` directory or `plan` skill in this repository. This is a dangling reference to something external. An AI encountering this will either error, hallucinate, or ignore the reference.

### 5. Contract excludes prep and proof

**File:** `skills/*/references/contract.md:2`

The contract says:

> "This document defines the shared conventions for **sitrep, preflight, and sweep** skills."

But `prep` also reads `CLAUDE.md` (Phase 4: "Read... CLAUDE.md") and works with the same project structure. If prep interprets `CLAUDE.md` differently than the contract specifies, the skills will produce inconsistent results. `proof` also writes to project files (Step 4: "Write the proof to the appropriate project file") without knowing the conventions.

### 6. Sitrep: Missing "When to Use" section

**File:** `skills/sitrep/SKILL.md`

Every other skill has an explicit "When to Use" section:
- Preflight: lines 10-19
- Sweep: lines 11-16
- Proof: lines 10-15
- Prep: implied by description (though also missing formal section)

Sitrep has no "When to Use" section. Its trigger conditions exist only in the 1-line frontmatter description. This makes it the hardest skill for an AI to decide when to invoke.

---

## Minor — Style, consistency, or edge-case concerns

### 7. Sweep: Temp marker grep is language-specific

**File:** `skills/sweep/SKILL.md:102`

```bash
grep -r "// TEMPORARY:" --include="*.ts" --include="*.js"
```

The `//` comment syntax and `*.ts`/`*.js` file filter assume TypeScript/JavaScript. If these skills are used in a Python, Ruby, Go, or other project, the marker format and grep pattern won't match. The contract also only shows the `//` style.

### 8. Prep: No standalone examples section

**File:** `skills/prep/SKILL.md`

Preflight, sitrep, sweep, and proof all have top-level `## Examples` sections with complete, copy-paste-ready output blocks. Prep has inline examples embedded within phase descriptions (e.g., the security audit ideal scene) but no standalone examples section showing a complete audit run. For the most complex skill (11 phases), this is where examples would help most.

### 9. Proof: No references directory or contract link

**File:** `skills/proof/`

Proof is the only skill with no `references/` directory and no mention of the shared contract. While proof is more standalone than the other skills, Step 4 says "Write the proof to the appropriate project file or document" — knowing the project's file conventions (from the contract) would help the AI place the proof correctly.

---

## Summary

| # | Severity | Skill | Issue |
|---|----------|-------|-------|
| 1 | Critical | sweep | Frontmatter says "do not change anything" but body mutates CLAUDE.md |
| 2 | Critical | prep | Name says "prep", title says "Codebase Audit", description says "alignment" |
| 3 | Critical | shared | contract.md is triplicated — creates the drift it warns against |
| 4 | Moderate | prep | References `/workflows:plan` which doesn't exist in this repo |
| 5 | Moderate | shared | Contract excludes prep and proof despite shared CLAUDE.md dependency |
| 6 | Moderate | sitrep | Missing "When to Use" section (all other skills have one) |
| 7 | Minor | sweep | Temp marker grep hardcoded to TS/JS |
| 8 | Minor | prep | No standalone examples section for 11-phase workflow |
| 9 | Minor | proof | No references directory or contract link |
