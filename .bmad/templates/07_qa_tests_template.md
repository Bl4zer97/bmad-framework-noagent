# QA Tests

---

## Document Metadata

| Field              | Value                               |
|--------------------|-------------------------------------|
| Project Name       | `{PROJECT_NAME}`                    |
| Version            | `{VERSION}`                         |
| Date               | `{DATE}`                            |
| QA Lead            | `{QA_LEAD_NAME}`                    |
| Sprint Ref         | `Sprint {N}`                        |
| Architecture Ref   | `{ARCHITECTURE_VERSION}`            |
| Status             | `{DRAFT / ACTIVE / COMPLETE}`       |

---

## Test Strategy Overview

> _Describe the overall testing philosophy, the test pyramid approach, and the tools used._

This project follows the **Test Pyramid** principle:
- **Unit Tests** (base — largest volume): Fast, isolated, no I/O. Test business logic in Domain and Application layers.
- **Integration Tests** (middle): Test interactions between components (EF Core + DB, API endpoints, external service clients).
- **E2E / Acceptance Tests** (top — smallest volume): Test full user journeys through the system, driven by acceptance criteria ATDD scenarios.
- **Performance Tests** (separate suite): Validate NFRs for response time and throughput.

### Testing Tools

| Test Type      | Framework / Tool                                          | Runner                          |
|----------------|-----------------------------------------------------------|---------------------------------|
| Unit           | xUnit + Moq + FluentAssertions                            | `dotnet test`                   |
| Integration    | xUnit + WebApplicationFactory + Testcontainers           | `dotnet test`                   |
| Acceptance     | SpecFlow / Reqnroll (Gherkin) or xUnit Given/When/Then    | `dotnet test`                   |
| Performance    | k6 / Azure Load Testing / BenchmarkDotNet                 | CI/CD pipeline                  |
| Security       | OWASP ZAP / Trivy / `dotnet list package --vulnerable`   | CI/CD pipeline                  |
| Code Coverage  | Coverlet + ReportGenerator                                | `dotnet test --collect`         |
| Static Analysis| Roslyn Analysers + StyleCop + SonarCloud                  | CI/CD pipeline                  |
| Mutation Testing| Stryker.NET (optional)                                   | Manual / scheduled              |

---

## Test Coverage Summary

| Project / Module                     | Unit Tests | Integration Tests | Coverage % | Target % | Status   |
|--------------------------------------|------------|-------------------|------------|----------|----------|
| `{ProjectName}.Domain`               | {N} tests  | —                 | {N}%       | 90%      | ✅ / ❌  |
| `{ProjectName}.Application`          | {N} tests  | —                 | {N}%       | 80%      | ✅ / ❌  |
| `{ProjectName}.Infrastructure`       | {N} tests  | {N} tests         | {N}%       | 70%      | ✅ / ❌  |
| `{ProjectName}.Api`                  | —          | {N} tests         | {N}%       | 70%      | ✅ / ❌  |
| Acceptance Scenarios                 | —          | {N} scenarios     | {N} of {N} | 100%     | ✅ / ❌  |
| **Overall**                          | **{N}**    | **{N}**           | **{N}%**   | **80%**  | ✅ / ❌  |

---

## Unit Test Specifications

> Tests reside in `tests/{ProjectName}.Domain.Tests/` and `tests/{ProjectName}.Application.Tests/`.

### Domain Layer Unit Tests

| Test ID  | Class Under Test              | Method / Behaviour                    | Scenario                                     | Expected Result                              |
|----------|-------------------------------|---------------------------------------|----------------------------------------------|----------------------------------------------|
| UT-D-001 | `{Entity}`                    | `Create` factory method               | Valid inputs                                 | Returns entity; `Id` is not empty            |
| UT-D-002 | `{Entity}`                    | `Create` factory method               | Null or empty required property              | Throws `DomainException` with message        |
| UT-D-003 | `{Entity}`                    | `{BusinessMethod}`                    | Valid state transition                       | State changed; domain event raised           |
| UT-D-004 | `{Entity}`                    | `{BusinessMethod}`                    | Invalid state — guard clause violated        | Throws `InvalidOperationException`           |
| UT-D-005 | `{ValueObject}`               | Equality comparison                   | Two VOs with same values                     | `Equals` returns `true`; hash codes equal    |
| UT-D-006 | `{ValueObject}`               | Equality comparison                   | Two VOs with different values                | `Equals` returns `false`                     |
| UT-D-007 | `{ValueObject}`               | Validation (`IsValid`)                | Invalid value (e.g., negative amount)        | Returns `false` / throws                     |

