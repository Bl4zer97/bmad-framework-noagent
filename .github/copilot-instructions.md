# BMAD Framework — Copilot Workspace Instructions

This workspace contains the **BMAD (Business-Motivated Agile Development)** framework — a structured AI-assisted software delivery system with two workflow modes: **Enterprise** and **Fast Kit**.

Always check the `.bmad/` directory for current project state (phase files, artifacts, gate evaluations) before responding to any workflow-related questions.

Full agent instructions live in:
- `agents/enterprise/` — full-ceremony enterprise agents
- `agents/fast/` — streamlined fast-delivery agents

---

## Available Agents

### Enterprise Mode (structured, gate-driven delivery)

| Agent | File | Purpose |
|---|---|---|
| Orchestrator | `agents/enterprise/orchestrator.md` | Coordinates all phases and quality gates |
| PM (John) | `agents/enterprise/pm.md` | Product vision, PRD, business requirements |
| BA (Sophia) | `agents/enterprise/ba.md` | Acceptance criteria, domain models, specs |
| Architect (Winston) | `agents/enterprise/architect.md` | System design, ADRs, technical direction |
| Developer (Amelia) | `agents/enterprise/developer.md` | Implementation, unit tests, code quality |
| QA (Quinn) | `agents/enterprise/qa.md` | Test plans, integration tests, quality gates |

### Fast Kit Mode (lean, low-ceremony delivery)

| Agent | File | Purpose |
|---|---|---|
| Orchestrator | `agents/fast/orchestrator.md` | Rapid coordination and task routing |
| PM | `agents/fast/pm.md` | Lean product briefs and user stories |
| BA | `agents/fast/ba.md` | Quick acceptance criteria and requirements |
| Architect | `agents/fast/architect.md` | Pragmatic design decisions |
| Developer | `agents/fast/developer.md` | Fast feature implementation |
| QA | `agents/fast/qa.md` | Targeted tests and acceptance validation |

---

## Workflow Modes

**Enterprise Mode** is for complex, regulated, or long-lived projects requiring rigorous phase gates (AI_READY, SPRINT_READY, etc.), full documentation artifacts, and traceability from business need to deployed code.

**Fast Kit Mode** is for smaller projects, prototypes, or teams that want BMAD structure without the full ceremony. It streamlines artifacts and skips non-essential gates to maximize delivery speed.

---

## Key Conventions

- Project state is tracked in `.bmad/` — always read this directory first
- Agent personas have names (John, Sophia, Winston, Amelia, Quinn) in Enterprise mode
- When asked to act as a BMAD agent, load the full instructions from the relevant `agents/` file
- Claude Code slash commands (e.g. `/bmad-orchestrator`) are defined in `.claude/commands/`
