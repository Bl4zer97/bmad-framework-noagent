# Implementation Log

---

## Log Metadata

| Field              | Value                             |
|--------------------|-----------------------------------|
| Project Name       | `{PROJECT_NAME}`                  |
| Sprint             | `Sprint {N}`                      |
| Sprint Dates       | `{START_DATE}` → `{END_DATE}`     |
| Primary Developer  | `{DEVELOPER_NAME}`                |
| Tech Lead          | `{TECH_LEAD_NAME}`                |
| Architecture Ref   | `{ARCHITECTURE_VERSION}`          |
| Last Updated       | `{DATE}`                          |

---

## Session Log

### Session: {DATE} — {DEVELOPER_NAME}

| Field               | Value                                            |
|---------------------|--------------------------------------------------|
| Date                | `{DATE}`                                         |
| Developer           | `{DEVELOPER_NAME}`                               |
| Sprint              | Sprint {N}                                       |
| Stories Worked On   | US-{XXX}, US-{XXX}                               |
| Stories Completed   | US-{XXX}                                         |
| Duration            | {N} hours                                        |
| AI Agent Used       | {YES / NO — VS Code Copilot Agent Mode}          |

#### Summary of Work

> _Brief narrative of what was implemented in this session._

{SESSION_SUMMARY}

---

## Story Implementation Records

### US-001: {STORY_TITLE}

**Status:** {IN_PROGRESS / COMPLETE}  
**Completed:** {DATE}  
**Developer:** {NAME}  
**PR / Branch:** `feature/US-001-{short-title}` → PR #{PR_NUMBER}  
**Commit(s):** `{COMMIT_SHA_SHORT}` — "{COMMIT_MESSAGE}"

#### Implementation Notes

> _Describe key implementation decisions, approaches taken, and any deviations from the original design._

{IMPLEMENTATION_NOTES}

#### Files Created

| File Path                                                                    | Description                                              |
|------------------------------------------------------------------------------|----------------------------------------------------------|
| `src/{ProjectName}.Domain/Entities/{Entity}.cs`                             | Core domain entity with business rules                   |
| `src/{ProjectName}.Domain/ValueObjects/{ValueObject}.cs`                    | Value object with equality and validation                |
| `src/{ProjectName}.Application/Features/{Feature}/Commands/{Command}.cs`    | CQRS command record                                      |
| `src/{ProjectName}.Application/Features/{Feature}/Commands/{Handler}.cs`    | MediatR command handler                                  |
| `src/{ProjectName}.Application/Features/{Feature}/Commands/{Validator}.cs`  | FluentValidation validator                               |
| `src/{ProjectName}.Application/Features/{Feature}/Queries/{Query}.cs`       | CQRS query record                                        |
| `src/{ProjectName}.Application/Features/{Feature}/Queries/{Handler}.cs`     | MediatR query handler                                    |
| `src/{ProjectName}.Application/Features/{Feature}/DTOs/{Dto}.cs`            | Data transfer object                                     |
| `src/{ProjectName}.Infrastructure/Persistence/Configurations/{Config}.cs`   | EF Core Fluent API entity configuration                  |
| `src/{ProjectName}.Infrastructure/Persistence/Repositories/{Repo}.cs`       | EF Core repository implementation                        |
| `src/{ProjectName}.Infrastructure/Migrations/{Timestamp}_{Name}.cs`         | EF Core database migration                               |
| `src/{ProjectName}.Api/Controllers/{Controller}.cs`                         | ASP.NET Core controller                                  |
| `tests/{ProjectName}.Domain.Tests/Entities/{Entity}Tests.cs`                | Domain entity unit tests                                 |
| `tests/{ProjectName}.Application.Tests/Features/{Feature}/{HandlerTests}.cs`| Handler unit tests with Moq                              |
| `tests/{ProjectName}.Api.Tests/Controllers/{Controller}Tests.cs`            | API integration tests with WebApplicationFactory         |

#### Files Modified

