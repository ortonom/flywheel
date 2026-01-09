# Refactoring Prompt

Guide for safe refactoring.

## Before Starting

1. **Why refactor?** - Clear goal (not just "cleaner")
2. **What's the scope?** - Define boundaries
3. **Test coverage?** - How will we know it still works?

## Refactoring Principles

- **One thing at a time** - Don't mix refactoring with feature work
- **Small steps** - Each change should be independently verifiable
- **Tests pass after each step** - Never break the build mid-refactor
- **Preserve behavior** - Refactoring changes structure, not functionality

## Common Refactors

- Extract function/method
- Inline function/method
- Rename for clarity
- Move to better location
- Simplify conditional
- Remove duplication (only if truly duplicated, not coincidentally similar)

## Red Flags

- "While I'm here, I'll also..."
- Changing behavior "because it's better"
- Large PRs mixing refactoring and features
- No way to verify correctness
