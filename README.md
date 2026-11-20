# Phase-State Governor Skill

A modular, production-grade agentic engineering skill that guides AI coding agents through document-driven phases with mathematical determinism, in-place state synchronization, clean code enforcement, legal/compliance guardrails, and context/token optimization.

---

## Why Phase-State Governor?

"Vibe coding"—developing software via conversational interactions with AI agents—provides unmatched development speed. However, without strict execution protocols, it suffers from severe systemic vulnerabilities:

1. **Out-of-Order Execution & Hallucination:** Agents begin writing complex implementation code before architecture specs are validated or dependencies are vetted.
2. **Context Window Degradation & Amnesia:** As conversations grow long, agents lose track of earlier architectural decisions and contradict previous steps.
3. **State Fragmentation & File Pollution:** Agents create duplicate progress trackers (`TODO_2.md`, `progress_temp.txt`), scratch test scripts (`test.js`, `check.py`), and messy backup files (`app_backup.py`).
4. **Legal & Licensing Liabilities:** Accidental injection of strong copyleft code (GPL/AGPL) into proprietary codebases, or exposure of unmasked PII and hardcoded secrets.
5. **Runaway Token Burn:** Agents repeatedly dump full multi-hundred-line files or trigger infinite repair loops upon encountering minor build errors.

The **Phase-State Governor** transforms raw vibe coding into a disciplined, reliable engineering workflow.

---

## Directory Structure

```
D:\SkillAgent\phase-state-governor\
├── SKILL.md                              # Main agent instruction & phase governance protocol
├── README.md                             # Architecture overview and user guide
└── references\
    ├── legal-compliance-matrix.md        # SPDX license classification, PII safeguards & IP rules
    ├── clean-code-and-zero-junk.md       # Clean code standards & zero-junk workspace mandate
    └── phase-pipeline-templates.md       # Standardized schemas for .agent/state.md and rollback logs
```

---

## Core Capabilities

### 1. Stage-Gate Phase Pipeline
- **Sequential Enforcement:** Strictly transitions from `Phase 0: Spec & Validation` $\to$ `Phase 1: Legal Pre-Check` $\to$ `Phase 2: Atomic Implementation` $\to$ `Phase 3: Verification & Cleanup` $\to$ `Phase 4: State Sync`.
- **Integrity Hashing:** Baseline SHA-256 hash tracking of specification documents to instantly detect spec drift.
- **Circuit Breaker:** Automatically halts execution if an agent fails 3 consecutive repair attempts, avoiding token loops.

### 2. In-Place Deterministic State Tracking
- **Single Source of Truth (SSOT):** Progress is maintained exclusively in `.agent/state.md`.
- **Atomic Targeted Edits:** Strictly updates target task lines in-place (`edit_file`) instead of re-writing the entire state file.
- **Zero-Junk Workspace:** Prohibits backup files, scratch scripts, or unmanaged temporary files. Mandates an automated workspace sweep (`git status --porcelain`) prior to phase completion.
- **Sole Author Git Attribution:** Mandates strictly one author for all git commits and push operations (`git config user.name` / `user.email`). Absolute ban on `Co-authored-by:` trailers or multiple co-authors.

### 3. Integrated Legal & Compliance Guardrails
- **License Whitelist:** Pre-flight gate check requiring permissible licenses (`MIT`, `Apache-2.0`, `BSD`) and blocking copyleft (`GPL`, `AGPL`, `SSPL`) in proprietary environments.
- **Privacy (GDPR / CCPA / Decree 13):** Zero tolerance for real PII in test fixtures or logs; strict requirement for synthetic data generators.
- **Provenance Logging:** Maintains an audit trail of AI model tier and human sign-off for compliance and copyright clearance.

### 4. Context & Token Budget Optimization
- **Lazy Context Slicing:** Loads only the active phase requirements rather than full project history.
- **Token Budget Allocation:** Enforces proportional token consumption (10% Planning, 65% Implementation, 25% Verification).
- **Delta Reporting:** Returns concise, structured status deltas to the user instead of repetitive prose.

---

## Synergies with Workspace Skills

This skill is designed as an autonomous module within your `D:\SkillAgent` ecosystem:
- **`llm-token-optimizer`:** Leverages prompt compression, model routing, and structured output patterns during execution phases to minimize latency and token expenditure.
- **`UI-UX-Design-Engineer`:** Provides the architectural and phased discipline when rolling out component libraries, design systems, and frontend state flows.
- **`Documentation & Architecture Skills` (Upcoming):** Consumes specification artifacts produced by upstream architecture skills as inputs for Phase 0.

---

## Quick Start / How to Use

When invoking your AI agent (Cursor, Claude Code, Antigravity, Cline, Roo-Code, or Gemini CLI), activate the skill by including:

```markdown
Activate the Phase-State Governor skill (`D:\SkillAgent\phase-state-governor\SKILL.md`).
Execute the task strictly phase-by-phase, enforce in-place state tracking in `.agent/state.md`,
maintain zero file junk, and verify license/PII compliance at each gate.
```
