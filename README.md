# g-code

Personal collection of Antigravity agent skills for planning, writing, reviewing, and refactoring code.

## Included Skills

### Custom Hybrid Skills
- [rules](skills/rules/SKILL.md): Establish, stress-test, and save project Dos, Don'ts, and personal preferences into `BEST_PRACTICES.md` and `AGENTS.md`.
- [blueprint](skills/blueprint/SKILL.md): Research an existing or new project, ask clarifying questions, and produce a step-by-step implementation plan.
- [forge](skills/forge/SKILL.md): Break a plan into atomic chunks and subatomic tasks, implement each with TDD and subagents, and verify the full test suite.
- [code-audit](skills/code-audit/SKILL.md): Run a read-only cleanliness, code-smell, and review report on a file, diff, or repository.
- [polish](skills/polish/SKILL.md): Refactor existing code one test-gated step at a time using Fowler patterns and clean-code rules.

### Third-Party Skills
- [clean-code](skills/clean-code/SKILL.md): Language-agnostic readability, naming, function design, and testing guidelines.
- [code-refactoring-skill](skills/code-refactoring-skill/SKILL.md): Safe, behavior-preserving refactoring catalog and code-smell checks.
- [avoid-ai-writing](skills/avoid-ai-writing/SKILL.md): Audit and edit prose to remove common AI writing patterns.

## Setup on a New Machine

Clone the repository and copy the `skills/` folder into your global Antigravity config directory.

**Windows (PowerShell):**
```powershell
git clone https://github.com/Gamusi/g-code.git
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.gemini\config\skills" | Out-Null
Copy-Item -Path ".\g-code\skills\*" -Destination "$env:USERPROFILE\.gemini\config\skills" -Recurse -Force
```

**macOS / Linux:**
```bash
git clone https://github.com/Gamusi/g-code.git
mkdir -p ~/.gemini/config/skills
cp -r ./g-code/skills/* ~/.gemini/config/skills/
```

Some hybrid skills (`blueprint`, `forge`, `polish`, `code-audit`) also reference the **Superpowers** plugin. On a new machine, install **Superpowers** using either option:

1. **Antigravity Marketplace:** Search for **Superpowers** in the Marketplace and install it.
2. **GitHub repository:** Clone `obra/superpowers` directly into your global plugins folder:

```powershell
# Windows (PowerShell)
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.gemini\config\plugins" | Out-Null
git clone https://github.com/obra/superpowers.git "$env:USERPROFILE\.gemini\config\plugins\superpowers"
```

```bash
# macOS / Linux
mkdir -p ~/.gemini/config/plugins
git clone https://github.com/obra/superpowers.git ~/.gemini/config/plugins/superpowers
```
