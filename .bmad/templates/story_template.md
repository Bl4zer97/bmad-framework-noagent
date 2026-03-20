# User Story

---

## Story Metadata

| Field            | Value                                                    |
|------------------|----------------------------------------------------------|
| Story ID         | `US-{XXX}`                                               |
| Title            | `{STORY_TITLE}`                                          |
| Epic             | `EP-{XX}` — {EPIC_NAME}                                  |
| Sprint           | `Sprint {N}`                                             |
| Story Points     | `{N}`                                                    |
| Priority         | `MUST / SHOULD / COULD`                                  |
| Status           | `TODO / IN_PROGRESS / IN_REVIEW / DONE`                  |
| Assignee         | `{DEVELOPER_NAME}`                                       |
| Reviewer         | `{REVIEWER_NAME}`                                        |
| Created Date     | `{DATE}`                                                 |
| Completed Date   | `{DATE}`                                                 |
| PR / Branch      | `feature/US-{XXX}-{short-slug}` → PR #{PR_NUMBER}        |

---

## Story Narrative

> _Follows the standard user story format. Be specific about the role, the action, and the measurable value._

**As a** `{ROLE — e.g., registered user, system administrator, billing manager}`,

**I want to** `{ACTION — what the user wants to do, expressed concisely}`,

**So that** `{BENEFIT — the value or outcome, ideally measurable or observable}`.

---

## Acceptance Criteria

> _Each criterion must be testable (not subjective). Use the Given/When/Then format for clarity._
> _Cover: happy path, error/unhappy path, and edge cases._

1. **Given** {PRECONDITION}, **When** {USER_ACTION}, **Then** {EXPECTED_OUTCOME}.

2. **Given** {PRECONDITION}, **When** {USER_ACTION}, **Then** {EXPECTED_OUTCOME}.

3. **Given** {PRECONDITION}, **When** {INVALID_USER_ACTION}, **Then** {ERROR_OUTCOME — e.g., "a 422 response is returned with a validation error on {field}"}.

4. **Given** {EDGE_CONDITION}, **When** {USER_ACTION}, **Then** {EDGE_CASE_OUTCOME}.

5. **Given** the user is not authenticated, **When** {USER_ACTION}, **Then** a `401 Unauthorized` response is returned.

6. **Given** the user does not have the `{REQUIRED_ROLE}` role, **When** {USER_ACTION}, **Then** a `403 Forbidden` response is returned.

---

## ATDD Scenarios

> _Gherkin-format scenarios suitable for SpecFlow / Reqnroll or xUnit BDD-style tests._
> _These must be reviewed and approved by the Product Owner before the story is sprint-ready._

```gherkin
Feature: {FEATURE_NAME}
  {OPTIONAL_FEATURE_DESCRIPTION}

  Background:
    Given I am authenticated as a "{ROLE}" user
    And the system has the following {entities}:
      | {Field1} | {Field2} | {Field3} |
      | {Value1} | {Value2} | {Value3} |

  # ── Happy Path ─────────────────────────────────────────────────────────────

  Scenario: {HAPPY_PATH_SCENARIO_TITLE}
    Given {PRECONDITION_1}
    And {PRECONDITION_2}
    When {ACTION}
    Then {PRIMARY_ASSERTION}
    And {SECONDARY_ASSERTION}

  # ── Unhappy Path ────────────────────────────────────────────────────────────

  Scenario: Reject request when {MISSING_OR_INVALID_CONDITION}
    Given {PRECONDITION}
    When {INVALID_ACTION}
    Then the response status code is 422
    And the response contains a validation error: "{EXPECTED_ERROR_MESSAGE}"

  Scenario: Reject unauthenticated request
    Given I am not authenticated
    When {ACTION}
    Then the response status code is 401

  Scenario: Reject request for insufficient permissions
    Given I am authenticated as a "{LOW_PRIVILEGE_ROLE}" user
    When {ACTION}
    Then the response status code is 403

  # ── Edge Cases ──────────────────────────────────────────────────────────────

  Scenario: Handle {EDGE_CASE_DESCRIPTION}
    Given {EDGE_PRECONDITION}
    When {ACTION}
    Then {EDGE_EXPECTED_RESULT}

  Scenario Outline: Validate {PROPERTY} field boundaries
    Given I am authenticated
    When I submit a request with {PROPERTY} set to "<value>"
    Then the response status code should be <expected_status>

    Examples:
      | value                  | expected_status |
      | {VALID_VALUE}          | 201             |
      | {BOUNDARY_MAX_VALUE}   | 201             |
      | {OVER_BOUNDARY_VALUE}  | 422             |
      | {NULL_VALUE}           | 422             |
      | {EMPTY_STRING}         | 422             |
```

