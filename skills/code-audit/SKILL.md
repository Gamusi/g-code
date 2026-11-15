---
name: code-audit
description: >-
  Run a comprehensive, read-only code quality, smell, and cleanliness audit on a
  file, diff, or project. Combines Clean Code principles, Fowler code-smell
  detection, and code-review rigor into one structured report with prioritized
  refactoring recommendations. Use when asked to "audit this code", "review how
  clean this is", "check for code smells", or invoked via /code-audit.
---

# Code Audit — Hybrid Clean Code, Smell & Review Report

Perform a **read-only** audit of the target code (a file, folder, git diff, or repository) and produce a clear, prioritized report on **how clean it is** and **how to make it cleaner**.

> **Important:** Do **not** modify any project files during `/code-audit` unless the user explicitly asks you to apply fixes afterward.

## Step 1: Load the Underlying Skill Standards

Before reviewing, read the relevant sections of the installed companion skills if you haven't already in this session:
- **Clean Code:** `~/.gemini/config/skills/clean-code/SKILL.md` (naming, function size, shallow nesting, error handling, boundaries, testability).
- **Code Smells & Refactoring Catalog:** `~/.gemini/config/skills/code-refactoring-skill/SKILL.md` (smell families: Bloaters, OO Abusers, Change Preventers, Dispensables, Couplers, Architectural/Layer violations).
- **Code Review Rigor:** Check correctness, edge cases, security, and test coverage (aligned with `requesting-code-review`).

## Step 2: Inspect the Target Scope

1. Determine what the user wants audited:
   - **Specific file(s) or folder:** Read the target files.
   - **Recent changes / PR:** Run `git status` and `git diff` (or `git diff main...HEAD`) to inspect changed lines and surrounding context.
   - **Whole project:** Map the directory structure first, then inspect entry points, core modules, and largest/most complex files.
2. Check whether automated tests, linters, or typecheckers exist (e.g., `package.json`, `pyproject.toml`, `Cargo.toml`) so you know what safety net is in place before recommending refactors.

## Step 3: Analyze Across 4 Lenses

1. **Correctness, Edge Cases & Security:** Bugs, race conditions, swallowed errors, unvalidated inputs, hardcoded secrets, resource leaks.
2. **Clean Code & Readability:** Vague names, oversized functions, deep nesting, mixed abstraction levels, magic numbers, dead code, missing/misleading comments.
3. **Code Smells & Architecture:** God classes/functions, tight coupling, duplicate logic, feature envy, shotgun surgery, leaky boundaries.
4. **Tests & Maintainability:** Missing tests for critical paths, brittle mocks, hard-to-test side effects.

## Step 4: Output the Audit Report

Present findings concisely using this structure:

### 1. Cleanliness Summary
- **Rating:** `Clean` / `Good (Minor Cleanup)` / `Needs Refactoring` / `High Technical Debt`
- **Top 3 Strengths:** What is already working well.
- **Top 3 Priorities:** The highest-impact improvements.

### 2. Findings (Grouped by Severity)
For each finding, include:
- **Severity:** `Critical` (bug/security) | `High` (major smell/maintainability) | `Medium` (clean-code) | `Low` (polish)
- **Location:** Exact `file_path:line_number`
- **Issue & Smell:** What is wrong and why it matters.
- **How to Fix:** Concrete, minimal recommendation (e.g., *"Extract `validateOrder()` and use early returns"*).

### 3. Suggested Refactoring Plan
A short, ordered list of safe, incremental steps (e.g., *Step 1: Add characterization tests → Step 2: Extract helper → Step 3: Simplify conditionals*).

End by asking if the user wants you to apply any of the recommended steps using the `refactor` (`code-refactoring-skill`) workflow.