| File Path                                                                    | Change Description                                       |
|------------------------------------------------------------------------------|----------------------------------------------------------|
| `src/{ProjectName}.Infrastructure/Persistence/{ProjectName}DbContext.cs`    | Added `DbSet<{Entity}>` and applied configuration        |
| `src/{ProjectName}.Application/DependencyInjection.cs`                      | Registered validators and AutoMapper profiles            |
| `src/{ProjectName}.Api/Program.cs`                                           | {CHANGE_DESCRIPTION}                                     |
| `{OTHER_FILE}`                                                               | {CHANGE_DESCRIPTION}                                     |

#### Key Decisions Made During Implementation

| Decision                                                  | Rationale                                                  | ADR Raised? |
|-----------------------------------------------------------|------------------------------------------------------------|-------------|
| {DECISION_1 — e.g., "Used record type for Command"}        | {RATIONALE — e.g., "Immutability, less boilerplate"}       | No / ADR-{N}|
| {DECISION_2}                                              | {RATIONALE}                                                | No / ADR-{N}|
| {DECISION_3}                                              | {RATIONALE}                                                | No / ADR-{N}|

---

### US-002: {STORY_TITLE}

**Status:** {IN_PROGRESS / COMPLETE}  
**Completed:** {DATE}  
**Developer:** {NAME}  
**PR / Branch:** `feature/US-002-{short-title}` → PR #{PR_NUMBER}  
**Commit(s):** `{COMMIT_SHA_SHORT}` — "{COMMIT_MESSAGE}"

#### Implementation Notes

{IMPLEMENTATION_NOTES}

#### Files Created

| File Path                  | Description         |
|----------------------------|---------------------|
| `{FILE_PATH}`              | {DESCRIPTION}       |

#### Files Modified

| File Path                  | Change Description  |
|----------------------------|---------------------|
| `{FILE_PATH}`              | {DESCRIPTION}       |

#### Key Decisions Made

| Decision                   | Rationale           | ADR Raised? |
|----------------------------|---------------------|-------------|
| {DECISION}                 | {RATIONALE}         | No          |

---

## Code Patterns Used

> _Record architectural patterns and C# conventions applied during this sprint to guide AI agents and future developers._

### Clean Architecture Patterns

| Pattern                          | Where Applied                               | Example                                             |
|----------------------------------|---------------------------------------------|-----------------------------------------------------|
| CQRS with MediatR                | All use cases                               | `Create{Entity}Command` + `IRequestHandler<,>`      |
| Repository Pattern               | Data access abstraction                     | `I{Entity}Repository` ← `{Entity}Repository`        |
| Specification Pattern            | Complex query predicates                    | `{Entity}ByStatusSpec : Specification<{Entity}>`    |
| Domain Events                    | Side effects after aggregate state changes  | `{Entity}CreatedEvent` dispatched post-save          |
| Outbox Pattern                   | Reliable event publishing                   | {APPLIED / NOT_APPLIED}                             |

### DDD Patterns

| Pattern                | Applied | Notes                                                    |
|------------------------|---------|----------------------------------------------------------|
| Aggregate Root         | ✅      | `{Entity}` is aggregate root controlling child access     |
| Value Objects          | ✅      | `{ValueObject}` — immutable, equality by value           |
| Domain Services        | {✅/❌} | `{DomainService}` for cross-aggregate operations         |
| Domain Events          | ✅      | Raised within aggregate, dispatched by infrastructure    |
| Factory Methods        | ✅      | `{Entity}.Create(...)` static factory for invariant checking |

### C# Conventions Applied

```csharp
// Record types for Commands/Queries (immutable)
public record Create{Entity}Command(string Property1, int Property2) : IRequest<{EntityDto}>;

// Primary constructors (C# 12)
public class {Entity}Repository(ApplicationDbContext context) : I{Entity}Repository
{
    public async Task<{Entity}?> GetByIdAsync(Guid id, CancellationToken ct = default)
        => await context.{Entities}.AsNoTracking().FirstOrDefaultAsync(e => e.Id == id, ct);
}

// Global usings in GlobalUsings.cs
global using {ProjectName}.Domain.Entities;
global using MediatR;

// Nullable reference types
#nullable enable
public string? OptionalProperty { get; private set; }
```

