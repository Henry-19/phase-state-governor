# Legal & Compliance Reference Matrix

This reference establishes the legal, regulatory, and software license boundaries that AI agents must enforce at each phase of project execution.

---

## 1. Open-Source License Governance

When an agent suggests or installs external packages, it must categorize the software license according to the project's intellectual property (IP) profile.

### License Classification Matrix

| License Category | Common SPDX Identifiers | Commercial / Proprietary Project | Open-Source (Permissive) | Open-Source (Copyleft) | Key Obligations & Risks |
| :--- | :--- | :---: | :---: | :---: | :--- |
| **Permissive** | `MIT`, `Apache-2.0`, `BSD-2-Clause`, `BSD-3-Clause`, `ISC` | **APPROVED** | **APPROVED** | **APPROVED** | Retain copyright notice and license disclaimer. Zero contamination risk. |
| **Weak Copyleft** | `LGPL-2.1`, `LGPL-3.0`, `MPL-2.0`, `EPL-2.0` | **CONDITIONAL** (Dynamic linking only; no source modification) | **APPROVED** | **APPROVED** | Modifications to the library itself must be shared. Static compilation risks tainting proprietary code. |
| **Strong Copyleft** | `GPL-2.0`, `GPL-3.0` | **BLOCKED** | **BLOCKED** (Unless parent project is GPL) | **APPROVED** | Any distribution containing or linked with this code forces the entire codebase into GPL. |
| **Network Copyleft** | `AGPL-3.0`, `SSPL-1.0` | **STRICTLY BLOCKED** | **BLOCKED** (Unless explicitly approved) | **APPROVED** | Running software as a network service (SaaS) triggers full source code disclosure obligations. |
| **Non-Commercial / Ambiguous** | `CC-BY-NC-4.0`, `JSON License` ("shall be used for Good, not Evil"), `Custom` | **BLOCKED** | **FLAG FOR REVIEW** | **FLAG FOR REVIEW** | Unclear legal definitions create severe commercial liability. |

### Pre-Flight Dependency Rule:
- Before running `npm install <pkg>`, `pip install <pkg>`, `cargo add <pkg>`, or `go get <pkg>`, check its declared license in registry metadata.
- If license is `GPL`, `AGPL`, or missing, halt the phase transition and request explicit user confirmation.

---

## 2. Data Privacy & PII Protection Standards

AI agents must guarantee that generated source code, database migrations, seed data, and unit test suites comply with global privacy frameworks (**GDPR**, **CCPA/CPRA**, and Vietnam **Decree 13/2023/ND-CP**).

### Sensitive Data Classification & Sanitization

| Data Tier | Examples | Permitted in Code / Mocks? | Required Safeguards in Architecture |
| :--- | :--- | :---: | :--- |
| **Tier 1: High-Risk PII** | National ID (SSN, CCCD, Passport), Biometric data, Health records, Credit card numbers | **NEVER** | Strong AES-256 / RSA encryption at rest; tokenization; strict field-level access control. |
| **Tier 2: Direct Identifiers** | Personal email, Phone number, Full legal name, Home address, IP address | **SYNTHETIC ONLY** (e.g., `user@example.com`, `+1-555-0199`) | Pseudonymization; consent tracking; data deletion & export APIs ("Right to be Forgotten"). |
| **Tier 3: Operational Secrets** | API keys, Private certificates, Database credentials, Auth tokens | **NEVER IN CODE** | Loaded strictly via environment variables or cloud secret managers (`.env.example` template only). |
| **Tier 4: Business Data** | Internal order IDs, synthetic analytics metrics | **YES** | Standard database indices, role-based access control (RBAC). |

### Code Generation Privacy Checkpoints:
1. **No Sensitive Data in Logs:** Prohibit logging raw HTTP request bodies, authorization headers, or sensitive query parameters.
2. **Deterministic Pseudonymization:** All test fixtures must use synthetic generator libraries (e.g., Faker with deterministic seeds), never real scraped customer data.

---

## 3. Intellectual Property (IP) Provenance & Clean-Room Rules

1. **Clean-Room Implementation:**
   - The agent must synthesize original implementations based on functional interface requirements.
   - Prohibit verbatim replication of proprietary algorithms or copyrighted code snippets without license attribution.
2. **Provenance Logging:**
   - Every completed phase milestone should maintain an audit record in the task tracker, documenting:
     - Target milestone / component name
     - Date and commit reference
     - Model tier utilized
     - Human-in-the-loop review status
3. **Patent & Trade Secret Insulation:**
   - Never extract or hardcode proprietary trade secrets or unreleased internal IP into client-side code bundles or public repositories.
4. **Sole Author Git Attribution:**
   - Git commits and push histories must reflect strictly one author (the primary committer/developer).
   - Prohibit automated or manual injection of `Co-authored-by:` trailers or multiple author tags to preserve clear, singular legal provenance.
