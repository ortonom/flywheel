# Skills

**Live:** `~/.claude/skills/[name]/SKILL.md` — where Claude loads them
**Mirror:** `~/flywheel/skills/[name]/SKILL.md` — version-controlled backup

When a skill is updated in either location, sync the other to match.

# Workflow

Before non-trivial implementation → run `/preflight`.

## Pacing

Default: execute plans autonomously.

If the user says **"one by one"**, **"step by step"**, or **"walk me through it"**:
- Do one step only
- Explain what you did and why
- Wait for approval before the next step

This applies even in bypass permissions mode. The user controls pacing regardless of permission settings.

To resume autonomous execution: **"run it"**, **"go"**, or **"YOLO"**.

## Working Principles

- **One objective per change.** Don't mix behavior, style, and architecture in one edit. Checkpoint after each stable improvement so any bad idea can be reverted without losing good work.

Other principles live in the skills that enforce them:
- Intent before implementation, scope every request → `/preflight`
- Role boundary, product orientation → `/prep`

## Cleanup
- **Delete implemented plans.** After a plan in `docs/plans/` is fully implemented and merged, delete it. Solutions docs (`docs/solutions/`) capture learnings; plans are scaffolding.

## Git Commits

Multiple Claude sessions may run in parallel on the same repo. When committing:
- Only stage files YOU changed in THIS session. NEVER use `git add -A` or `git add .`.
- Always add specific files by name.
- If unsure which files you touched, ask the user.

Do not create or use AGENTS.md/AGENT.md; CLAUDE.md is the single source of truth to prevent instruction drift.

For every repo: read ~/.claude/CLAUDE.md first, then ./CLAUDE.md if present (project file overrides master).

# Codex Coexistence

OpenAI Codex runs alongside Claude Code. Both agents share the same instruction source of truth.

## Codex Role
Codex is a brainstorming and plan creation partner ONLY. Implementation is performed by Claude Code.

## How It Works
- `~/.codex/AGENTS.md` — minimal file: read-only permissions, guardrails, pointer to follow `CLAUDE.md`. Does NOT inline rules.
- `~/.codex/config.toml` — `project_doc_fallback_filenames = ["CLAUDE.md"]` makes Codex read project-level `CLAUDE.md` files natively.
- Project repos have NO `AGENTS.md` — Codex reads `./CLAUDE.md` via the fallback config.

## Sync Rule
When this file (`~/.claude/CLAUDE.md`) changes, sync to:
- `~/flywheel/CLAUDE.md` — version-controlled backup (git-tracked)

`~/.codex/AGENTS.md` does NOT need syncing — it just points Codex to `CLAUDE.md`.

## Known Codex Behaviors
- Codex does NOT follow cross-file pointers ("read file X" in AGENTS.md is ignored).
- Codex may create rogue files (`~/CLAUDE.md`, project-level `AGENTS.md`, unauthorized `docs/` entries) if guardrails are missing.
- Codex self-reports file paths inaccurately (e.g., reports `~/AGENTS.md` when it loaded `~/.codex/AGENTS.md`).
- Codex may fabricate recovery files or claim actions it didn't take — verify on disk.
