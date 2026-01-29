---
name: proof
description: This skill should be used when a data discrepancy or process issue is discovered that needs to be presented to a stakeholder (accountant, client, team lead) in a meeting. It builds a structured, numbered proof that opens with a confirmation question, derives the conclusion step by step, and ends with next actions. Triggers on "/proof", "build a proof", "prove this to the accountant", or when presenting a data finding to a non-technical stakeholder.
---

# Proof

Build a structured proof for presenting a data discrepancy or process issue to a stakeholder. The goal is alignment, not blame.

## When to Use

- A data discrepancy has been identified (report numbers don't match expected values)
- The root cause is a process or methodology issue, not a code bug
- The finding needs to be presented to a stakeholder in a meeting
- The audience may not be technical (accountant, client, operations lead)

## Workflow

### Step 1: Gather the Evidence

Before building the proof, identify:

1. **The discrepancy** — what number is wrong, and what should it be?
2. **A concrete example** — one specific record that demonstrates the gap (e.g., Unit 909: report shows $901.84, correct value is $1,072.45)
3. **The suspected cause** — what process or field is being used incorrectly?

Ask the user for these if not already known. Do not proceed without a concrete example.

### Step 2: Build the Proof

Write the proof as a single numbered sequence using these sections in order:

**Confirm:**
Open with a plain-language question. No jargon, no technical field names. Frame it as two options — what they might be doing vs what they should be doing.

Follow with a blockquote stating the evidence for what is believed, citing the concrete example.

```
**Confirm:**
1. [Plain-language question with two options]

> We believe [option], based on [concrete example with specific numbers].
```

**If confirmed, then:**
Establish definitions and derive the conclusion. Each step references prior steps by number.

- Step 2-3: Define the two values (what they're using vs what's correct)
- Step 4: State the principle (what the metric should reflect)
- Step 5: Derive the conclusion (from steps 2, 3, 4)

```
**If confirmed, then:**
2. [Definition of what they're using]
3. [Definition of what's correct]
4. [The principle — what should be measured]
5. Therefore [conclusion] (from 2, 3, 4)
```

**What changed:** (include only if magnitude shifted over time)
Show before/after with specific percentages or amounts.

```
**What changed:**
6. Before [date]: [old condition] → [old impact]
7. After [date]: [new condition] → [new impact]
```

**Conclusion:**
Summarize the finding, referencing prior steps. Trace the downstream impact chain.

```
**Conclusion:**
8. [Summary of the error and its timeline] (from 5, 6, 7)
9. [Current state and scope]
10. [Downstream impact chain: A → B → C → D]
```

**Next steps:**
One line. What happens after the stakeholder confirms.

```
**Next steps:**
11. [Stakeholder confirms] → [process changes] → [remediation if needed]
```

### Step 3: Review the Proof

Check the proof against these criteria:

- [ ] Opens with a question, not an accusation
- [ ] The question uses plain language (no field names or API jargon)
- [ ] Every step after the question references which prior steps it derives from
- [ ] Technical references (field names, formulas) appear only in the logical steps, not the opening
- [ ] A concrete example with real numbers supports the claim
- [ ] The proof is self-contained — readable without other documents
- [ ] Ends with a clear next action

### Step 4: Place the Proof

Write the proof to the appropriate project file or document. The proof should be a standalone section, not buried inside other content.

## Example

From a real accountant meeting preparation:

```markdown
## Why `paid_sum` ≠ `base_price` — Proof

**Confirm:**
1. What number are you using for Rental Revenue on Airbnb bookings — the amount Airbnb deposits or the full room charge?

> We believe it's the deposit amount, based on Unit 909: the report shows $901.84, which matches what Airbnb deposited, not the full room charge of $1,072.45.

**If confirmed, then:**
2. `paid_sum` = what Airbnb deposits after taking their cut
3. `base_price` = the full room charge the guest pays for accommodation
4. Rental Revenue should reflect what was charged, not what Airbnb forwarded
5. Therefore `paid_sum` is never the correct number for Rental Revenue (from 2, 3, 4)

**What changed:**
6. Before Oct 27, 2025: Airbnb took 3% → `paid_sum` ≈ 97% of `base_price` → revenue understated by 3%
7. After Oct 27, 2025: Airbnb takes 15.5% → `paid_sum` ≈ 84.5% of `base_price` → revenue understated by 15.5%

**Conclusion:**
8. The error predates October 2025 — it was 3%, now it's 15.5% (from 5, 6, 7)
9. December 2025 falls entirely under the new model, so every OTA booking's revenue is understated by 15.5%
10. This flows through: Rental Revenue → Total Gross → Pool Net Income → Unit Allocation → Investor Distributions

**Next steps:**
11. Accountant confirms → spreadsheet methodology changes → past reports may need restatement
```

## Guidelines

- Never skip the confirmation step. The proof derives from the stakeholder's own answer.
- Keep the numbered sequence continuous. Do not restart numbering between sections.
- "What changed" is optional — only include when magnitude shifted over time.
- The downstream chain in the conclusion should use arrows (→) to show the flow.
- One proof per issue. Do not combine multiple discrepancies into one proof.
