# Sprint Plan

---

## Sprint Plan Metadata

| Field              | Value                             |
|--------------------|-----------------------------------|
| Project Name       | `{PROJECT_NAME}`                  |
| Version            | `{VERSION}`                       |
| Date               | `{DATE}`                          |
| Scrum Master       | `{SM_NAME}`                       |
| Product Owner      | `{PO_NAME}`                       |
| Architecture Ref   | `{ARCHITECTURE_VERSION}`          |
| Status             | `{DRAFT / APPROVED}`              |

---

## Sprint Overview

| Sprint # | Start Date  | End Date    | Sprint Goal                                              | Capacity (pts) | Committed (pts) |
|----------|-------------|-------------|----------------------------------------------------------|----------------|-----------------|
| Sprint 1 | {DATE}      | {DATE}      | {SPRINT_1_GOAL — e.g., "Core domain model + API scaffold"} | {N}          | {N}             |
| Sprint 2 | {DATE}      | {DATE}      | {SPRINT_2_GOAL}                                          | {N}            | {N}             |
| Sprint 3 | {DATE}      | {DATE}      | {SPRINT_3_GOAL}                                          | {N}            | {N}             |
| Sprint N | {DATE}      | {DATE}      | {SPRINT_N_GOAL — e.g., "Hardening, UAT support, go-live prep"} | {N}      | {N}             |

### Team Capacity — Sprint 1

| Developer        | Days Available | Capacity (pts) | Focus Area                           |
|------------------|----------------|----------------|--------------------------------------|
| {DEV_1_NAME}     | {N} days       | {N} pts        | {DOMAIN / APPLICATION / API}         |
| {DEV_2_NAME}     | {N} days       | {N} pts        | {INFRASTRUCTURE / DEVOPS}            |
| {DEV_3_NAME}     | {N} days       | {N} pts        | {TESTING / QA}                       |
| **Total**        | —              | **{N} pts**    |                                      |

> **Velocity assumption:** {N} story points per developer per sprint week, factoring in ceremonies, code review, and meetings.

---

## Sprint 1 — Detailed Plan

**Sprint Goal:** {SPRINT_1_GOAL}

**Dates:** {START_DATE} → {END_DATE}

### Committed User Stories

| Story ID | Title                                    | Points | Assignee      | Priority |
|----------|------------------------------------------|--------|---------------|----------|
| US-001   | {STORY_TITLE}                            | {N}    | {NAME}        | MUST     |
| US-002   | {STORY_TITLE}                            | {N}    | {NAME}        | MUST     |
| US-003   | {STORY_TITLE}                            | {N}    | {NAME}        | SHOULD   |
| **Total** |                                         | **{N}** |              |          |

### Sprint 1 Task Board

#### US-001: {STORY_TITLE}

| Task ID | Task Description                                          | Estimate | Assignee   | Status              |
|---------|-----------------------------------------------------------|----------|------------|---------------------|
| T-001.1 | Scaffold solution structure and `{ProjectName}.sln`       | 2h       | {NAME}     | TODO                |
| T-001.2 | Create `{Entity}` domain entity with business rules       | 3h       | {NAME}     | TODO                |
| T-001.3 | Create `{ValueObject}` value object                       | 1h       | {NAME}     | TODO                |
| T-001.4 | Create `Create{Entity}Command` + `CommandHandler`         | 2h       | {NAME}     | TODO                |
| T-001.5 | Add `Create{Entity}CommandValidator` (FluentValidation)   | 1h       | {NAME}     | TODO                |
| T-001.6 | Implement `{Entity}Repository` with EF Core               | 3h       | {NAME}     | TODO                |
| T-001.7 | Create EF Core migration for `{Entity}` table             | 1h       | {NAME}     | TODO                |
| T-001.8 | Create `{Entity}Controller` with POST endpoint            | 2h       | {NAME}     | TODO                |
| T-001.9 | Write unit tests — domain entity rules                    | 2h       | {NAME}     | TODO                |
| T-001.10| Write unit tests — command handler (Moq)                  | 2h       | {NAME}     | TODO                |
| T-001.11| Write integration tests — API endpoint (WebApplicationFactory) | 3h  | {NAME}     | TODO                |

