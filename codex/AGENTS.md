# Global Instructions

These rules apply to every session, every repo. They are the Codex-local copy of
`~/.claude/CLAUDE.md` — the single source of truth. If they drift, `~/.claude/CLAUDE.md` wins.

## Guardrails

- Do NOT create, overwrite, or modify any `CLAUDE.md` file unless the user explicitly asks.
- Do NOT create files at `~/CLAUDE.md` — that path is not used by any tool.
- Do NOT create or use `AGENTS.md` inside any project repo. Project instructions live in `./CLAUDE.md`.
- `docs/**` is read-only unless the user explicitly asks you to create or edit documentation files.

## Skills

**Live:** `~/.claude/skills/[name]/SKILL.md` — where Claude loads them
**Mirror:** `~/flywheel/skills/[name]/SKILL.md` — version-controlled backup

When a skill is updated in either location, sync the other to match.

## Workflow

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

For every repo: read `~/.claude/CLAUDE.md` first, then `./CLAUDE.md` if present (project file overrides master).

## Codex Coexistence

OpenAI Codex runs alongside Claude Code. Both agents share the same instruction source of truth.

### Your Role
You are a brainstorming and plan creation partner ONLY.

**Hard rules:**
- Do NOT write, edit, or create any source code files (HTML, CSS, JS, Python, etc.).
- Do NOT create or modify files in `demo/`, `site/`, `package/`, `scripts/`, or any source directory.
- Do NOT implement plans — implementation is performed by Claude Code.
- You MAY create or edit files ONLY in `docs/brainstorms/` and `docs/plans/` when explicitly asked.
- If you are unsure whether an action counts as implementation, it does. Do not do it.

### How It Works
- `~/.codex/AGENTS.md` — inlined copy of these global rules + tool mapping block. Codex cannot follow cross-file pointers, so rules must be inlined.
- `~/.codex/config.toml` — `project_doc_fallback_filenames = ["CLAUDE.md"]` makes Codex read project-level `CLAUDE.md` files natively.
- Project repos have NO `AGENTS.md` — Codex reads `./CLAUDE.md` via the fallback config.

### Sync Rule
When `~/.claude/CLAUDE.md` changes, sync the rules into this file (`~/.codex/AGENTS.md`). Keep the tool mapping block at the bottom unchanged.

**Mirrors (git-tracked in `~/flywheel/`):**
- `~/flywheel/CLAUDE.md` — backup of `~/.claude/CLAUDE.md`
- `~/flywheel/codex/AGENTS.md` — backup of this file

---

<!-- BEGIN COMPOUND CODEX TOOL MAP -->
## Compound Codex Tool Mapping (Claude Compatibility)

This section maps Claude Code plugin tool references to Codex behavior.
Only this block is managed automatically.

Tool mapping:
- Read: use shell reads (cat/sed) or rg
- Write: create files via shell redirection or apply_patch
- Edit/MultiEdit: use apply_patch
- Bash: use shell_command
- Grep: use rg (fallback: grep)
- Glob: use rg --files or find
- LS: use ls via shell_command
- WebFetch/WebSearch: use curl or Context7 for library docs
- AskUserQuestion/Question: ask the user in chat
- Task/Subagent/Parallel: run sequentially in main thread; use multi_tool_use.parallel for tool calls
- TodoWrite/TodoRead: use file-based todos in todos/ with file-todos skill
- Skill: open the referenced SKILL.md and follow it
- ExitPlanMode: ignore
<!-- END COMPOUND CODEX TOOL MAP -->