```csharp
// Example: tests/{ProjectName}.Domain.Tests/Entities/{Entity}Tests.cs
public class {Entity}Tests
{
    [Fact]
    public void Create_WithValidParameters_ShouldReturnEntity()
    {
        // Arrange
        var param1 = "{VALID_VALUE}";

        // Act
        var entity = {Entity}.Create(param1);

        // Assert
        entity.Should().NotBeNull();
        entity.Id.Should().NotBeEmpty();
        entity.{Property}.Should().Be(param1);
    }

    [Theory]
    [InlineData(null)]
    [InlineData("")]
    [InlineData("  ")]
    public void Create_WithInvalidName_ShouldThrowDomainException(string? invalidValue)
    {
        // Act
        var act = () => {Entity}.Create(invalidValue!);

        // Assert
        act.Should().Throw<DomainException>()
           .WithMessage("*{PROPERTY}*");
    }
}
```

### Application Layer Unit Tests

| Test ID  | Handler / Service                      | Scenario                                     | Mock Setup                              | Expected Result                              |
|----------|----------------------------------------|----------------------------------------------|-----------------------------------------|----------------------------------------------|
| UT-A-001 | `Create{Entity}CommandHandler`         | Valid command — entity created successfully  | Repo `AddAsync` called once             | Returns created entity DTO; 0 exceptions     |
| UT-A-002 | `Create{Entity}CommandHandler`         | Duplicate detected                           | Repo `ExistsAsync` returns `true`       | Throws `ConflictException`                   |
| UT-A-003 | `Get{Entity}ByIdQueryHandler`          | Entity found                                 | Repo `GetByIdAsync` returns entity      | Returns populated DTO                        |
| UT-A-004 | `Get{Entity}ByIdQueryHandler`          | Entity not found                             | Repo `GetByIdAsync` returns `null`      | Throws `NotFoundException`                   |
| UT-A-005 | `Create{Entity}CommandValidator`       | All required fields valid                    | —                                       | `IsValid` = true, no errors                  |
| UT-A-006 | `Create{Entity}CommandValidator`       | Required field missing                       | —                                       | `IsValid` = false; error on `{PROPERTY}`     |

```csharp
// Example: tests/{ProjectName}.Application.Tests/Features/{Feature}/Commands/Create{Entity}CommandHandlerTests.cs
public class Create{Entity}CommandHandlerTests
{
    private readonly Mock<I{Entity}Repository> _repositoryMock = new();
    private readonly Create{Entity}CommandHandler _handler;

    public Create{Entity}CommandHandlerTests()
    {
        _handler = new Create{Entity}CommandHandler(_repositoryMock.Object);
    }

    [Fact]
    public async Task Handle_WithValidCommand_ShouldCreateEntityAndReturnDto()
    {
        // Arrange
        var command = new Create{Entity}Command("{VALUE1}", {VALUE2});
        _repositoryMock
            .Setup(r => r.AddAsync(It.IsAny<{Entity}>(), It.IsAny<CancellationToken>()))
            .Returns(Task.CompletedTask);

        // Act
        var result = await _handler.Handle(command, CancellationToken.None);

        // Assert
        result.Should().NotBeNull();
        result.{Property}.Should().Be("{VALUE1}");
        _repositoryMock.Verify(r => r.AddAsync(It.IsAny<{Entity}>(), It.IsAny<CancellationToken>()), Times.Once);
    }
}
```

---

## Integration Test Specifications

> Tests reside in `tests/{ProjectName}.Infrastructure.Tests/` and `tests/{ProjectName}.Api.Tests/`.