---

## Technical Notes

> _Implementation hints for the developer. Identifies the Clean Architecture components required._

### .NET Implementation Guidance

**Layer: Domain (`{ProjectName}.Domain`)**
- Entity / Aggregate: `{Entity}` in `Entities/{Entity}.cs`
- Value Object (if needed): `{ValueObject}` in `ValueObjects/{ValueObject}.cs`
- Domain Event (if needed): `{Entity}{Action}Event` in `Events/{Entity}{Action}Event.cs`
- Guard clauses: Validate inputs in `{Entity}.Create(...)` factory method; throw `DomainException` on violation

**Layer: Application (`{ProjectName}.Application`)**
- Command / Query: `{Action}{Entity}Command` or `Get{Entity}Query` (use `record` type)
- Handler: `{Action}{Entity}CommandHandler : IRequestHandler<{Command}, {Response}>`
- Validator: `{Action}{Entity}CommandValidator : AbstractValidator<{Command}>` (FluentValidation)
- DTO: `{Entity}Dto` for outbound data transfer
- Mapping: Add to `{Feature}MappingProfile : Profile` AutoMapper profile

**Layer: Infrastructure (`{ProjectName}.Infrastructure`)**
- EF Config: `{Entity}Configuration : IEntityTypeConfiguration<{Entity}>` in `Persistence/Configurations/`
- Repository: `{Entity}Repository : I{Entity}Repository` in `Persistence/Repositories/`
- Migration: Run `dotnet ef migrations add {MigrationName}` if schema changes required

**Layer: API (`{ProjectName}.Api`)**
- Controller: `{Entities}Controller` — add action method to existing controller or create new
- Route: `[HttpPost("/api/v1/{resources}")]`
- Request model: `{Action}{Entity}Request` in `Models/Requests/`
- Response: Return `{EntityDto}` mapped from Application DTO

### Key C# Patterns

```csharp
// Command (Application layer) — use record for immutability
public record {Action}{Entity}Command(
    {Type} {Property1},
    {Type} {Property2}
) : IRequest<{EntityDto}>;

// Handler
public class {Action}{Entity}CommandHandler(
    I{Entity}Repository repository)
    : IRequestHandler<{Action}{Entity}Command, {EntityDto}>
{
    public async Task<{EntityDto}> Handle(
        {Action}{Entity}Command request,
        CancellationToken cancellationToken)
    {
        // 1. Map command to domain entity
        // 2. Apply business logic / validate via domain rules
        // 3. Persist via repository
        // 4. Map to DTO and return
    }
}

// Validator
public class {Action}{Entity}CommandValidator
    : AbstractValidator<{Action}{Entity}Command>
{
    public {Action}{Entity}CommandValidator()
    {
        RuleFor(x => x.{Property1})
            .NotEmpty()
            .MaximumLength({N});

        RuleFor(x => x.{Property2})
            .GreaterThan(0);
    }
}
```

### Database / EF Core Notes

- EF migration required: **{YES / NO}**
- New table(s): `{TABLE_NAMES}`
- New index(es): `{INDEX_DESCRIPTIONS}`
- New FK relationship: `{RELATIONSHIP_DESCRIPTION}`

### Integration / External Dependencies

- External API calls required: **{YES / NO}** — `{DESCRIPTION_IF_YES}`
- Cache invalidation required: **{YES / NO}** — `{CACHE_KEY_IF_YES}`
- Domain event to publish: **{YES / NO}** — `{EVENT_NAME_IF_YES}`
- Azure Service Bus message: **{YES / NO}** — `{TOPIC_OR_QUEUE_IF_YES}`

---

## Tasks

> _Break the story into individual tasks with time estimates._

