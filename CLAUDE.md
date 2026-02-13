# Teammate

Read `~/.claude/teammate.md` at the start of every session. It defines who you're working with: communication style, thinking patterns, values, constraints, and banned language. These aren't suggestions. They're operating rules.

# Skills

**Live:** `~/.claude/skills/[name]/SKILL.md` — where Claude loads them
**Mirror:** `~/flywheel/skills/[name]/SKILL.md` — version-controlled backup

# Compound Engineering

Each unit of effort should make the next one easier. After completing work, ask: what did we learn that should be folded back into the process, the docs, or the tools? If something is reusable, capture it where it will be found next time — not in a separate file, but in the doc or skill that serves the work.

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
Codex can brainstorm, plan, and implement. Claude Code and Codex may both execute implementation based on task ownership.

## How It Works
- `~/.codex/AGENTS.md` — Codex runtime policy file (permissions + guardrails) that should stay aligned with `CLAUDE.md` and avoid duplicating project rules.
- `~/.codex/config.toml` — `project_doc_fallback_filenames = ["CLAUDE.md"]` makes Codex read project-level `CLAUDE.md` files natively.
- Project repos have NO `AGENTS.md` — Codex reads `./CLAUDE.md` via the fallback config.

## Sync Rule
When you edit any of these files, immediately copy the changed file to its flywheel mirror and commit:

| Live | Flywheel mirror |
|---|---|
| `~/.claude/CLAUDE.md` | `~/flywheel/CLAUDE.md` |
| `~/.claude/teammate.md` | `~/flywheel/teammate.md` |
| `~/.codex/AGENTS.md` | `~/flywheel/codex/AGENTS.md` |
| `~/.claude/skills/**` | `~/flywheel/skills/**` |
| `~/.claude/settings.json` | `~/flywheel/settings.json` |

`~/.codex/AGENTS.md` should be reviewed when CLAUDE.md changes to keep Codex-specific permissions and guardrails aligned.

## Known Codex Behaviors
- Codex does NOT follow cross-file pointers ("read file X" in AGENTS.md is ignored).
- Codex may create rogue files (`~/CLAUDE.md`, project-level `AGENTS.md`, unauthorized `docs/` entries) if guardrails are missing.
- Codex self-reports file paths inaccurately (e.g., reports `~/AGENTS.md` when it loaded `~/.codex/AGENTS.md`).
- Codex may fabricate recovery files or claim actions it didn't take — verify on disk.
