---
name: blueprint
description: >-
  End-to-end planning and design hybrid skill for new or ongoing projects.
  Combines codebase research, Superpowers brainstorming and writing-plans, and
  interactive "grill me" clarification to turn detailed behavior/design ideas
  into a verified, step-by-step working implementation plan. Use when asked to
  "blueprint this", "plan this feature", "turn this design into a plan", or
  invoked via /blueprint.
---

# Blueprint — Research, Grill-Me & Working Plan

Turn a detailed behavior or design description—whether for a brand-new project or an ongoing codebase—into a **complete, ambiguity-free, actionable implementation plan**.

> **Important:** Stay in planning mode. Do **not** write or edit implementation code until the user reviews and approves the plan.

## Phase 1: Research & Grounding

Before asking the user questions, gather facts from the environment:
1. **Ongoing project:** Search and read the relevant modules, entry points, data models, config files, and existing tests to understand current patterns, constraints, and conventions.
2. **Greenfield project:** Confirm the target language, framework, runtime, and directory layout.
3. Read the companion Superpowers planning skills if not already loaded in this session:
   - `~/.gemini/config/plugins/superpowers/skills/brainstorming/SKILL.md`
   - `~/.gemini/config/plugins/superpowers/skills/writing-plans/SKILL.md`

## Phase 2: "Grill Me" Context Clarification

Compare the user's description against what you found in Phase 1. Identify every gap, ambiguity, or unstated assumption—then **grill the user only on what cannot be answered from the codebase**:
- **Behavior & Edge Cases:** Empty states, invalid inputs, error/retry behavior, concurrency, permissions.
- **Data & Interfaces:** Schema changes, API payloads, backward compatibility, migration needs.
- **Scope & Trade-offs:** Performance vs. simplicity, what is explicitly *out of scope* for v1.

**How to ask:**
- Group questions into a tight, focused round (3–6 high-signal questions, never a wall of 20).
- Use `ask_question` for finite design choices (putting your recommended option first) or concise numbered questions for open-ended details.
- If answers reveal new trade-offs, run one quick follow-up round until the design is locked.

## Phase 3: Design Summary & Alignment

Once context is clear, present a concise architectural snapshot:
- **Goal & Non-Goals:** What this delivers and what it intentionally leaves out.
- **Chosen Approach & Trade-offs:** Why this design fits the codebase (keeping clean boundaries and testability in mind).
- **Affected Files / Modules:** Exact paths to create or modify.

## Phase 4: Actionable Working Plan

Organize the design into an executable plan (aligned with `writing-plans`):
1. **Ordered Implementation Steps:** Small, verifiable tasks in dependency order. Each step must state:
   - **Target files**
   - **Concrete changes** (functions/components/contracts to add or update)
   - **Verification** (exact test or command to prove the step works)
2. **Testing & Verification Plan:** Unit, integration, and manual edge-case checks.
3. **Risks / Rollback Notes:** Anything to watch out for during implementation.

If the plan is substantial, save it as a Markdown artifact (with `RequestFeedback: true` if executable) or to the project's docs folder if the user prefers.

End by asking how the user wants to proceed—for example:
- Adjust any part of the plan
- Isolate work in a branch (`using-git-worktrees`)
- Start executing step-by-step (`executing-plans` or `subagent-driven-development`)
