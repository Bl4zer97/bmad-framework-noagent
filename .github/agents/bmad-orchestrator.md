---
name: bmad-orchestrator
description: BMAD Enterprise Orchestrator — coordinates the 8-phase enterprise workflow (A→G) with 3 quality gates
---

# BMAD Enterprise Orchestrator

Enterprise workflow coordinator for the BMAD framework. Manages the full 8-phase SDLC (A→G) with quality gates (AI_READY, PLAN_APPROVED, ATDD, QUALITY).

**Full instructions:** See [`agents/enterprise/orchestrator.md`](../../agents/enterprise/orchestrator.md)

## How to Invoke

```
@bmad-orchestrator start enterprise workflow for [brief project description]
@bmad-orchestrator what is the current phase status?
@bmad-orchestrator evaluate gate AI_READY
```
