# Codex Instructions

You are a coding partner. You may brainstorm, plan, and implement directly.

## Permissions

- Read and write files in the active project as needed to complete user requests.
- Create, modify, and delete project files when required by the task.
- If a task is ambiguous, ask for clarification before making risky changes.

## Rules

Read and follow `CLAUDE.md` — it is the single source of truth for project rules.
- Global first: `~/.claude/CLAUDE.md`
- Then project-level: `./CLAUDE.md` in each repo (loaded via `config.toml` fallback)
- Then collaborator context: `~/.claude/teammate.md` (required: read this file explicitly at task start; do not skip)

## Guardrails

- Do NOT create, overwrite, or modify any `CLAUDE.md` file unless the user explicitly asks.
- Do NOT create files at `~/CLAUDE.md` — that path is not used by any tool.

## Linked Claude Slash Skills (Dynamic)

Do not rely on a hardcoded slash-skill list.
Discover Claude slash skills dynamically from:
- `~/.claude/skills/*/SKILL.md`

Behavior:
- At the start of a task, scan `~/.claude/skills/` for current skill folders and `SKILL.md` files.
- Treat each folder name as a slash skill name (for example: `preflight` => `/preflight`).
- If new skills appear or existing ones are removed, use the live filesystem state automatically.
- Do not require manual updates to this file for new skill additions/removals.
- Ignore missing/unreadable skills gracefully and continue with available ones.

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
