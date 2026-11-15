---
name: code-audit
description: >-
  Run a comprehensive, read-only code correctness, data-integrity, smell, and
  cleanliness audit on a file, diff, or project. Traces full state lifecycles,
  separates confirmed bugs from plausible risks, and prioritizes functional
  reliability above stylistic polish. Use when asked to "audit this code",
  "review how clean this is", "check for code smells", or invoked via
  /code-audit.
---

# Code Audit — Correctness, Data Integrity, Smell & Cleanliness Report

Perform a **read-only** audit of the target code (a file, folder, git diff, or repository) and produce a prioritized report covering **functional correctness**, **data integrity**, and **code cleanliness**.

> **Important:** Do **not** modify any project files during `/code-audit` unless the user explicitly asks you to apply fixes afterward.

## Step 1: Load the Underlying Skill Standards

Before reviewing, read the relevant sections of the installed companion skills if you haven't already in this session:
- **Clean Code:** `~/.gemini/config/skills/clean-code/SKILL.md` (naming, function size, shallow nesting, error handling, boundaries, testability).
- **Code Smells & Refactoring Catalog:** `~/.gemini/config/skills/code-refactoring-skill/SKILL.md` (smell families: Bloaters, OO Abusers, Change Preventers, Dispensables, Couplers, Architectural/Layer violations).
- **Code Review Rigor:** Check correctness, edge cases, security, and test coverage (aligned with `requesting-code-review`).

## Step 2: Trace the Full Lifecycle (Do Not Audit Slices in Isolation)

1. **Read all state-touching paths** for the target scope—never rate a file after reading only one section:
   - **Specific file(s):** Read initialization, state mutations, storage read/write/clear paths, timers/async handlers, UI rendering, and export/edit flows.
   - **Recent changes / PR:** Run `git status` and `git diff` (or `git diff main...HEAD`), plus inspect callers and downstream consumers of changed functions.
   - **Whole project:** Map the directory structure, then inspect entry points, core modules, state stores, and largest/most complex files.
2. **Trace end-to-end workflows** relevant to the code (e.g., `create → run → pause/overrun → switch → stop → edit → delete → reload → export`).
3. Check whether automated tests, linters, or typecheckers exist so you know what safety net is in place.

## Step 3: Analyze Across 4 Prioritized Lenses

Rank **Correctness & Data Integrity** strictly above **Maintainability & Polish**:

1. **Correctness & Data Integrity (Highest Priority):**
   - **Linked-data invariants:** If an item is edited, reordered, or deleted, do parallel arrays, totals, or historical records (`record`, `events`, `stats`) stay consistent?
   - **Time & async ordering:** Do foreground timers (`setInterval`) and reload recovery (`Date.now()`) agree? Can unawaited writes, clears, or dual-storage fallbacks (`IndexedDB` vs `localStorage`) race or load stale data?
   - **Failure feedback & math edge cases:** Do failed saves/loads notify the user honestly instead of showing false success? Can overruns, negative values, or empty inputs corrupt downstream totals or displays?
   - **Security & leaks:** Unvalidated inputs, XSS (`innerHTML` without escaping), hardcoded secrets, resource leaks.
2. **Tests & Verification Coverage:** Missing tests for critical state transitions, silent catches, hard-to-test side effects.
3. **Code Smells & Architecture:** God classes/functions, tight coupling, duplicate logic, feature envy, shotgun surgery, leaky boundaries.
4. **Clean Code, Accessibility & Polish:** Vague names, oversized functions, deep nesting, stale `aria-label`/keyboard states on dynamic controls, inline styles, cosmetic cleanup.

## Step 4: Output the Audit Report

### Rules for Evidence & Severity
- **Correctness beats style:** Any data-corruption, silent-save-failure, or wrong-calculation bug is at least `High` (or `Critical`) and **must** rank ahead of duplication, caching micro-optimizations, or CSS cleanup. A file with unresolved `High`/`Critical` correctness or data-integrity bugs cannot be rated `Clean` or `Good`.
- **Verify negative claims across the full scope:** Never assert that error handling, validation, or a guard is "missing" unless you have read the entire enclosing function or block through its closing boundary.
- **Confirmed vs. Plausible & Preconditions:** Cite exact `file:line` code for confirmed bugs, and explicitly state the precondition required for any conditional bug to trigger. Label unexecuted concurrency/race conditions as **Plausible Risk (untested at runtime)** rather than claiming they definitely occurred.
- **Calibrate severity by actual impact:** Separate **actual data loss or corruption** from **UI, counter, or telemetry drift**, and separate **project-convention or lint deviations** from **proven runtime or accessibility failures**.
- **Separate intentional trade-offs from accidental complexity:** Check project constraints (`README`, `AGENTS.md`, deployment model) before flagging an architectural pattern as a smell; only flag structure or duplication when it creates concrete divergence or maintenance risk within those constraints.
- **No over-generalized strengths:** Never claim "zero external dependencies", "100% self-contained", or "accessible controls" unless you verified every asset link and dynamic state change.
- **Check fix side effects:** Ensure every proposed fix addresses the root invariant—never suggest a display-only clamp that leaves underlying totals or timelines inconsistent.

### Report Structure

#### 1. Audit Summary
- **Cleanliness & Reliability Rating:** `Clean & Reliable` / `Good (Minor Cleanup)` / `Needs Fixes (Correctness/Integrity Risks)` / `High Technical Debt`
- **Top 3 Verified Strengths:** Specific, evidence-backed positives.
- **Top 3 Priorities:** Ranked strictly by impact (data integrity & correctness first).

#### 2. Findings (Grouped by Severity: `Critical` → `High` → `Medium` → `Low`)
For each finding, include:
- **Severity & Type:** e.g., `High — Data Integrity (Confirmed)` or `Medium — Async Ordering (Plausible Risk)`
- **Location:** Exact `file_path:line_number`
- **Issue & Impact:** What goes wrong, which workflow triggers it, and why it matters.
- **How to Fix:** Concrete, root-cause recommendation that preserves invariants.

#### 3. Suggested Fix & Refactoring Plan
An ordered list starting with **correctness & data-integrity fixes**, followed by **tests**, and finally **structural/clean-code refactors**.

Keep the report purely evaluative—conclude once the findings and plan are presented without prompting to start follow-up edits unless the user asks.
