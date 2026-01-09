# Code Review Prompt

Review this code with focus on:

## Correctness
- Does it do what it's supposed to do?
- Edge cases handled?
- Error conditions considered?

## Security
- Input validation at boundaries?
- No injection vulnerabilities?
- Secrets properly handled?

## Maintainability
- Clear intent?
- Appropriate complexity for the problem?
- Would future-me understand this?

## Performance
- Any obvious inefficiencies?
- Appropriate data structures?
- (Only flag if actually problematic)

## Output Format

For each issue found:
1. Location (file:line)
2. Severity (critical/important/minor/nitpick)
3. What's wrong
4. Suggested fix

End with: "X issues found (Y critical, Z important)"
