---
name: phase-state-governor
description: >-
  Deterministic phase-driven execution engine for AI coding agents. Enforces structured stage-gate
  progression across document phases, in-place atomic state tracking, clean code with zero file clutter,
  phase-level legal and compliance guardrails, and context/token budget optimization.
---

# Phase-State Governor (SKILL.md)

An operational framework and execution standard designed to guide AI coding agents through phase-oriented workflows. It eliminates common vibe-coding failure modes—such as context drift, out-of-order execution, package hallucination, copyleft license contamination, file clutter, and runaway token consumption.

---

## 1. Core Operating Principles

The Governor operates on six non-negotiable laws:

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                       SIX LAWS OF PHASE GOVERNANCE                         │
├────────────────────────┬──────────────────────────┬─────────────────────────┤
│ 1. Sequential Gating   │ 2. In-Place SSOT Sync    │ 3. Zero-Junk Workspace  │
│ Strict phase-by-phase  │ Single state file,       │ Clean code, no backup   │
│ progression only       │ atomic delta edits       │ or orphan artifacts     │
├────────────────────────┼──────────────────────────┼─────────────────────────┤
│ 4. Legal Pre-Flight    │ 5. Lazy Context Slicing  │ 6. Integrity Hashing    │
│ License, PII & IP gate │ Load only active phase   │ Detect spec drift       │
│ checks at each stage   │ data; cap token budget   │ before code generation  │
└────────────────────────┴──────────────────────────┴─────────────────────────┘
```

---

## 2. Phase-Driven Execution Pipeline (Stage-Gate Model)

Every multi-step or document-driven engineering task MUST progress sequentially through distinct phases. Skipping or parallelizing gated phases is strictly prohibited.

```text
[Input Spec / Plan]
        │
        ▼
┌────────────────────────┐
│  Phase 0: Spec & Hash  │ ──► Verify spec completeness; compute SHA-256 state hash
└────────────────────────┘
        │ (Passed Gate 0)
        ▼
┌────────────────────────┐
│  Phase 1: Legal Pre-Check│ ──► Inspect dependencies, licenses, PII boundaries
└────────────────────────┘
        │ (Passed Gate 1)
        ▼
┌────────────────────────┐
│  Phase 2: Atomic Impl  │ ──► Write clean, minimal code; strict single-responsibility
└────────────────────────┘
        │ (Passed Gate 2)
        ▼
┌────────────────────────┐
│  Phase 3: Verify & Clean│ ──► Execute tests, purge temporary scratch/junk files
└────────────────────────┘
        │ (Passed Gate 3)
        ▼
┌────────────────────────┐
│  Phase 4: In-Place Sync│ ──► Atomically update SSOT state file; report phase delta
└────────────────────────┘
        │
        ▼