### Repository Integration Tests

| Test ID   | Repository                     | Operation              | Scenario                              | Expected Result                             |
|-----------|--------------------------------|------------------------|---------------------------------------|---------------------------------------------|
| IT-R-001  | `{Entity}Repository`           | `AddAsync` + `GetByIdAsync` | Add entity and retrieve by ID     | Retrieved entity matches created entity     |
| IT-R-002  | `{Entity}Repository`           | `GetByIdAsync`         | ID not in database                    | Returns `null`                              |
| IT-R-003  | `{Entity}Repository`           | `GetAllAsync`          | Multiple entities exist               | Returns all entities (correct count)        |
| IT-R-004  | `{Entity}Repository`           | `UpdateAsync`          | Update properties                     | Persisted changes visible on next fetch     |
| IT-R-005  | `{Entity}Repository`           | Soft delete            | Delete entity                         | Entity has `DeletedAt` set; excluded from queries |

```csharp
// Example using Testcontainers for SQL Server
public class {Entity}RepositoryTests : IAsyncLifetime
{
    private readonly MsSqlContainer _sqlContainer = new MsSqlBuilder().Build();
    private ApplicationDbContext _context = null!;
    private {Entity}Repository _repository = null!;

    public async Task InitializeAsync()
    {
        await _sqlContainer.StartAsync();
        var options = new DbContextOptionsBuilder<ApplicationDbContext>()
            .UseSqlServer(_sqlContainer.GetConnectionString())
            .Options;
        _context = new ApplicationDbContext(options);
        await _context.Database.MigrateAsync();
        _repository = new {Entity}Repository(_context);
    }

    public async Task DisposeAsync() => await _sqlContainer.DisposeAsync();

    [Fact]
    public async Task AddAsync_ShouldPersistEntity()
    {
        // Arrange
        var entity = {Entity}.Create("{VALUE}");

        // Act
        await _repository.AddAsync(entity);
        await _context.SaveChangesAsync();
        var result = await _repository.GetByIdAsync(entity.Id);

        // Assert
        result.Should().NotBeNull();
        result!.Id.Should().Be(entity.Id);
    }
}
```

### API Integration Tests

| Test ID   | Endpoint                         | HTTP Method | Scenario                              | Auth         | Expected Status | Expected Body           |
|-----------|----------------------------------|-------------|---------------------------------------|--------------|-----------------|-------------------------|
| IT-API-001| `/api/v1/{resources}`            | POST        | Valid request body                    | Bearer JWT   | 201 Created     | Created entity DTO      |
| IT-API-002| `/api/v1/{resources}`            | POST        | Invalid request body (validation fail)| Bearer JWT   | 422 Unprocessable Entity | ProblemDetails  |
| IT-API-003| `/api/v1/{resources}`            | POST        | No auth token                         | None         | 401 Unauthorized| ProblemDetails          |
| IT-API-004| `/api/v1/{resources}/{id}`       | GET         | Existing resource ID                  | Bearer JWT   | 200 OK          | Entity DTO              |
| IT-API-005| `/api/v1/{resources}/{id}`       | GET         | Non-existent resource ID              | Bearer JWT   | 404 Not Found   | ProblemDetails          |
| IT-API-006| `/api/v1/{resources}/{id}`       | DELETE      | Existing resource ID                  | Bearer JWT   | 204 No Content  | Empty body              |
| IT-API-007| `/health`                        | GET         | System healthy                        | None         | 200 OK          | `{"status":"Healthy"}`  |

---

## Acceptance Test Scenarios

> Scenarios correspond directly to acceptance criteria in user stories. Written in Gherkin format.

### Feature: {FEATURE_1_NAME}

