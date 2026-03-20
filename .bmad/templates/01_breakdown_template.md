# Spec Breakdown

---

## Breakdown Metadata

| Field           | Value                        |
|-----------------|------------------------------|
| Project Name    | `{PROJECT_NAME}`             |
| PRD Reference   | `{PRD_VERSION}` (e.g., PRD v1.0) |
| Date            | `{DATE}`                     |
| Business Analyst| `{BA_NAME}`                  |
| Product Manager | `{PM_NAME}`                  |
| Tech Lead       | `{TECH_LEAD_NAME}`           |
| Status          | `{STATUS}` (Draft / Reviewed / Approved) |

---

## Domain Model

> _High-level domain model derived from requirements. Follows DDD (Domain-Driven Design) terminology._

### Entities

| Entity Name         | Description                                              | Aggregate Root? |
|---------------------|----------------------------------------------------------|-----------------|
| `{Entity1}`         | {DESCRIPTION}                                            | Yes / No        |
| `{Entity2}`         | {DESCRIPTION}                                            | Yes / No        |
| `{Entity3}`         | {DESCRIPTION}                                            | No              |

### Value Objects

| Value Object Name   | Description                                              | Owning Entity   |
|---------------------|----------------------------------------------------------|-----------------|
| `{ValueObject1}`    | {DESCRIPTION} (e.g., Money, Address, Email)              | `{Entity1}`     |
| `{ValueObject2}`    | {DESCRIPTION}                                            | `{Entity2}`     |

### Aggregates

| Aggregate Root      | Contained Entities / VOs                                | Invariant Description                        |
|---------------------|---------------------------------------------------------|----------------------------------------------|
| `{AggregateRoot1}`  | `{Entity}`, `{ValueObject}`                             | {INVARIANT — e.g., "Order total must be > 0"} |
| `{AggregateRoot2}`  | `{Entity}`, `{ValueObject}`                             | {INVARIANT}                                   |

### Domain Events

| Event Name                  | Triggered By                  | Consumed By                    |
|-----------------------------|-------------------------------|--------------------------------|
| `{DomainEvent1}`            | {TRIGGER_ACTION}              | {CONSUMER_SERVICE_OR_HANDLER}  |
| `{DomainEvent2}`            | {TRIGGER_ACTION}              | {CONSUMER_SERVICE_OR_HANDLER}  |

---

## Bounded Contexts

```
┌─────────────────────────────┐    ┌─────────────────────────────┐
│  {BoundedContext1}           │    │  {BoundedContext2}           │
│                             │    │                             │
│  Entities:                  │    │  Entities:                  │
│  - {Entity1}                │◄──►│  - {Entity3}                │
│  - {Entity2}                │    │  - {Entity4}                │
│                             │    │                             │
│  Owner: {TEAM/SERVICE}      │    │  Owner: {TEAM/SERVICE}      │
└─────────────────────────────┘    └─────────────────────────────┘
```

| Bounded Context      | Responsibility                                  | Upstream / Downstream      |
|----------------------|-------------------------------------------------|----------------------------|
| `{Context1}`         | {RESPONSIBILITY}                                | Upstream of `{Context2}`   |
| `{Context2}`         | {RESPONSIBILITY}                                | Downstream of `{Context1}` |
| `{Context3}`         | {RESPONSIBILITY}                                | Standalone                 |

### Context Map Relationships

- **{Context1} → {Context2}**: {RELATIONSHIP_TYPE} (e.g., Customer-Supplier, Conformist, ACL)
- **{Context2} → {Context3}**: {RELATIONSHIP_TYPE}

---

## Feature Breakdown

