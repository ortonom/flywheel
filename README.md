# Flywheel

Version-controlled backup and sync hub for AI agent configuration. One repo, two agents, single source of truth.

```
                          ┌─────────────────────────┐
                          │   ~/.claude/CLAUDE.md    │
                          │   (source of truth)      │
                          └────────┬────────┬────────┘
                                   │        │
                      ┌────────────┘        └────────────┐
                      ▼                                  ▼
           ┌─────────────────────┐            ┌─────────────────────┐
           │  ~/.codex/AGENTS.md │            │  ~/flywheel/        │
           │  (inlined copy for  │            │  (git-tracked       │
           │   Codex to read)    │            │   backup)           │
           └─────────┬───────────┘            └─────────────────────┘
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
   ┌─────────────┐     ┌──────────────┐
   │ Codex CLI   │     │ Claude Code  │◄── reads ~/.claude/ directly
   │ brainstorm  │     │ implement    │
   │ + plan ONLY │     │ + deploy     │
   └──────┬──────┘     └──────┬───────┘
          │                   │
          └───────┬───────────┘
                  ▼
        ┌──────────────────┐
        │  ./CLAUDE.md     │  ◄── project-level rules
        │  (per-repo)      │      read by BOTH agents
        └──────────────────┘
```

## Why This Exists

Claude Code and OpenAI Codex run in parallel on the same projects. Their instruction files live in dot-directories (`~/.claude/`, `~/.codex/`) that aren't git-tracked. If an agent overwrites or corrupts an instruction file, it's gone.

Flywheel solves this: every instruction file and skill has a git-tracked mirror here. If something breaks, `git log` and `git show` recover it.

## Architecture

```
~/.claude/CLAUDE.md          ←→  flywheel/CLAUDE.md           (global rules)
~/.claude/skills/*/SKILL.md  ←→  flywheel/skills/*/SKILL.md   (skills)
~/.codex/AGENTS.md           ←→  flywheel/codex/AGENTS.md     (Codex inlined rules)
```

### Source of Truth

`~/.claude/CLAUDE.md` is the single source of truth for all cross-project rules. Everything else derives from it:

- `~/.codex/AGENTS.md` — inlined copy (Codex can't follow cross-file pointers)
- `~/flywheel/CLAUDE.md` — git-tracked backup
- `~/flywheel/codex/AGENTS.md` — git-tracked backup of the Codex copy
- Project-level `./CLAUDE.md` — project-specific rules (read by both agents)

### Agent Roles

| Agent | Role | Reads |
|-------|------|-------|
| **Claude Code** | Implementation, review, testing, deployment | `~/.claude/CLAUDE.md` + `./CLAUDE.md` |
| **OpenAI Codex** | Brainstorming and plan creation ONLY | `~/.codex/AGENTS.md` + `./CLAUDE.md` (via fallback config) |

Codex does not implement. Claude Code does not brainstorm plans without being asked.

## Repo Structure

```
flywheel/
├── CLAUDE.md              # Mirror of ~/.claude/CLAUDE.md
├── settings.json          # Claude Code permissions and plugin config
├── codex/
│   └── AGENTS.md          # Mirror of ~/.codex/AGENTS.md
└── skills/
    ├── preflight/         # Pre-implementation readiness check
    │   ├── SKILL.md
    │   └── references/
    ├── prep/              # Pre-planning alignment
    │   ├── SKILL.md
    │   └── references/
    ├── proof/             # Structured proof for stakeholders
    │   └── SKILL.md
    ├── sitrep/            # Project status report
    │   ├── SKILL.md
    │   └── references/
    └── sweep/             # Config audit for bloat and drift
        ├── SKILL.md
        └── references/
```

## Sync Protocol

When `~/.claude/CLAUDE.md` changes:

1. Copy to `~/flywheel/CLAUDE.md`
2. Sync relevant rules into `~/.codex/AGENTS.md` (keep tool mapping block unchanged)
3. Copy to `~/flywheel/codex/AGENTS.md`
4. Commit and push flywheel

When a skill changes in either location, sync the other:
- Live: `~/.claude/skills/[name]/SKILL.md`
- Mirror: `~/flywheel/skills/[name]/SKILL.md`

## Codex Configuration

Two files outside this repo make Codex read the shared instruction files:

**`~/.codex/config.toml`** (symlinked to iCloud):
```toml
project_doc_fallback_filenames = ["CLAUDE.md"]
```
This makes Codex read project-level `CLAUDE.md` files natively — no `AGENTS.md` in project repos.

**`~/.codex/AGENTS.md`** (mirrored here as `codex/AGENTS.md`):
Contains inlined global rules + guardrails + tool mapping. Codex cannot follow cross-file pointers, so the rules from `~/.claude/CLAUDE.md` must be copied directly into this file.

## Known Codex Behaviors

Documented here because they affect how you maintain the sync:

- Codex does NOT follow cross-file pointers. "Read file X" in AGENTS.md is acknowledged but the file contents are not loaded.
- Codex may create rogue files (`~/CLAUDE.md`, project-level `AGENTS.md`, unauthorized docs) if guardrails are missing.
- Codex self-reports file paths inaccurately (e.g., reports `~/AGENTS.md` when it loaded `~/.codex/AGENTS.md`).
- Codex may fabricate file names or claim actions it didn't take. Always verify on disk.

## Recovery

If an instruction file is lost or corrupted:

```bash
# See what it looked like at any point in history
cd ~/flywheel
git log --oneline -- CLAUDE.md
git show <commit>:CLAUDE.md

# Restore to live location
cp ~/flywheel/CLAUDE.md ~/.claude/CLAUDE.md
cp ~/flywheel/codex/AGENTS.md ~/.codex/AGENTS.md
```