```gherkin
Feature: {FEATURE_1_NAME}
  As a {ROLE}
  I want to {ACTION}
  So that {VALUE}

  Background:
    Given I am authenticated as a "{ROLE}" user
    And the system is in a clean state

  # ── Happy Path ──────────────────────────────────────────────────────

  Scenario: Successfully {ACTION_DESCRIPTION}
    Given {PRECONDITION}
    When I send a POST request to "/api/v1/{resources}" with:
      | Field       | Value       |
      | property1   | {VALUE1}    |
      | property2   | {VALUE2}    |
    Then the response status code should be 201
    And the response body should contain a valid "{resource}" ID
    And the "{resource}" should be retrievable via GET "/api/v1/{resources}/{id}"

  # ── Unhappy Path ─────────────────────────────────────────────────────

  Scenario: Reject {ACTION_DESCRIPTION} with missing required field
    Given {PRECONDITION}
    When I send a POST request to "/api/v1/{resources}" with missing "{REQUIRED_FIELD}"
    Then the response status code should be 422
    And the response body should contain a validation error for "{REQUIRED_FIELD}"

  Scenario: Reject {ACTION_DESCRIPTION} when not authenticated
    Given I am not authenticated
    When I send a POST request to "/api/v1/{resources}"
    Then the response status code should be 401

  # ── Edge Cases ────────────────────────────────────────────────────────

  Scenario: Reject duplicate {resource} creation
    Given a "{resource}" with {UNIQUE_IDENTIFIER} "{VALUE}" already exists
    When I attempt to create another "{resource}" with the same {UNIQUE_IDENTIFIER}
    Then the response status code should be 409
    And the response body should indicate a conflict

  Scenario Outline: Validate {resource} property boundaries
    Given I am authenticated
    When I create a "{resource}" with {PROPERTY} set to "<value>"
    Then the result should be "<outcome>"

    Examples:
      | value            | outcome    |
      | {VALID_VALUE}    | 201        |
      | {BOUNDARY_VALUE} | 201        |
      | {INVALID_VALUE}  | 422        |
      | {NULL_VALUE}     | 422        |
```

---

## Performance Benchmarks

### Response Time Targets

| Endpoint                          | Scenario                      | p50 Target | p95 Target | p99 Target |
|-----------------------------------|-------------------------------|------------|------------|------------|
| `GET /api/v1/{resources}`         | 100 concurrent users           | < 50ms     | < 200ms    | < 500ms    |
| `POST /api/v1/{resources}`        | 50 concurrent users            | < 100ms    | < 300ms    | < 800ms    |
| `GET /api/v1/{resources}/{id}`    | 200 concurrent users           | < 30ms     | < 100ms    | < 300ms    |

### Load Test Script (k6)

```javascript
// tests/performance/load-test.js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '2m', target: 50 },   // ramp up
    { duration: '5m', target: 100 },  // sustain
    { duration: '2m', target: 0 },    // ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<200'],
    http_req_failed: ['rate<0.01'],
  },
};

export default function () {
  const res = http.get(`${__ENV.BASE_URL}/api/v1/{resources}`, {
    headers: { Authorization: `Bearer ${__ENV.JWT_TOKEN}` },
  });
  check(res, { 'status is 200': (r) => r.status === 200 });
  sleep(1);
}
```

### BenchmarkDotNet (micro-benchmarks)

```csharp
// tests/{ProjectName}.Benchmarks/{Benchmark}Benchmarks.cs
[MemoryDiagnoser]
[SimpleJob(RuntimeMoniker.Net80)]
public class {Entity}MappingBenchmarks
{
    private {Entity} _entity = null!;
    private IMapper _mapper = null!;

    [GlobalSetup]
    public void Setup()
    {
        _entity = {Entity}.Create("{VALUE}");
        var config = new MapperConfiguration(cfg => cfg.AddProfile<{MappingProfile}>());
        _mapper = config.CreateMapper();
    }

    [Benchmark]
    public {EntityDto} MapEntityToDto() => _mapper.Map<{EntityDto}>(_entity);
}
```

---

## Security Test Checklist

### OWASP Top 10 Verification

