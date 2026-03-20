# Epics & User Stories

---

## Project Reference

| Field            | Value                          |
|------------------|--------------------------------|
| Project Name     | `{PROJECT_NAME}`               |
| PRD Reference    | `{PRD_VERSION}`                |
| Breakdown Ref    | `{BREAKDOWN_VERSION}`          |
| Date             | `{DATE}`                       |
| Product Owner    | `{PO_NAME}`                    |
| Scrum Master     | `{SM_NAME}`                    |
| Status           | `{DRAFT / REFINED / APPROVED}` |

---

## Epic Summary Table

| Epic ID | Epic Name                    | Description                                                   | Priority | Story Points | Status       |
|---------|------------------------------|---------------------------------------------------------------|----------|--------------|--------------|
| EP-01   | {EPIC_1_NAME}                | {SHORT_DESCRIPTION}                                           | MUST     | {N} pts      | {STATUS}     |
| EP-02   | {EPIC_2_NAME}                | {SHORT_DESCRIPTION}                                           | MUST     | {N} pts      | {STATUS}     |
| EP-03   | {EPIC_3_NAME}                | {SHORT_DESCRIPTION}                                           | SHOULD   | {N} pts      | {STATUS}     |
| EP-04   | {EPIC_4_NAME}                | {SHORT_DESCRIPTION}                                           | COULD    | {N} pts      | {STATUS}     |
| **Total** |                            |                                                               |          | **{N} pts**  |              |

---

## EP-01: {EPIC_1_NAME}

**Description:** {FULL_EPIC_DESCRIPTION}

**Business Value:** {BUSINESS_VALUE}

**Definition of Done for Epic:**
- [ ] All child stories completed and accepted
- [ ] Integration tests passing
- [ ] PO sign-off on all acceptance criteria
- [ ] Deployed and smoke-tested in staging environment

---

### US-001: {USER_STORY_1_TITLE}

| Field           | Value                                       |
|-----------------|---------------------------------------------|
| Story ID        | US-001                                      |
| Epic            | EP-01                                       |
| Sprint          | Sprint {N}                                  |
| Story Points    | {N}                                         |
| Priority        | MUST / SHOULD / COULD                       |
| Status          | {TODO / IN_PROGRESS / DONE}                 |
| Assignee        | {NAME}                                      |

**Story Narrative:**
> As a **{ROLE}**,
> I want to **{ACTION/FEATURE}**,
> So that **{BENEFIT/VALUE}**.

**Acceptance Criteria:**

1. **Given** {CONTEXT}, **When** {ACTION}, **Then** {EXPECTED_OUTCOME}.
2. **Given** {CONTEXT}, **When** {ACTION}, **Then** {EXPECTED_OUTCOME}.
3. **Given** {CONTEXT}, **When** {ACTION}, **Then** {EXPECTED_OUTCOME — error/edge case}.

**ATDD Scenarios:**

```gherkin
Feature: {FEATURE_NAME}

  Scenario: {HAPPY_PATH_SCENARIO_TITLE}
    Given {PRECONDITION}
    When {ACTION}
    Then {EXPECTED_RESULT}
    And {ADDITIONAL_ASSERTION}

  Scenario: {UNHAPPY_PATH_SCENARIO_TITLE}
    Given {PRECONDITION}
    When {INVALID_ACTION}
    Then {ERROR_RESULT}

  Scenario: {EDGE_CASE_SCENARIO_TITLE}
    Given {EDGE_CONDITION}
    When {ACTION}
    Then {EDGE_CASE_RESULT}
```

**Technical Notes (.NET):**
- Handler: `{UseCase}CommandHandler` in `{ProjectName}.Application`
- Validator: `{Command}Validator` using FluentValidation
- Repository: `I{Entity}Repository` → `{Entity}Repository`
- EF migration required: {YES / NO}
- New API endpoint: `{METHOD} /api/v1/{resource}`

**Tasks:**

| Task ID | Description                                       | Estimate | Status              |
|---------|---------------------------------------------------|----------|---------------------|
| T-001.1 | Create `{Entity}` domain entity                   | {N}h     | {TODO / DONE}       |
| T-001.2 | Create `{Command}` and `{Handler}`                | {N}h     | {TODO / DONE}       |
| T-001.3 | Add FluentValidation for `{Command}`              | {N}h     | {TODO / DONE}       |
| T-001.4 | Implement `{Repository}` with EF Core             | {N}h     | {TODO / DONE}       |
| T-001.5 | Create `{Controller}` endpoint                    | {N}h     | {TODO / DONE}       |
| T-001.6 | Write unit tests (xUnit + Moq)                    | {N}h     | {TODO / DONE}       |
| T-001.7 | Write integration tests                           | {N}h     | {TODO / DONE}       |

**Dependencies:** {US-XXX / None}

---

### US-002: {USER_STORY_2_TITLE}