| Feature ID | Feature Name              | User Story Refs         | Complexity     | Sprint Estimate | Priority |
|------------|---------------------------|-------------------------|----------------|-----------------|----------|
| F-01       | {FEATURE_NAME}            | US-001, US-002          | Low/Med/High   | {N} points      | MUST     |
| F-02       | {FEATURE_NAME}            | US-003, US-004          | Low/Med/High   | {N} points      | MUST     |
| F-03       | {FEATURE_NAME}            | US-005                  | Low/Med/High   | {N} points      | SHOULD   |
| F-04       | {FEATURE_NAME}            | US-006, US-007          | Low/Med/High   | {N} points      | COULD    |
| **Total**  |                           |                         |                | **{N} points**  |          |

### Complexity Definitions

| Level  | Description                                                                 |
|--------|-----------------------------------------------------------------------------|
| Low    | Well-understood, minimal unknowns, single layer change, ≤ 4 story points    |
| Medium | Some unknowns, multi-layer change or new integration, 5–8 story points      |
| High   | Significant unknowns, architectural impact, cross-team dependency, ≥ 13 pts |

---

## Technical Breakdown

> _Maps features to Clean Architecture layers and identifies the technical work required per layer._

### Layer: Domain (`{ProjectName}.Domain`)

| Work Item                         | Description                                              | Feature Ref |
|-----------------------------------|----------------------------------------------------------|-------------|
| Create `{Entity1}` entity          | Core domain entity with business rules                   | F-01        |
| Create `{ValueObject1}` VO         | Immutable value object with validation                   | F-01        |
| Create `{AggregateRoot}` aggregate | Aggregate root with invariants                           | F-01        |
| Define `I{Repository}` interface   | Repository abstraction (no implementation here)          | F-02        |
| Raise `{DomainEvent}` event        | Domain event for {TRIGGER}                               | F-03        |

### Layer: Application (`{ProjectName}.Application`)

| Work Item                              | Description                                              | Feature Ref |
|----------------------------------------|----------------------------------------------------------|-------------|
| Create `{UseCase}Command` / `Query`    | CQRS command or query with MediatR                       | F-01        |
| Create `{UseCase}Handler`              | Handler implementing business orchestration              | F-01        |
| Create `{Dto}` DTO                     | Data transfer object for API boundary                    | F-02        |
| Create `I{Service}` interface          | Application service abstraction                          | F-02        |
| Add FluentValidation for `{Command}`   | Input validation using FluentValidation                  | F-01, F-02  |
| Configure AutoMapper profile           | Mapping between domain entities and DTOs                 | F-02        |

### Layer: Infrastructure (`{ProjectName}.Infrastructure`)

| Work Item                              | Description                                              | Feature Ref |
|----------------------------------------|----------------------------------------------------------|-------------|
| Create `{Entity}Configuration` EF      | EF Core Fluent API configuration / entity configuration  | F-01        |
| Implement `{Repository}` class         | Concrete repository using EF Core DbContext              | F-01        |
| Create EF Core migration               | Database schema migration                                | F-01        |
| Implement `{ExternalService}Client`    | HTTP client for external API integration                 | F-03        |
| Configure Azure Key Vault provider     | Secret injection via IConfiguration                      | NFR         |
| Add Serilog sinks                      | File, console, Azure App Insights sinks                  | NFR         |

### Layer: API / Presentation (`{ProjectName}.Api`)

| Work Item                              | Description                                              | Feature Ref |
|----------------------------------------|----------------------------------------------------------|-------------|
| Create `{Resource}Controller`          | ASP.NET Core controller with route attributes            | F-02        |
| Define request / response models       | API-layer models (separate from Application DTOs)        | F-02        |
| Add Swagger / OpenAPI annotations      | XML docs + Swashbuckle attributes                        | F-02        |
| Configure middleware pipeline          | Auth, error handling, correlation ID middleware          | NFR         |
| Add health check endpoints             | `/health` and `/health/ready` via ASP.NET Health Checks  | NFR         |

### Layer: Tests (`{ProjectName}.Tests`)