#### US-002: {STORY_TITLE}

| Task ID | Task Description                                          | Estimate | Assignee   | Status |
|---------|-----------------------------------------------------------|----------|------------|--------|
| T-002.1 | {TASK}                                                    | {N}h     | {NAME}     | TODO   |
| T-002.2 | {TASK}                                                    | {N}h     | {NAME}     | TODO   |
| T-002.3 | Write unit tests                                          | {N}h     | {NAME}     | TODO   |
| T-002.4 | Write integration tests                                   | {N}h     | {NAME}     | TODO   |

#### Sprint 1 Infrastructure / DevOps Tasks

| Task ID | Task Description                                          | Estimate | Assignee   | Status |
|---------|-----------------------------------------------------------|----------|------------|--------|
| DEV-001 | Set up GitHub Actions / Azure DevOps CI pipeline          | 4h       | {NAME}     | TODO   |
| DEV-002 | Configure `dotnet build` + `dotnet test` in pipeline      | 2h       | {NAME}     | TODO   |
| DEV-003 | Set up Docker build and push to Azure Container Registry  | 3h       | {NAME}     | TODO   |
| DEV-004 | Configure Azure App Service staging environment           | 2h       | {NAME}     | TODO   |
| DEV-005 | Set up Azure Key Vault and link to App Service            | 2h       | {NAME}     | TODO   |
| DEV-006 | Configure Application Insights connection                 | 1h       | {NAME}     | TODO   |
| DEV-007 | Set up code coverage reporting (Coverlet + reportgenerator)| 1h      | {NAME}     | TODO   |

---

## Sprint 2 — Summary Plan

**Sprint Goal:** {SPRINT_2_GOAL}

**Dates:** {START_DATE} → {END_DATE}

| Story ID | Title                                    | Points | Assignee | Priority |
|----------|------------------------------------------|--------|----------|----------|
| US-004   | {STORY_TITLE}                            | {N}    | {NAME}   | MUST     |
| US-005   | {STORY_TITLE}                            | {N}    | {NAME}   | MUST     |
| US-006   | {STORY_TITLE}                            | {N}    | {NAME}   | SHOULD   |
| **Total** |                                         | **{N}** |         |          |

> Detailed task breakdown to be produced in sprint planning meeting at start of Sprint 2.

---

## Sprint N — Summary Plan

**Sprint Goal:** {SPRINT_N_GOAL}

**Dates:** {START_DATE} → {END_DATE}

| Story ID | Title                                    | Points | Assignee | Priority |
|----------|------------------------------------------|--------|----------|----------|
| US-{N}   | {STORY_TITLE}                            | {N}    | {NAME}   | {PRIORITY} |
| Hardening| Performance testing + bug fixes          | {N}    | Team     | MUST     |
| UAT      | UAT support, defect resolution           | {N}    | Team     | MUST     |

---

## Definition of Done (DoD)

> _A story is only DONE when ALL of the following are checked:_

### Code Quality

- [ ] Code compiles with `dotnet build` — zero errors, zero warnings
- [ ] Nullable reference types respected — no `#nullable disable` suppressions without justification
- [ ] No `TODO` comments left in committed code (tracked as technical debt tickets instead)
- [ ] Code follows Clean Architecture — no cross-layer dependency violations
- [ ] Async/await used for all I/O operations — no blocking `.Result` or `.Wait()`
- [ ] `IDisposable` / `IAsyncDisposable` properly implemented where applicable

### Testing