| Task ID      | Description                                                   | Estimate | Assignee   | Status              |
|--------------|---------------------------------------------------------------|----------|------------|---------------------|
| T-{XXX}.1    | Create/update `{Entity}` domain entity + business rules       | {N}h     | {NAME}     | TODO / IN_PROGRESS / DONE |
| T-{XXX}.2    | Create `{Action}{Entity}Command` record                       | {N}h     | {NAME}     | TODO                |
| T-{XXX}.3    | Implement `{Action}{Entity}CommandHandler`                    | {N}h     | {NAME}     | TODO                |
| T-{XXX}.4    | Add `{Action}{Entity}CommandValidator`                        | {N}h     | {NAME}     | TODO                |
| T-{XXX}.5    | Add AutoMapper mapping in `{Feature}MappingProfile`           | {N}h     | {NAME}     | TODO                |
| T-{XXX}.6    | Add/update `{Entity}Configuration` (EF Core Fluent API)       | {N}h     | {NAME}     | TODO                |
| T-{XXX}.7    | Implement/update `{Entity}Repository`                         | {N}h     | {NAME}     | TODO                |
| T-{XXX}.8    | Create EF Core migration                                      | {N}h     | {NAME}     | TODO                |
| T-{XXX}.9    | Add controller action + request/response models               | {N}h     | {NAME}     | TODO                |
| T-{XXX}.10   | Add Swagger annotations and XML docs                          | {N}h     | {NAME}     | TODO                |
| T-{XXX}.11   | Write unit tests — domain entity (xUnit + FluentAssertions)   | {N}h     | {NAME}     | TODO                |
| T-{XXX}.12   | Write unit tests — handler (xUnit + Moq)                      | {N}h     | {NAME}     | TODO                |
| T-{XXX}.13   | Write unit tests — validator                                  | {N}h     | {NAME}     | TODO                |
| T-{XXX}.14   | Write integration tests — API endpoint (WebApplicationFactory)| {N}h     | {NAME}     | TODO                |
| **Total**    |                                                               | **{N}h** |            |                     |

---

## Definition of Done

> _All items must be checked before this story can be marked DONE._

### Code Quality
- [ ] `dotnet build` passes with 0 errors and 0 warnings
- [ ] Nullable reference types respected throughout
- [ ] No `TODO` comments left in committed code
- [ ] Clean Architecture dependency rule not violated
- [ ] All async I/O uses `async/await` — no `.Result` or `.Wait()`

### Testing
- [ ] Unit tests written for domain entity business rules
- [ ] Unit tests written for application command/query handler
- [ ] Unit tests written for FluentValidation validator
- [ ] Integration tests written for API endpoint(s)
- [ ] All tests pass: `dotnet test`
- [ ] Code coverage for new code ≥ 80%
- [ ] All ATDD acceptance scenarios verified (manual or automated)

### Code Review
- [ ] Pull Request opened against `develop` / `main`
- [ ] PR title references story ID (e.g., `feat: US-{XXX} — {STORY_TITLE}`)
- [ ] At least 1 peer code review approved
- [ ] All review comments resolved

### Deployment
- [ ] Deployed to staging via CD pipeline
- [ ] Smoke test on staging passes
- [ ] Health checks return 200 OK

### Documentation
- [ ] Swagger/OpenAPI annotations complete for new endpoints
- [ ] New config keys documented
- [ ] ADR created if a new architectural decision was made

### Product Acceptance
- [ ] Product Owner has reviewed on staging environment
- [ ] All acceptance criteria verified and signed off by PO
- [ ] Story status updated to DONE in backlog tool

---

## Notes & Dependencies

### Dependencies

| Type              | Description                                                   | Status         |
|-------------------|---------------------------------------------------------------|----------------|
| Story Dependency  | Depends on `US-{YYY}` — `{DEPENDENCY_REASON}`                | {DONE / PENDING}|
| Technical         | Requires `{EXTERNAL_SERVICE}` to be available                 | {RESOLVED / PENDING}|
| Infrastructure    | `{AZURE_RESOURCE}` must be provisioned                        | {RESOLVED / PENDING}|

### Design Decisions Made During Implementation

> _(Fill in during or after implementation — also capture in Implementation Log)_

- {DECISION}: {RATIONALE}
- {DECISION}: {RATIONALE}

### Known Issues / Technical Debt

> _(Record any shortcuts taken due to time constraints — create a tech debt ticket for each)_

- {TECHNICAL_DEBT_ITEM} → Tech Debt ticket: TD-{N}

### Q&A / Open Questions

> _(Record questions raised during refinement or implementation, and their answers)_

| Question                          | Asked By  | Answered By | Answer                  | Date       |
|-----------------------------------|-----------|-------------|-------------------------|------------|
| {QUESTION}                        | {NAME}    | {NAME}      | {ANSWER}                | {DATE}     |