| Work Item                              | Description                                              | Feature Ref |
|----------------------------------------|----------------------------------------------------------|-------------|
| Unit tests for Domain entities/VOs     | xUnit tests for business rules and invariants            | F-01        |
| Unit tests for Application handlers    | Handler tests with Moq mocks                             | F-01, F-02  |
| Integration tests for repositories     | EF Core in-memory or test container tests                | F-01        |
| Integration tests for API endpoints    | WebApplicationFactory-based HTTP tests                   | F-02        |
| Acceptance tests                       | SpecFlow / Given-When-Then scenarios                     | F-01–F-04   |

---

## Integration Points

| System / Service         | Protocol     | Auth Method              | Data Direction | Owner          | SLA / Notes                     |
|--------------------------|--------------|--------------------------|----------------|----------------|---------------------------------|
| `{ExternalSystem1}`      | REST / HTTP  | API Key / OAuth 2.0      | Outbound       | {TEAM}         | {SLA / VERSION}                 |
| `{ExternalSystem2}`      | gRPC         | mTLS                     | Bidirectional  | {TEAM}         | {SLA / VERSION}                 |
| Azure Service Bus        | AMQP         | Managed Identity         | Bidirectional  | Platform Team  | Standard tier                   |
| Azure Blob Storage       | REST (SDK)   | Managed Identity         | Outbound       | Platform Team  | LRS / GRS depending on env      |
| Azure AD / Entra ID      | OIDC / OAuth | Client Credentials       | Outbound (auth)| Identity Team  | Production tenant ID: {ID}      |
| SQL Server / PostgreSQL  | TCP/TDS      | SQL Auth / AAD Auth      | Bidirectional  | DBA Team       | {CONNECTION_STRING_REF}         |
| `{InternalService}`      | REST / HTTP  | JWT Bearer               | Inbound        | {TEAM}         | v{VERSION} API contract         |

---

## Data Model Overview

> _High-level overview of database tables / collections. See Architecture Document for full ERD._

| Entity / Table Name   | Key Properties                                          | Relationships                                         | Storage        |
|-----------------------|---------------------------------------------------------|-------------------------------------------------------|----------------|
| `{TableName1}`        | `Id` (PK), `{Property1}`, `{Property2}`, `CreatedAt`   | Has many `{TableName2}`                               | SQL Server     |
| `{TableName2}`        | `Id` (PK), `{ForeignKey}` (FK), `{Property}`           | Belongs to `{TableName1}`                             | SQL Server     |
| `{TableName3}`        | `Id` (PK), `{Property1}`, `{Property2}`, `UpdatedAt`   | Many-to-many with `{TableName1}` via join table        | SQL Server     |

### Soft Delete Strategy

> All entities implementing `ISoftDeletable` will use a `DeletedAt` timestamp column rather than physical deletion.

### Audit Fields

> All entities will inherit from `AuditableEntity` with fields: `CreatedAt`, `CreatedBy`, `UpdatedAt`, `UpdatedBy`.

---

## API Endpoints

> _Preliminary endpoint list. Full specification in Architecture Document and OpenAPI spec._

| Method   | Path                                         | Description                                  | Auth Required | Request Body           | Response             |
|----------|----------------------------------------------|----------------------------------------------|---------------|------------------------|----------------------|
| `GET`    | `/api/v1/{resources}`                        | List all {resources} (paged)                  | Bearer JWT    | –                      | `PagedResult<{Dto}>` |
| `GET`    | `/api/v1/{resources}/{id}`                   | Get single {resource} by ID                   | Bearer JWT    | –                      | `{Dto}`              |
| `POST`   | `/api/v1/{resources}`                        | Create new {resource}                         | Bearer JWT    | `Create{Resource}Req`  | `{Dto}` (201)        |
| `PUT`    | `/api/v1/{resources}/{id}`                   | Full update of {resource}                     | Bearer JWT    | `Update{Resource}Req`  | `{Dto}` (200)        |
| `PATCH`  | `/api/v1/{resources}/{id}`                   | Partial update of {resource}                  | Bearer JWT    | `Patch{Resource}Req`   | `{Dto}` (200)        |
| `DELETE` | `/api/v1/{resources}/{id}`                   | Soft delete {resource}                        | Bearer JWT    | –                      | `204 No Content`     |
| `GET`    | `/health`                                    | Basic health check                            | None          | –                      | `HealthReport`       |
| `GET`    | `/health/ready`                              | Readiness probe (DB, dependencies)            | None          | –                      | `HealthReport`       |

