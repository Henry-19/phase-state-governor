# Phase Pipeline Templates & Schemas

This reference provides standardized markdown schemas for maintaining state, tracking phase progress, verifying spec integrity, and logging rollback events.

---

## 1. Canonical State File Template (`.agent/state.md`)

Use this exact structure when initializing or maintaining the project's single source of truth:

```markdown
# Agent Execution State & Phase Tracker

## Metadata
- **Project Name:** [Insert Project Name]
- **Target Specification:** `docs/spec.md`
- **Spec Integrity Hash (SHA-256):** `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`
- **Current Phase:** Phase 2: Core Domain Logic
- **Execution Status:** IN_PROGRESS <!-- [NOT_STARTED | IN_PROGRESS | BLOCKED | COMPLETED] -->
- **Last Updated:** 2026-11-20T18:30:00Z

---

## Phase Pipeline Matrix

### Phase 0: Specification & Validation
- [x] Input specification parsed and validated
- [x] Scope boundary defined; out-of-scope items identified
- [x] Baseline SHA-256 computed and recorded
- **Gate 0 Sign-Off:** PASSED (2026-11-20)

### Phase 1: Legal, Compliance & Dependencies
- [x] External dependencies audited against license whitelist (MIT, Apache-2.0, BSD)
- [x] Zero-PII check: Mock schemas and test fixtures use synthetic data only
- [x] Secrets hygiene: No hardcoded API keys or passwords; `.env.example` verified
- **Gate 1 Sign-Off:** PASSED (2026-11-20)

### Phase 2: Core Implementation
- [x] Module 1: Data access layer and repository interfaces
- [ ] Module 2: Business domain service layer
- [ ] Module 3: Request validation middleware
- **Gate 2 Criteria:** Clean builds, unit tests covering positive and edge cases, zero dead code.

### Phase 3: Verification & Cleanliness Sweep
- [ ] Full test suite execution (100% pass rate required)
- [ ] Linter & static analysis pass (zero errors, zero critical warnings)
- [ ] Zero-Junk Sweep: No scratch files, temp scripts, or `.bak` files remaining
- **Gate 3 Criteria:** Verified green build; clean git working tree.

### Phase 4: State Sync & Delivery
- [ ] State file updated in-place with completion timestamp
- [ ] Concise delta report generated for user review
- **Gate 4 Criteria:** Formal handoff ready for next cycle.

---

## Provenance & Incident Log

| Timestamp | Phase | Event Type | Description | Author / Agent Tier |
| :--- | :---: | :---: | :--- | :--- |
| 2026-11-20 18:15 | Phase 0 | INITIALIZATION | Created state tracker and validated architecture spec | Gemini 3.8 Flash |
| 2026-11-20 18:22 | Phase 1 | COMPLIANCE_PASS| Audited packages (express, zod, prisma); all MIT/Apache-2.0 | Gemini 3.8 Flash |

---

## Rollback & Circuit Breaker Record
<!-- Populate only if a phase is rolled back or blocked -->
- **Incident Date:** N/A
- **Failed Phase:** None
- **Root Cause:** N/A
- **Restoration Checkpoint:** N/A
```

---

## 2. In-Place Delta Update Snippet Example

When updating task completion, the agent MUST perform a targeted in-place edit rather than re-writing the file:

```markdown
<!-- TARGET BLOCK TO REPLACE -->
- [ ] Module 2: Business domain service layer

<!-- REPLACEMENT BLOCK -->
- [x] Module 2: Business domain service layer (Completed 2026-11-20T18:45:00Z)
```

---

## 3. Incident Rollback Entry Template

When rolling back a phase due to errors or violations:

```markdown
### Rollback Event: Phase 2 Aborted
- **Trigger:** License Contamination detected (Third-party library had AGPL-3.0 license).
- **Actions Taken:**
  1. Uninstalled `violating-package` from `package.json`.
  2. Reverted code modifications via `git checkout -- src/services/`.
  3. Purged untracked temporary files via `git clean -f`.
  4. Phase status set to `BLOCKED` pending user selection of an MIT-compatible alternative.
- **Rollback Hash / Green Commit:** `commit-abc1234`
```