[Next Phase OR Completion]
```

### Stage-Gate Criteria Matrix

| Phase | Entry Requirements | Core Activity | Exit Criteria (Gate Pass) |
| :--- | :--- | :--- | :--- |
| **Phase 0: Spec & Validation** | User goal, design doc, or task description. | Parse objectives, identify dependencies, calculate baseline SHA-256 hash of the spec. | Validated task matrix created in designated state file (`.agent/state.md`). |
| **Phase 1: Legal & Compliance** | List of intended packages, APIs, and data models. | Check software licenses against whitelist; review data schemas for PII / privacy regulations. | 100% compliant dependencies (MIT/Apache-2.0/BSD); zero unmasked PII. |
| **Phase 2: Implementation** | Gate 1 signed off; active phase context loaded. | Implement modular, clean, idiomatic code adhering to SRP (Single Responsibility Principle). | Code builds cleanly; unit tests implemented alongside code; no dead code. |
| **Phase 3: Verification & Cleanup** | Implementation completed for the phase slice. | Run test suites, linters; scan workspace for stray scratch/temporary files and delete them. | Tests pass (100%); zero lint errors; zero extraneous files created. |
| **Phase 4: State Sync & Handoff** | Verification passed; clean diff confirmed. | Atomically update check-boxes and phase status in the primary state file; report concise delta. | State file updated in-place; readiness confirmed for subsequent phase. |

---

## 3. In-Place State Tracking (Single Source of Truth)

AI agents frequently degrade project stability by generating duplicate progress files (e.g., `TODO_v2.md`, `progress_temp.txt`, `notes.md`). This skill enforces **Deterministic In-Place Tracking**:

1. **Fixed SSOT Location:**
   - All lifecycle progress is tracked exclusively in `.agent/state.md` (or the project's canonical tracker).
   - If the file does not exist, initialize it in Phase 0 using the standardized format (see `references/phase-pipeline-templates.md`).
2. **Atomic Targeted Editing:**
   - **DO NOT** rewrite the whole state file when toggling a task.
   - Use surgical string replacement or target line updates (e.g., changing `- [ ] Implement user auth schema` to `- [x] Implement user auth schema (Completed 2026-11-20)`).
   - Preserves formatting, prevents token waste, and avoids merge conflicts.
3. **Spec Integrity Hashing:**
   - If the underlying specification document is modified externally, the agent recalculates the SHA-256 hash. If mismatched, the agent stops immediately to re-align before continuing code changes.

---

## 4. Clean Code & Zero-Junk Workspace Rules

To prevent project rot, spaghetti code, and cluttered workspaces:

```text
┌───────────────────────────────────────────────────────────────────────────┐
│                       ZERO-JUNK WORKSPACE MANDATE                         │
├─────────────────────────────────────┬─────────────────────────────────────┤
│ ❌ PROHIBITED HABITS                │ ✅ REQUIRED PRACTICES               │
├─────────────────────────────────────┼─────────────────────────────────────┤
│ • Creating `file_backup.py` or `.bak`│ • Trust Git version control         │
│ • Leaving test runners like `test.js`│ • Put tests in standard `/tests/`   │
│ • Generating empty helper/scratch   │ • Perform in-memory or ephemeral    │
│   files in the workspace root       │   evaluations; purge on exit        │
│ • Bloated 500+ line monolothic files│ • Modular units (< 250 lines/file)  │
│ • Commented-out dead code chunks    │ • Delete obsolete code completely   │
│ • Monolithic speculative abstractions│ • YAGNI (You Aren't Gonna Need It)  │
└─────────────────────────────────────┴─────────────────────────────────────┘
```

- **File Cleanup Protocol:** Before declaring any phase complete, the agent must run an automated workspace check (`git status --porcelain` or folder sweep). Any file not part of the explicit deliverables must be deleted immediately.
- **Code Conciseness:** Write self-documenting code with clear type annotations. Avoid redundant boilerplate comments that simply restate syntax.

---

## 5. Phase-Gated Legal & Compliance Guardrails

Legal checks are integrated directly into phase transitions rather than postponed to production deployment:

### 1. Open-Source License Compatibility
- **Permissive Whitelist:** `MIT`, `Apache-2.0`, `BSD-2-Clause`, `BSD-3-Clause`, `ISC`.
- **Restricted / Copyleft Blocklist (for Commercial/Proprietary Repos):** `GPL-2.0`, `GPL-3.0`, `AGPL-3.0`, `SSPL`, `EUPL`.
- **Verification Rule:** Before introducing any external library (npm, PyPI, Go module, crates.io), check its SPDX identifier. Never install or recommend copyleft dependencies without explicit user sign-off.

### 2. Privacy & Data Protection (GDPR, CCPA, Local Privacy Laws)
- **Zero-PII Storage:** No real names, national IDs (CCCD/SSN), personal email addresses, phone numbers, or credit card numbers in seed data, mock fixtures, or unit tests.
- **Data Minimization:** Ensure APIs only collect fields strictly required for the specific business function.
- **Secrets Sanitization:** Absolute ban on hardcoding API keys, JWT secrets, passwords, or private certificates. Force all secrets into environment variables (`.env.example`).

### 3. Intellectual Property (IP) Provenance & Attribution
- Avoid wholesale copying of distinctive proprietary algorithms or verbatim snippets without mandatory attribution headers.
- Maintain an audit trail in phase sign-offs noting AI-assisted milestones for copyright registrations and corporate governance compliance.

---

## 6. Context Window & Performance Optimization

To ensure maximum speed, minimal token cost, and prevent agent hallucinations:

1. **Lazy Context Slicing:**
   - **Do not read entire documents repeatedly.** Read only the section headers relevant to the active phase.
   - For files exceeding 300 lines, use targeted byte/line offset reads.
2. **Phase Token Budget & Early Stopping:**
   - **Planning & Spec Phase:** $\le 10\%$ of conversation token budget.
   - **Implementation Phase:** $\le 65\%$ of conversation token budget.
   - **Verification & State Sync:** $\le 25\%$ of conversation token budget.
   - **Circuit Breaker:** If an agent attempts the same failed compilation/test fix 3 times consecutively, it MUST trigger an immediate circuit breaker, pause execution, and ask the user for guidance rather than burning tokens in an infinite repair loop.
3. **Differential Delta Reporting:**
   - Responses to the user must be concise delta summaries:
     ```markdown
     ### [Phase 2 Complete: Authentication Middleware]
     - **Status:** PASS (All 6 unit tests green)
     - **Compliance:** Whitelisted dependencies (bcrypt, jsonwebtoken: MIT)
     - **State Sync:** Updated `.agent/state.md` (Task 2.1 marked done)
     - **Next Action:** Advancing to Phase 3: Route Handlers
     ```

---

## 7. Phase Rollback Protocol (Safe Reversal)

When a phase fails validation (e.g., architectural mismatch, license violation, or irrecoverable test breakage):

1. **Clean Workspace Reversion:**
   - Revert modified source files to the last known green commit/checkpoint (`git checkout -- <paths>`).
   - Remove newly created uncommitted files (`git clean -f`).
2. **State Sync Rollback:**
   - Update `.agent/state.md` to reflect status: `BLOCKED` or `ROLLED_BACK`.
   - Log the precise failure reason under the phase's `Incident Log` section.
3. **Zero Orphan Residue:**
   - Verify that no half-installed packages or residual config edits linger in `package.json` or environment files.
