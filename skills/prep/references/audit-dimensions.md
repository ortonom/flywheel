# Audit Dimensions

Reference checklists for defining ideal scene targets. Consult the dimensions relevant to the audit's purposes — not all apply to every audit.

---

## 1. Core Business Logic

### Financial Calculations
- [ ] BigDecimal (or equivalent) for all money — no floating point
- [ ] Rounding strategy documented and consistent
- [ ] Allocation algorithms handle remainders (largest-remainder or equivalent)
- [ ] No penny drift across distributed calculations
- [ ] Constants match documented specifications

### Pure Functions
- [ ] Calculation modules are stateless (no database calls, no side effects)
- [ ] Inputs and outputs clearly typed
- [ ] Easy to test in isolation

### Edge Cases
- [ ] Negative values handled (rejected, zeroed, or documented behavior)
- [ ] Null/nil values coalesced (not silently dropped)
- [ ] Zero values don't cause division errors
- [ ] Boundary values tested (min/max thresholds, cutoffs)
- [ ] Empty collections handled (no errors on empty arrays/hashes)

---

## 2. External API Clients

### Safety
- [ ] Read-only operations enforced at code level (not just convention)
- [ ] Write operations require explicit flags or separate client
- [ ] HTTP methods restricted (e.g., GET-only allowlist)

### Resilience
- [ ] Rate limiting implemented (delays between requests)
- [ ] Pagination handled (not just first page)
- [ ] Timeout configured (not relying on library defaults)
- [ ] Retry logic with backoff for transient failures
- [ ] Error responses parsed and surfaced clearly

### Security
- [ ] SSL/TLS verification enabled (flag any `VERIFY_NONE`)
- [ ] API keys/tokens not hardcoded
- [ ] Auth credentials in environment variables or secrets manager

### Compatibility
- [ ] Multiple API response formats handled (version differences)
- [ ] Response wrapper variations accounted for
- [ ] Graceful handling of unexpected fields or missing fields

---

## 3. Data Pipelines / Sync Services

### Idempotency
- [ ] Running sync twice produces same result
- [ ] Upsert logic uses proper unique keys
- [ ] No duplicate records created on re-run

### Incremental Sync
- [ ] Uses timestamps (`updated_at`, `modified_since`) to avoid full re-sync
- [ ] Handles clock skew or timezone differences
- [ ] Falls back to full sync when incremental state is lost

### Data Normalization
- [ ] Handles format variations in source data (arrays vs objects, nested vs flat)
- [ ] ID normalization consistent across services (string vs integer, prefixes)
- [ ] Case handling explicit (case-insensitive matching where needed)

### Orphan Detection
- [ ] References to deleted/missing parent records detected
- [ ] Orphaned child records handled (warned, skipped, or cleaned)
- [ ] Foreign key violations caught before they hit the database

### Classification / Categorization
- [ ] Auto-classification rules documented
- [ ] Keyword matching is case-insensitive with documented terms
- [ ] Fallback category exists for unmatched records

---

## 4. Orchestration / Pipelines

### Atomicity
- [ ] Multi-step operations wrapped in transactions
- [ ] Failure at any step rolls back all changes
- [ ] No partial state on error

### Locking / Finalization
- [ ] Finalized/locked records cannot be modified without explicit unlock
- [ ] Lock status checked before write operations
- [ ] Unlock requires explicit action (not automatic)

### Sequencing
- [ ] Pipeline follows load → validate → process → save order
- [ ] Dependencies between steps are explicit
- [ ] No implicit ordering assumptions

### Concurrency
- [ ] Concurrent execution of same operation prevented (advisory locks, mutex)
- [ ] Transaction isolation level appropriate for the workload
- [ ] Race conditions identified and documented (even if low-risk)

---

## 5. Schema Design

### Indexes
- [ ] Unique indexes on natural keys (not just surrogate keys)
- [ ] Composite indexes for common query patterns
- [ ] Foreign key columns indexed
- [ ] No missing indexes on frequently filtered columns

### Constraints
- [ ] NOT NULL on required fields
- [ ] Check constraints on enum/status fields
- [ ] Foreign key constraints with appropriate cascade behavior
- [ ] Default values where sensible

### Types
- [ ] Decimal/numeric for money (not float)
- [ ] Appropriate string lengths
- [ ] Timestamps with timezone where needed
- [ ] Boolean fields have defaults

---

## 6. Test Suite Alignment

### Coverage
- [ ] Core business logic has unit tests
- [ ] Edge cases explicitly tested (not just happy path)
- [ ] Integration tests cover full pipelines

### Correctness
- [ ] Test expectations match documented business rules
- [ ] Test expectations match actual implementation behavior
- [ ] No tests asserting wrong behavior that happens to pass

### Maintenance
- [ ] Tests are independent (no order dependency)
- [ ] Test data is self-contained (factories/fixtures, not shared state)
- [ ] Failing tests have clear error messages

---

## Severity Classification

When reporting issues:

- **Critical** — Data loss, incorrect calculations, or security breach
- **Major** — Failures under conditions likely to occur
- **Minor** — Code quality, maintainability, unlikely edge cases
- **Documented** — Known tradeoff with existing justification