- [ ] Unit tests written for all domain logic and application handlers
- [ ] Unit test coverage for new code ≥ 80% (measured by Coverlet)
- [ ] All unit tests pass: `dotnet test --no-build`
- [ ] Integration tests written for API endpoints and repository layer
- [ ] All integration tests pass in CI pipeline
- [ ] Acceptance criteria verified against ATDD scenarios
- [ ] No skipped (`[Fact(Skip=...)]`) or commented-out tests in commit

### Code Review

- [ ] Pull Request created from feature branch to `main` / `develop`
- [ ] PR description references story ID (e.g., `Closes #US-001`)
- [ ] At least 1 peer code review approved
- [ ] All code review comments resolved or explicitly marked as won't-fix with reason
- [ ] No merge conflicts

### Deployment

- [ ] Feature branch CI pipeline passes (build + test)
- [ ] Deployed to staging environment via CD pipeline
- [ ] Smoke test on staging environment passes
- [ ] Health check endpoints return 200 OK on staging
- [ ] No breaking changes to existing API contracts (or version bump applied)

### Documentation

- [ ] OpenAPI / Swagger annotations complete for new endpoints
- [ ] Any new configuration keys documented in `README` or config guide
- [ ] New EF Core migrations are reversible and tested
- [ ] ADR created for any new significant architectural decision

### Product Owner Sign-off

- [ ] PO has reviewed and accepted all acceptance criteria on staging
- [ ] Story status updated to DONE in backlog tool

---

## Sprint Ceremonies Schedule

| Ceremony                | Frequency     | Duration  | Participants                                    |
|-------------------------|---------------|-----------|-------------------------------------------------|
| Sprint Planning         | Start of sprint | 2–4 hours | Full team + PO                                 |
| Daily Stand-up          | Daily         | 15 min    | Dev team + SM                                   |
| Sprint Review           | End of sprint | 1–2 hours | Full team + PO + Stakeholders                   |
| Sprint Retrospective    | End of sprint | 1 hour    | Dev team + SM                                   |
| Backlog Refinement      | Mid-sprint    | 1 hour    | Dev team + PO + BA                              |
| Architecture Review     | As needed     | 1 hour    | Architect + Tech Lead + Dev team                |

---

## Sprint Risks & Impediments

| ID   | Risk / Impediment                                    | Sprint    | Probability | Impact | Mitigation / Action                                         |
|------|------------------------------------------------------|-----------|-------------|--------|-------------------------------------------------------------|
| SR-1 | Azure environment not ready for Sprint 1             | Sprint 1  | Medium      | High   | Escalate to Platform team; use Docker locally as fallback   |
| SR-2 | Unfamiliar with {EXTERNAL_SYSTEM} API               | Sprint 2  | Low         | Medium | Spike story in Sprint 1 to validate integration approach    |
| SR-3 | Team member absence reducing capacity               | Any       | Medium      | Medium | Build 10% buffer into sprint commitment                     |
| SR-4 | EF Core migration complexity for {ENTITY}           | Sprint 1  | Low         | High   | Architect to review migration plan before implementation    |
| SR-5 | {CUSTOM_RISK}                                        | {SPRINT}  | {PROB}      | {IMP}  | {MITIGATION}                                                |

---

## ATDD Gate Checklist

> _Must be verified before each sprint's stories are considered sprint-planning-ready._

- [ ] All stories have ATDD Given/When/Then scenarios written (in story template or separate `.feature` file)
- [ ] ATDD scenarios reviewed and approved by PO
- [ ] Scenarios cover: happy path, unhappy path, and at least one edge case per story
- [ ] Scenarios are automatable (no subjective or manual-only assertions)
- [ ] Scenarios reviewed by QA lead for testability
- [ ] Acceptance tests can be linked to the CI/CD pipeline (SpecFlow / Reqnroll / manual xUnit)
- [ ] **ATDD Gate: APPROVED ✅ / BLOCKED ❌**

**Approved by:** {NAME} | **Date:** {DATE}