---

## Test Coverage Summary

| Project                              | Lines Covered | Lines Total | Coverage % | Target % | Status   |
|--------------------------------------|---------------|-------------|------------|----------|----------|
| `{ProjectName}.Domain`               | {N}           | {N}         | {N}%       | 90%      | ✅ / ❌  |
| `{ProjectName}.Application`          | {N}           | {N}         | {N}%       | 80%      | ✅ / ❌  |
| `{ProjectName}.Infrastructure`       | {N}           | {N}         | {N}%       | 70%      | ✅ / ❌  |
| `{ProjectName}.Api`                  | {N}           | {N}         | {N}%       | 70%      | ✅ / ❌  |
| **Overall**                          | **{N}**       | **{N}**     | **{N}%**   | **80%**  | ✅ / ❌  |

> Coverage generated by: `dotnet test --collect:"XPlat Code Coverage" && reportgenerator -reports:**/coverage.cobertura.xml -targetdir:coverage-report`

---

## Technical Debt Log

| ID   | Description                                                        | Story Ref | Priority | Created Date | Resolution Plan              |
|------|--------------------------------------------------------------------|-----------|----------|--------------|------------------------------|
| TD-1 | {TECHNICAL_DEBT_ITEM — e.g., "In-memory cache should be Redis in prod"} | US-003 | Medium | {DATE}  | Sprint {N} — tech debt ticket |
| TD-2 | {TECHNICAL_DEBT_ITEM}                                              | US-{N}    | Low      | {DATE}       | {PLAN}                       |
| TD-3 | Missing pagination on `GET /api/v1/{resources}` list endpoint      | US-{N}    | High     | {DATE}       | Address in Sprint {N}        |
| TD-4 | {TECHNICAL_DEBT_ITEM}                                              | {REF}     | {PRIORITY}| {DATE}      | {PLAN}                       |

---

## Blockers & Resolutions

| ID   | Blocker Description                                            | Date Raised | Raised By | Status     | Resolution                                     | Date Resolved |
|------|----------------------------------------------------------------|-------------|-----------|------------|------------------------------------------------|---------------|
| BL-1 | {BLOCKER — e.g., "Azure Service Bus namespace not provisioned"}| {DATE}      | {NAME}    | RESOLVED   | {RESOLUTION — e.g., "Provisioned by DevOps"}  | {DATE}        |
| BL-2 | {BLOCKER}                                                      | {DATE}      | {NAME}    | OPEN       | {CURRENT_STATUS}                               | —             |

---

## PR & Commit References

| Story  | Branch Name                          | PR Number   | PR Status       | Merged Date | Commit SHA (short) |
|--------|--------------------------------------|-------------|-----------------|-------------|--------------------|
| US-001 | `feature/US-001-{short-title}`       | #{N}        | Merged / Open   | {DATE}      | `{SHA}`            |
| US-002 | `feature/US-002-{short-title}`       | #{N}        | Merged / Open   | {DATE}      | `{SHA}`            |
| US-003 | `feature/US-003-{short-title}`       | #{N}        | Open            | —           | —                  |

---

## Sprint Completion Summary

| Metric                         | Target     | Actual     | Notes                                     |
|--------------------------------|------------|------------|-------------------------------------------|
| Story Points Committed         | {N}        | {N}        |                                           |
| Story Points Completed         | {N}        | {N}        |                                           |
| Velocity                       | {N}        | {N}        |                                           |
| Test Coverage (overall)        | ≥ 80%      | {N}%       |                                           |
| Build Success Rate (CI)        | 100%       | {N}%       |                                           |
| Defects Introduced             | 0          | {N}        | {NOTES}                                   |
| Technical Debt Items Created   | Minimise   | {N}        |                                           |
| Sprint Goal Met?               | Yes        | {YES / NO} | {REASON_IF_NO}                            |
