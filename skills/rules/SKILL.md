---
name: rules
description: >-
  Establish, challenge, and persist project rules, best practices, and personal
  coding/communication tastes (Dos and Don'ts) into physical project docs
  (e.g., BEST_PRACTICES.md and AGENTS.md) so agents remember them automatically.
  Includes objective brainstorming and "grill me" checks to prevent impractical
  or conflicting rules. Triggers on /rules, "remember this always", "add a
  rule", "i do not like", "i hate", "i love", or when setting project
  guidelines.
---

# Rules — Objective Rulemaking, Taste Learning & Enforcement

Capture the user's project standards, Dos and Don'ts, and personal preferences into a **persistent, auto-loaded rule document**—after objectively stress-testing the rule so it stays practical.

## Step 1: Load Context & Existing Rules

1. Read `~/.gemini/antigravity/builtin/skills/agy-customizations/docs/rules.md` if you need a refresher on Antigravity rule discovery (`AGENTS.md` / `GEMINI.md`, 24 KB per-file cap).
2. Inspect the current project root for existing rule/guideline files:
   - `BEST_PRACTICES.md`, `AGENTS.md`, `GEMINI.md`, `.agents/rules/`
   - If the user is establishing a **global** personal preference (e.g., *"I hate emojis in responses across all projects"*), also check `~/.gemini/config/AGENTS.md` (or `GEMINI.md`).
3. Read existing rules first so you catch duplicates or contradictions.

## Step 2: Objective Brainstorm & "Grill Me" Check

Do **not** blindly append every preference verbatim. Before writing a rule, evaluate it objectively:
- **Clarity & Testability:** Is it specific enough for an agent to follow without guessing? (e.g., turn *"keep code neat"* into *"Keep functions under ~40 lines and use early returns instead of nested if/else"*.)
- **Practicality & Edge Cases:** Could this rule backfire in common situations? (e.g., *"never write comments"* → suggest *"avoid obvious 'what' comments; still comment non-obvious 'why' decisions"*.)
- **Conflicts:** Does it clash with an existing rule in `BEST_PRACTICES.md` / `AGENTS.md` or with the project's framework/linter?

**When to grill the user:**
- If the rule is vague, overly broad, has risky edge cases, or conflicts with an existing rule, push back respectfully and objectively:
  - Explain **why** it might be impractical or ambiguous.
  - Propose a sharper, scoped version (with exceptions if needed) and ask 1–3 concise questions (or use `ask_question`) to confirm.
- If the preference is already crystal clear and practical (e.g., *"Never use emojis in code, docs, or chat responses"*), acknowledge it concisely and proceed.

## Step 3: Persist & Wire for Automatic Enforcement

1. **Project-level rules:**
   - Create or update `BEST_PRACTICES.md` (or the user's designated guideline file) in the **project root**, organized into clean sections:
     - `## Dos`
     - `## Don'ts`
     - `## Architecture & Code Style`
     - `## Communication & Workflow Tastes`
   - **Ensure Antigravity auto-loads it:** Check `AGENTS.md` (or `GEMINI.md`) at the project root. If `BEST_PRACTICES.md` is separate, ensure `AGENTS.md` exists and includes `@[Best Practices](BEST_PRACTICES.md)` (or keeps a concise summary of the rules) so Antigravity automatically enforces them in every future session without needing to be reminded.
   - Keep each rule file well under Antigravity's **24 KB (24,000 byte)** per-file cap.
2. **Global personal tastes (when explicitly cross-project):**
   - Persist in `~/.gemini/config/AGENTS.md` (creating it if needed) and tell the user it now applies globally.

## Step 4: Confirm What Was Recorded

Show the exact rule(s) added, the file(s) updated, and note that agents will automatically follow them going forward.
