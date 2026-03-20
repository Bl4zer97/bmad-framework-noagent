# 🧳 PORTABLE-SETUP.md — BMAD Framework for Consultants

This guide explains how to use the BMAD framework alongside any client project without polluting the client's repository.

---

## Overview

As a C# consultant, you often work in client codebases you cannot (or should not) modify structurally. BMAD is designed to sit **beside** the client's `.sln` — providing full AI-assisted SDLC support without adding any files the client needs to care about.

---

## Option A: Sibling Directory (Recommended)

Keep BMAD as a sibling to the client's solution folder:

```
client-project/
├── ClientSolution/        ← Client's .NET solution
│   ├── src/
│   ├── tests/
│   └── ClientSolution.sln
└── bmad-framework/        ← BMAD framework (this repo)
    ├── .github/
    ├── .bmad/
    └── agents/
```

**Setup:**
```bash
cd client-project
git clone https://github.com/Bl4zer97/bmad-framework-noagent.git bmad-framework
```

No changes to the client repo at all.

---

## Option B: Subdirectory with .gitignore

Embed BMAD inside the client repo as a hidden folder, then exclude it:

```
client-project/           ← Client's git repo
├── src/
├── tests/
├── ClientSolution.sln
└── .bmad-framework/      ← Add to client's .gitignore
    ├── .github/
    ├── .bmad/
    └── agents/
```

**Setup:**
```bash
cd client-project
git clone https://github.com/Bl4zer97/bmad-framework-noagent.git .bmad-framework
echo ".bmad-framework/" >> .gitignore
```

---

## .gitignore Rules

**Option B — exclude the framework folder:**
```gitignore
# BMAD Framework (do not commit)
.bmad-framework/
```

**If using inline artifacts only:**
```gitignore
# BMAD output files (generated during development)
.bmad/*.md
```

---

## VS Code Workspace Setup

Create a `.code-workspace` file so Copilot agents can reference both the client solution and BMAD agents from one workspace root.

**`client.code-workspace`:**
```json
{
  "folders": [
    { "name": "Client Solution", "path": "../ClientSolution" },
    { "name": "BMAD Framework", "path": "." }
  ],
  "settings": {
    "github.copilot.chat.agent.enabled": true
  }
}
```

Open with:
```bash
code client.code-workspace
```

Copilot Agent Mode will resolve `@bmad-*` mentions against the agent Markdown files in the workspace.

---

## Use Cases

### Use Case 1: Creating a New Solution from Scratch

Start the full enterprise workflow:
```
@bmad-orchestrator start enterprise workflow for [describe the solution]
```
Progress through phases A→G. Generated artifacts land in `.bmad/`. Use `@bmad-developer` to implement the code in the client solution folder.

### Use Case 2: Problem Solving & Debugging

Skip straight to the developer agent:
```
@bmad-developer I have a bug in InvoiceService.cs — NullReferenceException on line 42 when discount is null
```

### Use Case 3: Code Review

```
@bmad-qa review the InvoiceController.cs for test coverage gaps
@bmad-architect review the current solution structure for Clean Architecture violations
```

### Use Case 4: Architecture Design

```
@bmad-architect design a multi-tenant architecture for a SaaS billing platform on Azure
```
The architect produces `ARCHITECTURE.md` in `.bmad/` which feeds subsequent agents.

### Use Case 5: Full SDLC Management

Run the complete enterprise workflow A→G to manage an entire project lifecycle — from requirements through tested, documented code.

---

## Workflow Outputs

Files generated in `.bmad/` during a workflow run:

| File | Phase | Commit? |
|------|-------|---------|
| `PRD.md` | A — PM | Optional (share with client) |
| `BA-REVIEW.md` | B — BA | No (internal working doc) |
| `SPECS.md` | C — BA | Optional (share with client) |
| `ARCHITECTURE.md` | D — Architect | Yes (valuable artifact) |
| `SPRINT-PLAN.md` | E — PM | Optional |
| `DOCUMENTATION.md` | Review | Yes (ship with code) |

**General rule:** Commit artifacts you'd want to hand to the client; keep internal scoring/review docs out of source control.

---

## Tips for Consultants

**Reset for a new project:**
```bash
rm .bmad/*.md
# Then restart: @bmad-orchestrator start enterprise workflow for ...
```

**Customize agent personas for client-specific needs:**
Edit the relevant `agents/enterprise/*.md` file to add client context:
```markdown
## Client Context
- Tech stack: .NET 8, Azure Service Bus, PostgreSQL
- Naming conventions: PascalCase for all public members
- Forbidden libraries: log4net (use Serilog only)
```

**Add client-specific patterns:**
Append a `## Client Patterns` section to `agents/enterprise/developer.md` with code snippets, architecture decisions, or banned approaches specific to that engagement.

**Share workflow state with a client:**
Copy the relevant `.bmad/*.md` files into a `docs/` folder in the client repo and commit them as living documentation.

---

*For full framework documentation, see [README.md](README.md).*
