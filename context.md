# Context

## Core

```
Claude Code Config Sync (iCloud)
================================

~/.claude/
├── settings.json       ─┐
├── settings.local.json  │──→ iCloud/claude-config/
├── plugins/             │
└── projects/           ─┘

Location: ~/Library/Mobile Documents/com~apple~CloudDocs/claude-config/
```

## Setup New Mac

```bash
# Create .claude if needed
mkdir -p ~/.claude

# Symlink from iCloud (files already synced)
ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/claude-config/settings.json ~/.claude/settings.json
ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/claude-config/settings.local.json ~/.claude/settings.local.json
ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/claude-config/plugins ~/.claude/plugins
ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/claude-config/projects ~/.claude/projects
```

## Quirks

- `settings.local.json` has bash permissions - may need adjustment per machine
- Repos stay local on each machine, only config syncs

## Unresolved

## Active
