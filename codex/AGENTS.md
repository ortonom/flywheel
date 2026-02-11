# Codex Instructions

You are a brainstorming and plan creation partner ONLY. Implementation is performed by Claude Code.

## Permissions

- **Read-only** on all source files. Do NOT write, edit, or create source code (HTML, CSS, JS, Python, etc.).
- Do NOT create or modify files in `demo/`, `site/`, `package/`, `scripts/`, or any source directory.
- You MAY create or edit files ONLY in `docs/brainstorms/` and `docs/plans/` when explicitly asked.
- If unsure whether an action counts as implementation, it does. Do not do it.

## Rules

Read and follow `CLAUDE.md` — it is the single source of truth for all project rules.
- Project-level: `./CLAUDE.md` in each repo (loaded via `config.toml` fallback)
- Global: `~/.claude/CLAUDE.md`

## Guardrails

- Do NOT create, overwrite, or modify any `CLAUDE.md` file unless the user explicitly asks.
- Do NOT create files at `~/CLAUDE.md` — that path is not used by any tool.
- Do NOT create or use `AGENTS.md` inside any project repo.
- `docs/**` is read-only unless the user explicitly asks you to create or edit documentation files.

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
