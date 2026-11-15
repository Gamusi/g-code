---
name: polish
description: >-
  Hybrid safe refactoring and code-improvement skill. Combines code-audit smell
  detection, Fowler-style incremental refactoring (code-refactoring-skill),
  clean-code standards, characterization tests (TDD), and strict
  verification-before-completion. Use when asked to "polish this code",
  "refactor and clean up", "improve this module safely", or invoked via /polish.
---

# Polish — Audit, Refactor & Verify

Safely improve existing code without changing external behavior. Combines smell detection, Clean Code standards, incremental Fowler refactors, and test-gated verification.

## Step 1: Load Companion Skill Standards

Read the relevant companion skills if not already loaded in this session:
- `~/.gemini/config/skills/code-audit/SKILL.md` (smell & cleanliness lens)
- `~/.gemini/config/skills/code-refactoring-skill/SKILL.md` (Fowler incremental contract)
- `~/.gemini/config/skills/clean-code/SKILL.md` (naming, function size, nesting, boundaries)
- `~/.gemini/config/plugins/superpowers/skills/test-driven-development/SKILL.md`
- `~/.gemini/config/plugins/superpowers/skills/verification-before-completion/SKILL.md`

## Step 2: Establish the Safety Net (Tests First)

1. Identify the existing test command and run it to confirm a green baseline.
2. **If the target code has no tests:** Write characterization tests locking in its current observable behavior *before* making any structural edits.

## Step 3: Diagnose & Plan Small Moves

1. Scan the target file(s) or module for:
   - **Bloat & Complexity:** Oversized functions/classes, deep nesting, long parameter lists.
   - **Duplication & Coupling:** Repeated logic, feature envy, leaky abstractions.
   - **Clean-Code Friction:** Vague names, swallowed errors, mixed abstraction levels.
2. List the ordered, incremental refactoring moves (e.g., *1. Extract helper -> 2. Replace nested conditionals with guard clauses -> 3. Rename unclear symbols*).

## Step 4: Execute One Move at a Time (Test-Gated)

For **each** refactoring step:
1. Apply **one** focused, behavior-preserving transformation.
2. Run the relevant tests immediately.
3. **If tests fail:** Revert or fix immediately before moving to the next step—never stack broken edits.

## Step 5: Final Verification Gate

Follow `verification-before-completion`:
1. Run the **full** test suite, typechecker, and linter **after the final edit**.
2. Summarize the before/after improvements, exact files changed, and observed test results.
