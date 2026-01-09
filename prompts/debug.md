# Debugging Prompt

Help me debug this issue systematically.

## Gathering Information

1. **What's the symptom?** - Exact error message or unexpected behavior
2. **When does it happen?** - Steps to reproduce
3. **When did it start?** - Recent changes that might be related
4. **What have you tried?** - Avoid re-treading ground

## Investigation Approach

1. **Reproduce** - Confirm I can see the problem
2. **Isolate** - Find the smallest case that shows the issue
3. **Trace** - Follow the data/control flow to the failure point
4. **Hypothesize** - Form theory about root cause
5. **Test** - Verify hypothesis with targeted check
6. **Fix** - Make minimal change to address root cause
7. **Verify** - Confirm fix works and didn't break anything else

## Common Gotchas

- Is it actually running the code you think it is?
- Are the inputs what you expect?
- Any caching involved?
- Environment differences (dev vs prod)?
- Race conditions or timing issues?