| OWASP Category                          | Test Method                                              | Status     |
|-----------------------------------------|----------------------------------------------------------|------------|
| A01 — Broken Access Control             | Auth integration tests; role-based endpoint tests        | ✅ / ❌ / 🔄 |
| A02 — Cryptographic Failures            | TLS config review; no sensitive data in logs              | ✅ / ❌ / 🔄 |
| A03 — Injection (SQL, LDAP)             | EF Core parameterised queries; no raw SQL interpolation   | ✅ / ❌ / 🔄 |
| A04 — Insecure Design                   | Architecture review; threat modelling                    | ✅ / ❌ / 🔄 |
| A05 — Security Misconfiguration         | `appsettings` review; no dev settings in production       | ✅ / ❌ / 🔄 |
| A06 — Vulnerable Components             | `dotnet list package --vulnerable` in CI                  | ✅ / ❌ / 🔄 |
| A07 — Auth & Session Failures           | JWT validation tests; token expiry tested                 | ✅ / ❌ / 🔄 |
| A08 — Software/Data Integrity           | Signed container images; dependency checksums             | ✅ / ❌ / 🔄 |
| A09 — Logging & Monitoring Failures     | Serilog audit logs; Application Insights alerts           | ✅ / ❌ / 🔄 |
| A10 — Server-Side Request Forgery       | Validated HTTP client calls; no user-controlled URIs      | ✅ / ❌ / 🔄 |

### .NET-Specific Security Checks

- [ ] No hardcoded secrets or connection strings in source code
- [ ] `appsettings.json` contains no secrets (uses Key Vault references)
- [ ] All user inputs validated before reaching domain layer
- [ ] Exception details not exposed in production API responses (`UseDeveloperExceptionPage` only in Development)
- [ ] CORS policy explicitly configured — no wildcard origins
- [ ] `HttpOnly` and `Secure` flags on cookies (if cookies used)
- [ ] Security headers present: `X-Content-Type-Options`, `X-Frame-Options`, `Strict-Transport-Security`

---

## Quality Gate Checklist

> _All items must pass before a release candidate is promoted to production._

### Code Coverage

- [ ] Overall project coverage ≥ **80%** (as reported by Coverlet)
- [ ] Domain layer coverage ≥ **90%**
- [ ] Application layer coverage ≥ **80%**
- [ ] No coverage regression vs previous release

### Test Results

- [ ] All unit tests pass: `dotnet test` — 0 failures
- [ ] All integration tests pass in CI environment
- [ ] All acceptance scenarios pass
- [ ] No skipped tests without documented justification

### Static Analysis

- [ ] `dotnet build` produces 0 warnings (treat warnings as errors in CI)
- [ ] StyleCop / Roslyn analyser violations resolved
- [ ] SonarCloud quality gate passed (if configured): no new critical issues
- [ ] `dotnet list package --vulnerable` — 0 critical/high vulnerabilities

### Code Review

- [ ] All PRs merged with ≥ 1 approved review
- [ ] No outstanding unresolved review comments

### Performance

- [ ] Load test passes: p95 < 200ms at target concurrency
- [ ] No memory leaks detected under sustained load

### Security

- [ ] OWASP security checklist above completed
- [ ] Dependency vulnerability scan clean

### **QUALITY GATE: PASS ✅ / FAIL ❌**

---

## Bug Tracker

| Bug ID  | Description                                              | Severity        | Status         | Assignee    | Story Ref | Raised Date | Resolved Date |
|---------|----------------------------------------------------------|-----------------|----------------|-------------|-----------|-------------|---------------|
| BUG-001 | {BUG_DESCRIPTION}                                        | Critical / High / Medium / Low | Open / Fixed / Closed | {NAME} | US-{N} | {DATE} | {DATE} |
| BUG-002 | {BUG_DESCRIPTION}                                        | {SEVERITY}      | {STATUS}       | {NAME}      | US-{N}    | {DATE}      | {DATE}        |

### Severity Definitions

| Severity | Definition                                                                      |
|----------|---------------------------------------------------------------------------------|
| Critical | System crash, data loss, security breach, or complete feature unavailability    |
| High     | Major feature not working; significant impact on users; no workaround           |
| Medium   | Feature works partially; workaround exists but is inconvenient                  |
| Low      | Minor cosmetic issue or edge case with minimal user impact                      |