### API Versioning Strategy

- URL-based versioning: `/api/v1/`, `/api/v2/`
- Use `Asp.Versioning.Mvc` NuGet package
- Deprecation headers on older versions

---

## Dependencies & External Services

### NuGet Package Dependencies

| Package                              | Version   | Purpose                                          |
|--------------------------------------|-----------|--------------------------------------------------|
| `MediatR`                            | 12.x      | CQRS mediator pattern                            |
| `FluentValidation.AspNetCore`        | 11.x      | Input validation pipeline                        |
| `AutoMapper.Extensions.Microsoft.DependencyInjection` | 12.x | Object mapping            |
| `Microsoft.EntityFrameworkCore`      | 8.x       | ORM                                              |
| `Microsoft.EntityFrameworkCore.SqlServer` | 8.x  | SQL Server provider                              |
| `Serilog.AspNetCore`                 | 8.x       | Structured logging                               |
| `Swashbuckle.AspNetCore`             | 6.x       | OpenAPI / Swagger                                |
| `xunit`                              | 2.x       | Unit testing framework                           |
| `Moq`                                | 4.x       | Mocking framework for unit tests                 |
| `FluentAssertions`                   | 6.x       | Fluent test assertions                           |
| `{ADDITIONAL_PACKAGE}`               | {VER}     | {PURPOSE}                                        |

### External Service Dependencies

| Service                     | Purpose                              | Env Config Key                       |
|-----------------------------|--------------------------------------|--------------------------------------|
| Azure Key Vault             | Secret management                    | `KeyVault:Uri`                       |
| Azure Application Insights  | Telemetry and monitoring             | `ApplicationInsights:ConnectionString` |
| Azure AD / Entra ID         | Authentication / authorization       | `AzureAd:TenantId`, `AzureAd:ClientId` |
| `{EXTERNAL_SERVICE}`        | {PURPOSE}                            | `{CONFIG_KEY}`                       |

---

## Definition of Ready Checklist

> _All items below must be checked before a user story enters a sprint._

- [ ] User story has a clear narrative (As a / I want / So that)
- [ ] Acceptance criteria are defined and unambiguous
- [ ] ATDD scenarios (Given/When/Then) are written and reviewed
- [ ] Story is estimated in story points by the team
- [ ] All dependencies identified and either resolved or planned
- [ ] UI/UX designs attached (if applicable)
- [ ] API contract defined (request/response models documented)
- [ ] Database schema changes identified and migration plan noted
- [ ] Security requirements for the story noted (auth, authorisation, data sensitivity)
- [ ] Story is small enough to complete within a single sprint
- [ ] Product Owner has approved acceptance criteria

---

## Sprint Estimate Summary

| Category                    | Points / Effort        | Notes                              |
|-----------------------------|------------------------|------------------------------------|
| Domain Layer                | {N} story points       |                                    |
| Application Layer (CQRS)    | {N} story points       |                                    |
| Infrastructure Layer        | {N} story points       |                                    |
| API Layer                   | {N} story points       |                                    |
| Testing                     | {N} story points       | Included in each story estimate     |
| DevOps / CI-CD Setup        | {N} story points       | One-off Sprint 1 investment         |
| **Total MVP Estimate**       | **{N} story points**   |                                    |
| Team Velocity (per sprint)  | {N} story points       |                                    |
| **Estimated Sprints**        | **{N} sprints**        | {N}-week sprints                   |
