---
name: forge
description: >-
  Atomic, test-driven implementation hybrid skill. Combines Superpowers
  execution, subagent-driven development, parallel dispatch, TDD, clean-code,
  and verification-before-completion. Breaks every big task into at least 2
  levels (Atomic Chunks -> Subatomic Tasks), implements each with tests, and
  consolidates and verifies the whole result. Use when asked to "implement this
  plan", "build this atomically", or invoked via /forge.
---

# Forge — 2-Level Atomic, Subagent & TDD Implementation

Execute a plan or feature request by decomposing it **at least 2 levels deep**, implementing every unit with **Test-Driven Development (TDD)** and **subagents** where independent, and **consolidating + verifying** the complete system.

## Step 1: Load Companion Skill Standards

Before writing code, read the relevant Superpowers and quality skills if not already loaded in this session:
- `~/.gemini/config/plugins/superpowers/skills/subagent-driven-development/SKILL.md`
- `~/.gemini/config/plugins/superpowers/skills/dispatching-parallel-agents/SKILL.md`
- `~/.gemini/config/plugins/superpowers/skills/executing-plans/SKILL.md`
- `~/.gemini/config/plugins/superpowers/skills/test-driven-development/SKILL.md`
- `~/.gemini/config/plugins/superpowers/skills/verification-before-completion/SKILL.md`
- `~/.gemini/config/skills/clean-code/SKILL.md`

If working on an existing repo and the user wants branch isolation, use `using-git-worktrees` first.

## Step 2: Mandatory 2-Level Decomposition (Before Coding)

Do **not** start coding until the task is broken down at least 2 levels deep:

1. **Level 1 — Atomic Chunks:** Divide the overall goal or `/blueprint` plan into cohesive, self-contained chunks (e.g., *Chunk A: Data Model & Validation*, *Chunk B: Core Service Logic*, *Chunk C: CLI/API Layer*). Define each chunk's interface and test boundary.
2. **Level 2 — Subatomic Tasks:** Divide **every** Atomic Chunk into tiny, single-responsibility subatomic tasks (e.g., *A.1: Define types & schema*, *A.2: Write validation test cases*, *A.3: Implement `validateInput()`*).
   - Each subatomic task must touch a minimal surface area and have an explicit test requirement.

Present this 2-level task tree briefly before executing (or confirm it if ambiguity remains).

## Step 3: Execute via Subagents + Strict TDD

For each Atomic Chunk and its Subatomic Tasks:
- **Parallelize independent work:** Use `subagent-driven-development` / `dispatching-parallel-agents` when chunks or subatomic tasks do not edit overlapping files. Keep tightly coupled or shared-file steps sequential via `executing-plans`.
- **TDD on every subatomic task (non-negotiable):**
  1. **Red:** Write or extend the unit/integration test for that subatomic behavior and observe it fail (or confirm what it tests).
  2. **Green:** Implement the minimal, `clean-code`-compliant code to make the test pass.
  3. **Refactor:** Clean up duplication, naming, or nesting while keeping tests green.
- **No untested code:** Every implemented subatomic unit must have corresponding passing tests before moving on.

## Step 4: Consolidate & Integrate

Once all subatomic tasks and atomic chunks are complete:
1. **Wire everything together:** Connect modules, exports, routes, or CLI entry points.
2. **Resolve integration seams:** Check types, error propagation, and shared state across chunks.
3. **Add integration/end-to-end tests** covering the full workflow across chunks.

## Step 5: Final Verification Gate

Follow `verification-before-completion`:
1. Run the **entire** test suite, typechecker, and linter **after the final edit**.
2. Confirm all tests pass with zero failures or skipped assertions.
3. Summarize:
   - What was built (mapped to Atomic Chunks → Subatomic Tasks)
   - Exact test commands run and observed results
   - Next step options (e.g., run `/code-audit` for a final cleanliness check or `finishing-a-development-branch` to merge/PR).