| Field           | Value                                       |
|-----------------|---------------------------------------------|
| Story ID        | US-002                                      |
| Epic            | EP-01                                       |
| Sprint          | Sprint {N}                                  |
| Story Points    | {N}                                         |
| Priority        | MUST / SHOULD / COULD                       |
| Status          | {TODO / IN_PROGRESS / DONE}                 |
| Assignee        | {NAME}                                      |

**Story Narrative:**
> As a **{ROLE}**,
> I want to **{ACTION/FEATURE}**,
> So that **{BENEFIT/VALUE}**.

**Acceptance Criteria:**

1. **Given** {CONTEXT}, **When** {ACTION}, **Then** {EXPECTED_OUTCOME}.
2. **Given** {CONTEXT}, **When** {ACTION}, **Then** {EXPECTED_OUTCOME}.

**ATDD Scenarios:**

```gherkin
  Scenario: {SCENARIO_TITLE}
    Given {PRECONDITION}
    When {ACTION}
    Then {EXPECTED_RESULT}
```

**Technical Notes (.NET):**
- {IMPLEMENTATION_NOTE}

**Tasks:**

| Task ID | Description                                       | Estimate | Status        |
|---------|---------------------------------------------------|----------|---------------|
| T-002.1 | {TASK_DESCRIPTION}                                | {N}h     | {STATUS}      |
| T-002.2 | {TASK_DESCRIPTION}                                | {N}h     | {STATUS}      |

**Dependencies:** US-001

---

## EP-02: {EPIC_2_NAME}

**Description:** {FULL_EPIC_DESCRIPTION}

**Business Value:** {BUSINESS_VALUE}

---

### US-003: {USER_STORY_3_TITLE}

| Field           | Value                                       |
|-----------------|---------------------------------------------|
| Story ID        | US-003                                      |
| Epic            | EP-02                                       |
| Sprint          | Sprint {N}                                  |
| Story Points    | {N}                                         |
| Priority        | MUST / SHOULD / COULD                       |
| Status          | {TODO / IN_PROGRESS / DONE}                 |

**Story Narrative:**
> As a **{ROLE}**,
> I want to **{ACTION/FEATURE}**,
> So that **{BENEFIT/VALUE}**.

**Acceptance Criteria:**

1. {CRITERION}
2. {CRITERION}

**ATDD Scenarios:**

```gherkin
  Scenario: {SCENARIO_TITLE}
    Given {PRECONDITION}
    When {ACTION}
    Then {EXPECTED_RESULT}
```

**Technical Notes (.NET):**
- {IMPLEMENTATION_NOTE}

**Tasks:**

| Task ID | Description                   | Estimate | Status   |
|---------|-------------------------------|----------|----------|
| T-003.1 | {TASK}                        | {N}h     | {STATUS} |
| T-003.2 | {TASK}                        | {N}h     | {STATUS} |

**Dependencies:** {NONE / US-XXX}

---

## EP-03: {EPIC_3_NAME}

> _(Repeat epic/story pattern as above for EP-03, EP-04, etc.)_

---

## Story Map Overview

```
User Activities
│
├── {ACTIVITY_1}
│   ├── US-001 {STORY_TITLE}
│   ├── US-002 {STORY_TITLE}
│   └── US-003 {STORY_TITLE}
│
├── {ACTIVITY_2}
│   ├── US-004 {STORY_TITLE}
│   └── US-005 {STORY_TITLE}
│
└── {ACTIVITY_3}
    ├── US-006 {STORY_TITLE}
    └── US-007 {STORY_TITLE}

━━━━━━━━━━━━━━━ MVP / Release 1 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    US-001  US-002  US-004

━━━━━━━━━━━━━━━ Release 2 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    US-003  US-005  US-006

━━━━━━━━━━━━━━━ Future ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    US-007
```

---

## Backlog Prioritisation Rationale

| Factor                            | Rationale for Current Ordering                                       |
|-----------------------------------|----------------------------------------------------------------------|
| Business value                    | {HIGH_VALUE_STORIES} deliver core value and enable MVP               |
| Technical dependencies            | {FOUNDATIONAL_STORIES} must precede dependent stories                |
| Risk reduction                    | {HIGH_RISK_STORIES} scheduled early to surface unknowns              |
| Stakeholder feedback loops        | Core user journeys prioritised for early UAT                         |
| Team learning                     | Domain model foundation stories first to align team understanding    |

---

## Sprint Allocation Suggestion

| Sprint | Stories               | Capacity (pts) | Theme                                  |
|--------|-----------------------|----------------|----------------------------------------|
| 1      | US-001, US-002        | {N}            | Core domain model + CI/CD setup        |
| 2      | US-003, US-004        | {N}            | {THEME}                                |
| 3      | US-005, US-006        | {N}            | {THEME}                                |
| 4      | US-007, US-008        | {N}            | {THEME}                                |
| N      | US-XXX, hardening     | {N}            | Bug fixing, performance, UAT support   |

> **Note:** Allocation is a suggestion. Final sprint commitments are determined during sprint planning with the full team based on actual velocity and team availability.
