---
name: prep
description: Pre-planning alignment between human and AI. Use before /workflows:plan to ensure we're planning the right thing. Establishes goal, purposes, ideal scene, and targets through discovery — not assumptions. Triggers on "audit this codebase", "is this production-ready", "what's the state of this system", or when alignment is needed before planning begins.
---

# Codebase Audit

Human-AI alignment before planning. Establish what exists, what should exist, and whether we're focused on the right problem — so `/workflows:plan` plans the right thing.

## Phase 1: Scope

Ask: **Entire codebase or specific area?**

If specific: what area and why?

## Phase 2: Goal and Purposes

**Goal:** What outcome? (singular)
- "Know if it's production-ready"
- "Find what's broken"
- "Verify this security control works"
- "Understand what I'm inheriting"

**Purpose(s):** Why does the goal matter? What does it serve? (multiple)
- Fundraising due diligence?
- Handoff to another team?
- Something feels wrong?
- Evaluate contractor work?
- Prevent specific failure mode?

Purposes shape which dimensions matter and how much.

## Phase 3: Alignment Check

Before proceeding, verify alignment:

1. Is this the right **goal**? Or is there a better framing?
2. Do the **purposes** actually require this goal? Or is there a simpler path?
3. Are we solving the right **problem**?

If misaligned → surface it, propose reframe, get agreement before continuing.

## Phase 4: Existing Scene

What's actually here? Read and observe:

1. Project root — framework, language, structure
2. Documentation — README, CLAUDE.md, specs for business rules
3. Schema — migrations, models, data relationships
4. Test suite — run it, capture results (passes, failures, errors)
5. Core code — in order of relevance to the purposes

Read with the purposes in mind. Not everything matters equally.

## Phase 5: Ideal Scene

**Ask: What does the ideal scene look like?**

The ideal scene is what the area *ought* to be — a clear picture of the desired state. Describe it so someone can see it.

Example (from security audit):
> "AI cannot modify production data in Hostify. The system is read-only by design, enforced at multiple layers. Any attempt to write is blocked before it reaches the network."

The ideal scene is qualitative — the vision of how things should work.

## Phase 6: Targets

**Ask: What confirms we've reached the ideal scene?**

Targets are the concrete, verifiable criteria that prove the ideal scene exists. Each target must be:
- **Verifiable** — can prove it's met or not
- **Terminable** — has a clear done state
- **Derived from the ideal scene** — confirms the picture

Example (same security audit):
| Target | What it confirms |
|--------|------------------|
| T1: No write methods exist in client | "read-only by design" |
| T2: Runtime check blocks non-GET | "enforced at multiple layers" |
| T3: No bypass via raw HTTP elsewhere | "any attempt blocked" |
| T4: Spec fails if write methods added | enforcement is tested |

**Bad target:** "Code should be secure."
**Good target:** "Client only exposes GET methods; spec fails if POST/PUT/PATCH/DELETE added."

Consult `references/audit-dimensions.md` for dimension-specific checklists when defining targets.

## Phase 7: Gap Analysis

For each target:

1. **Status** — Met, partial, or gap?
2. **Evidence** — What proves it?
3. **Impact** — How much does this gap matter given the purposes?

Map dependencies: what affects what, to what degree?

## Phase 8: Plan

Work backwards from ideal scene to existing scene:

1. What must change to close each gap?
2. What's the sequence? (dependencies)
3. What's the priority? (impact × effort)

## Phase 9: Simulate

Run the plan forward mentally:

1. If we execute these steps, do we reach the ideal scene?
2. Does achieving the targets actually serve the purposes?
3. Does serving the purposes achieve the goal?

If simulation fails → revise plan, return to Phase 8.

## Phase 10: Execute

Run the plan. For an audit, this means:

- Verify each target with evidence
- Document findings as discovered
- Note anything unexpected

## Phase 11: Feedback

Deliver the valuable final product — a report answering the goal.

```
## Audit Report: [Subject]

### Summary
[1-2 sentences. What was audited. Verdict.]

### Goal
[What we set out to determine]

### Purposes
[Why it mattered]

### Targets Assessed
| Target | Status | Evidence |
|--------|--------|----------|
| ... | ✅/⚠️/❌ | ... |

### Gaps Found
[If any — what, why it matters, remediation]

### Strengths
[What's well-built — be specific]

### Remaining Risk
[What's not covered, out of scope, or residual]

### Verdict
[Direct answer to the goal]
```

---

## Principles

- **Purposes drive depth.** Not all dimensions apply equally. Weight effort by relevance.
- **Targets must be concrete.** Vague ideals can't be verified.
- **Simulate before executing.** Catch flawed plans before wasting effort.
- **Acknowledge what works.** Strengths section is not optional.
- **Surface misalignment early.** Don't audit the wrong thing.
