# Clean Code & Zero-Junk Workspace Mandate

This guide establishes the technical quality bar and workspace hygiene requirements for AI agents during rapid iterative development ("vibe coding").

---

## 1. The Clean Code Bar for AI Agents

Vibe coding frequently falls victim to "shallow functionality"—code that appears to work on the happy path but creates catastrophic technical debt. AI agents must abide by the following software engineering standards:

### Core Engineering Principles
1. **Single Responsibility Principle (SRP):**
   - Each module, file, or class must own one distinct responsibility.
   - Max file length: **250 lines**. If a file exceeds 300 lines, refactor into cohesive submodules or helper utilities.
   - Max function length: **40 lines**. If logic exceeds this, decompose into smaller pure functions.
2. **KISS & YAGNI:**
   - Implement only what is required to satisfy the active phase spec.
   - Do not invent hypothetical future use-cases, speculative configuration parameters, or elaborate multi-layered abstract wrapper classes.
3. **DRY (Don't Repeat Yourself):**
   - Centralize shared constants, types, and utility functions in designated `/shared` or `/utils` directories.
   - Never duplicate validation or transformation logic across different endpoints or files.
4. **Explicit Typing & Self-Documentation:**
   - Use strict TypeScript, Python type hints, Go structs, or Rust strong typing.
   - Avoid meaningless or ambiguous variable names (`data`, `temp`, `res2`, `flag`).
   - Do not write trivial comments that restate syntax (e.g., `// loop through users`). Only comment on non-obvious business domain constraints or mathematical formulations.

---

## 2. Common Vibe-Coding Anti-Patterns to Eliminate

| Anti-Pattern | Manifestation | Required Agent Behavior |
| :--- | :--- | :--- |
| **Debug Spatter** | Littering code with `console.log("here 1")`, `print("DEBUG:", x)` | Use structured loggers with log levels (`debug`, `info`, `error`). Remove temporary ad-hoc print calls before completing the phase. |
| **Zombie / Dead Code** | Commenting out 50 lines of obsolete code "just in case" | Delete dead code immediately. Version control (Git) retains the historical record. |
| **Hallucinated Package Import** | Importing a package that doesn't exist or isn't listed in dependencies | Verify dependency registration in manifest files (`package.json`, `pyproject.toml`) before writing `import` statements. |
| **Monolithic Try/Catch** | Wrapping an entire 200-line controller in one generic `try/catch (e) { log(e) }` | Catch specific exceptions where they occur; handle failures gracefully; propagate domain errors with clear status codes. |
| **Hardcoded Magic Numbers** | Embedding values like `86400`, `0.15`, `"admin_role"` directly into business logic | Extract into named constants or configuration objects (`SESSION_TTL_SECONDS = 86400`). |

---

## 3. Zero-Junk Workspace Policy

One of the most persistent failure modes of AI agents is cluttering workspaces with unmanaged, temporary, and duplicate files.

### 1. Strict Prohibitions (Zero Tolerance)
- **NO Backup Copies:** Never generate files matching patterns like `*_backup.*`, `*.bak`, `*_v1.*`, `*_v2.*`, `* copy.*`. Git handles versioning.
- **NO Root-Level Scratch Scripts:** Never create files like `test.js`, `check.py`, `dummy.ts` in the workspace root. If an ephemeral test is necessary, place it in an ignored scratch directory or run it in memory, then delete it immediately.
- **NO Redundant Progress Notes:** Never create multiple tracker documents like `PROGRESS.txt`, `agent_summary.md`, `TODO_NOW.md`. Progress belongs strictly in `.agent/state.md`.

### 2. Workspace Sweep & Garbage Collection Procedure
At the end of **every single phase**, the agent MUST perform an automated workspace sweep:

```bash
# 1. Inspect uncommitted or untracked changes
git status --porcelain

# 2. Check for unexpected artifacts:
# - Look for *.tmp, *.log, *.bak, or untracked scratch files
# - Ensure test artifacts and build outputs are in .gitignore
```

If any untracked or non-essential file was created during the phase execution:
- **Delete it immediately** before marking the phase as complete.
- Verify the working directory only contains genuine deliverables (source code, tests, documentation, state update).

---

## 4. In-Place Editing vs. Whole-File Rewriting

AI agents waste tokens and introduce silent regressions when rewriting entire multi-hundred-line files to change a single function.

```text
❌ BAD (Whole File Rewrite):
Agent transmits 400 lines of existing code to change 3 lines in one function.
- High token cost
- High latency
- High risk of losing existing comments or subtle edge-case handling

✅ GOOD (Atomic Targeted In-Place Edit):
Agent uses targeted string replacement or exact diff tools.
- Lowest token cost
- Instant processing
- Zero risk of collateral regression in untouched functions
```

### Golden Rule for Code Modification:
Always locate the exact target block, include sufficient leading/trailing context for uniqueness, and execute a targeted replacement. Never re-emit unchanged file headers, imports, or sibling functions unless a project-wide refactoring is explicitly mandated.

---

## 5. Git Commit & Push Attribution Mandate (Sole Author Enforcement)

When committing and pushing changes across repositories:

- **Sole Author Integrity:** All commits and git history must strictly identify exactly **one author** (the primary developer configured in `git config user.name` and `git config user.email`).
- **Strict Prohibition on Co-Authors:** Never include `Co-authored-by:` git trailers, multiple author trailers, or secondary attribution tags in commit messages or git push operations.
- **Clean Commit Hygiene:** Commits must be focused, atomic, and free of extraneous authorship metadata or AI tool signature trailers unless explicitly requested.
