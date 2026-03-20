# Architecture Decision Record

---

## ADR-{XXXX}: {DECISION_TITLE}

| Field         | Value                                                    |
|---------------|----------------------------------------------------------|
| ID            | ADR-{XXXX}                                               |
| Title         | {DECISION_TITLE}                                         |
| Status        | `{Proposed / Accepted / Deprecated / Superseded}`        |
| Date          | `{DATE}`                                                 |
| Deciders      | `{NAMES_AND_ROLES}`                                      |
| Technical Area| `{Architecture / Data / Security / Infrastructure / API}`|
| Supersedes    | `ADR-{YYYY}` (if applicable, else N/A)                   |
| Superseded by | `ADR-{ZZZZ}` (if applicable, else N/A)                   |

---

## Status

> **{Proposed / Accepted / Deprecated / Superseded}**
>
> - **Proposed** — Under discussion; no decision yet made
> - **Accepted** — Decision made; implementation to follow or in progress
> - **Deprecated** — Previously accepted but no longer recommended; still in use
> - **Superseded** — Replaced by a newer ADR; should no longer be followed

---

## Context

> _Describe the problem or situation that necessitates a decision. Include:_
> _- What is the technical or architectural challenge?_
> _- What are the relevant forces at play (constraints, requirements, team capabilities)?_
> _- What is the current state of the system?_

{CONTEXT_DESCRIPTION}

### Forces / Drivers

- {FORCE_1 — e.g., "The team is working in a .NET ecosystem; consistency with existing patterns is important"}
- {FORCE_2 — e.g., "The system must support horizontal scaling on Azure App Service"}
- {FORCE_3 — e.g., "NFR-P1 requires p95 API response time < 200ms"}
- {FORCE_4}

### Problem Statement

> _One or two sentences summarising the decision that needs to be made._

{PROBLEM_STATEMENT}

---

## Decision

> _Clearly state the decision made. Be direct: "We will use..." or "We have decided to..."._
> _Explain the key reasoning behind the choice._

**We will {DECISION}.**

{DECISION_RATIONALE — 2–4 paragraphs explaining why this was chosen over alternatives}

### Decision Summary

| Aspect                | Chosen Approach                    |
|-----------------------|------------------------------------|
| Technology / Pattern  | `{CHOSEN_TECHNOLOGY_OR_PATTERN}`   |
| NuGet Package(s)      | `{PACKAGE_NAME}` v{VERSION}        |
| .NET Feature Used     | {FEATURE — e.g., "Minimal APIs", "Primary Constructors"} |
| Configuration         | {HOW_IT_IS_CONFIGURED}             |
| Applied To            | {SCOPE — e.g., "All Application layer use cases"} |

---

## Consequences

### Positive Consequences ✅

- {POSITIVE_1 — e.g., "Clean separation of commands and queries; easier to optimise read and write paths independently"}
- {POSITIVE_2 — e.g., "Strong typing eliminates a class of runtime errors"}
- {POSITIVE_3}

### Negative Consequences ❌

- {NEGATIVE_1 — e.g., "Increased number of files and classes for each feature; higher initial setup cost"}
- {NEGATIVE_2 — e.g., "Developers unfamiliar with the pattern require onboarding time"}
- {NEGATIVE_3}

### Neutral Consequences ⚖️

- {NEUTRAL_1 — e.g., "Some duplication between Command/Query DTOs and API request/response models, but these serve different purposes"}
- {NEUTRAL_2}

---

## Alternatives Considered

> _List at least two alternatives that were considered and explain why they were rejected._

### Option 1: {ALTERNATIVE_1_NAME} — REJECTED

**Description:** {DESCRIPTION}

**Pros:**
- {PRO_1}
- {PRO_2}

**Cons:**
- {CON_1 — reason for rejection}
- {CON_2}

**Why rejected:** {REJECTION_RATIONALE}

---

### Option 2: {ALTERNATIVE_2_NAME} — REJECTED

**Description:** {DESCRIPTION}

**Pros:**
- {PRO_1}

**Cons:**
- {CON_1 — reason for rejection}

**Why rejected:** {REJECTION_RATIONALE}

---

### Option 3: {CHOSEN_OPTION_NAME} — ACCEPTED

> _(This is the decision made above. Summarised here for comparison.)_

**Description:** {DESCRIPTION}

**Pros:**
- {PRO_1}
- {PRO_2}

**Cons:**
- {CON_1}

---

## Implementation Notes (.NET Specific)

> _Provide concrete guidance for developers implementing this decision in C#/.NET._

### Project / Layer

Applied in: `{ProjectName}.{LayerName}` project.

### Code Example

```csharp
// {FILE_PATH}
// {BRIEF_DESCRIPTION_OF_WHAT_THIS_DEMONSTRATES}

{CODE_EXAMPLE}
```

### NuGet Packages Required

```xml
<!-- {ProjectName}.{LayerName}.csproj -->
<PackageReference Include="{PACKAGE_NAME}" Version="{VERSION}" />
```

### Registration (Dependency Injection)

```csharp
// {ProjectName}.{LayerName}/DependencyInjection.cs
public static IServiceCollection Add{FeatureName}(
    this IServiceCollection services,
    IConfiguration configuration)
{
    // {REGISTRATION_CODE}
    return services;
}
```

### Configuration (if applicable)

```json
// appsettings.json
{
  "{SectionName}": {
    "{Setting}": "{VALUE}"
  }
}
```

### Migration Steps (if this decision requires schema changes)

1. {STEP_1}
2. {STEP_2}
3. Run `dotnet ef migrations add {MigrationName}`

### Gotchas & Known Issues

- {GOTCHA_1 — e.g., "MediatR pipeline behaviours must be registered in order; validation before logging"}
- {GOTCHA_2}

---

## Related ADRs

| ADR ID     | Title                               | Relationship                                   |
|------------|-------------------------------------|------------------------------------------------|
| ADR-{YYYY} | {TITLE}                             | Prerequisite — must be implemented first       |
| ADR-{ZZZZ} | {TITLE}                             | Related — uses the same infrastructure         |
| ADR-{WWWW} | {TITLE}                             | Supersedes this decision (if applicable)       |

---

## References

- {REFERENCE_1 — e.g., "Martin Fowler — CQRS: https://martinfowler.com/bliki/CQRS.html"}
- {REFERENCE_2 — e.g., "Microsoft Docs — MediatR in ASP.NET Core"}
- {REFERENCE_3}
