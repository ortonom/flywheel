---
name: preflight
description: This skill should be used before starting non-trivial work to verify readiness. It asks three critical questions about reproducibility, test data, and requirement clarity. If any answer is concerning, it stops work and requires clarification first. Triggers on "/preflight", before implementing features, or when planning significant changes.
---

# Preflight

Verify readiness before building. Stop if foundations are shaky.

## When to Use

Invoke before any non-trivial work:

- Implementing new features
- Refactoring existing code
- Building integrations
- Any work that will take more than a few minutes

Skip for trivial changes (typo fixes, simple config changes).

## The Three Questions

Ask and answer each question. Be honest.

### 1. Can we reproduce existing output from inputs?

**Yes**: Given the same inputs, the current system produces consistent, known outputs.

**No**: Outputs are unpredictable, undocumented, or depend on hidden state.

**Concerning if No**: Building on top of unreproducible behavior compounds uncertainty.

### 2. Do we have verified data to test against?

**Yes**: Golden data, test fixtures, or known-good examples exist for validation.

**No**: No reference data to compare against.

**Concerning if No**: How will correctness be verified?

### 3. Are requirements clear or assumed?

**Clear**: Requirements are documented or explicitly stated by the user.

**Assumed**: Requirements are inferred or guessed.

**Concerning if Assumed**: Assumptions may be wrong. Clarify before building.

## Workflow

### 1. Evaluate Each Question

For each question, determine the answer based on:

- What's documented in CLAUDE.md
- What exists in the codebase (tests, fixtures, golden data)
- What the user has explicitly stated

### 2. Output Assessment

Format:

```
PREFLIGHT CHECK
1. Reproducible outputs? [Yes/No] — [brief evidence]
2. Verified test data? [Yes/No] — [brief evidence]
3. Requirements clear? [Clear/Assumed] — [brief evidence]

[READY TO PROCEED or STOP — CLARIFY FIRST]
```

### 3. If Concerning

If any answer is concerning, output:

```
STOP — CLARIFY FIRST

Before proceeding, resolve:
- [List specific questions or gaps]
```

Do not proceed with implementation until clarified.

## Examples

**Ready to proceed:**
```
PREFLIGHT CHECK
1. Reproducible outputs? Yes — lib/ has 119 passing tests
2. Verified test data? Yes — verification/golden/ contains December report
3. Requirements clear? Clear — CLAUDE.md documents fee structure

READY TO PROCEED
```

**Not ready:**
```
PREFLIGHT CHECK
1. Reproducible outputs? No — no tests for allocation logic
2. Verified test data? Yes — golden data exists
3. Requirements clear? Assumed — penalty redistribution rules unclear

STOP — CLARIFY FIRST

Before proceeding, resolve:
- How should penalties redistribute when all units are blocked?
- Should allocation tests be written first?
```

## Contract

This skill follows the shared contract in `references/contract.md`.
