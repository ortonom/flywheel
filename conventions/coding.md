# Coding Conventions

## General Principles

- **Simplicity over cleverness** - Write code that's easy to understand
- **Minimal changes** - Only modify what's necessary to solve the problem
- **No gold-plating** - Don't add features, refactoring, or "improvements" beyond what was asked

## Code Style

- Follow the existing conventions of whatever codebase I'm working in
- Prefer explicit over implicit
- Avoid premature abstractions - three similar lines are often better than a helper function

## Documentation

- Don't add comments unless the logic isn't self-evident
- Don't add docstrings to code you didn't significantly change
- README updates only when explicitly requested

## Error Handling

- Don't add defensive error handling for scenarios that can't happen
- Trust internal code and framework guarantees
- Only validate at system boundaries (user input, external APIs)

## Testing

- Write tests when asked or when making significant logic changes
- Match the testing style already used in the project
- Don't over-test trivial code
