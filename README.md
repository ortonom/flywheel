# Flywheel

Personal intelligence layer for Claude Code. Contains working conventions, reusable prompts, and accumulated learnings that apply across all projects.

## Why Flywheel?

Browser-based Claude Code starts each session fresh. Flywheel bridges that gap:

- **One brain, many projects** - Maintain consistent conventions everywhere
- **Rolling context** - Session handoffs simulate memory across sessions
- **Compound learnings** - Discoveries in one project benefit all future work

## Structure

```
flywheel/
├── CLAUDE.md          # Entry point - Claude reads this automatically
├── conventions/       # How you like to work
│   ├── coding.md     # Code style preferences
│   ├── communication.md  # How Claude should interact
│   └── workflow.md   # Development patterns
├── prompts/          # Reusable prompt templates
│   ├── code-review.md
│   ├── debug.md
│   ├── refactor.md
│   └── session-handoff.md
├── learnings/        # Accumulated knowledge
│   └── *.md         # Insights worth preserving
└── sessions/         # Session continuity
    └── current.md   # Active working context
```

## Usage

### In This Repo

Work directly here when developing the Flywheel itself.

### In Other Projects

**Option 1: Reference manually**
At session start, tell Claude: "Read my Flywheel at [path/url] to understand my preferences."

**Option 2: Git submodule**
```bash
git submodule add <flywheel-repo-url> .flywheel
```
Then in your project's CLAUDE.md:
```markdown
See .flywheel/CLAUDE.md for my conventions and preferences.
```

**Option 3: Copy CLAUDE.md content**
Copy relevant sections from Flywheel's CLAUDE.md into project-specific CLAUDE.md files.

## Session Workflow

### Starting
1. Open `sessions/current.md`
2. Read recent context
3. Pick up where you left off

### Ending
1. Update `sessions/current.md` with what happened
2. Note next steps specifically
3. Add any new learnings to `learnings/`

## Customization

This is your personal system. Adjust everything:

- **conventions/** - Match your actual coding style
- **prompts/** - Add templates for your common tasks
- **learnings/** - Build your knowledge base
- **sessions/current.md** - Format that works for your brain

## Philosophy

- Simple beats comprehensive
- Specific beats abstract
- Used beats perfect
- Updated beats stale

The best Flywheel is one you actually use. Start minimal, add what you need.
