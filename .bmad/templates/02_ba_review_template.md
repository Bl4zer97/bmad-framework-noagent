# BA Review — AI Ready Gate

---

## Review Metadata

| Field            | Value                                         |
|------------------|-----------------------------------------------|
| Project Name     | `{PROJECT_NAME}`                              |
| PRD Version      | `{PRD_VERSION}` (e.g., v1.2)                 |
| Breakdown Ref    | `{BREAKDOWN_VERSION}`                         |
| Reviewer         | `{BA_REVIEWER_NAME}`                          |
| Review Date      | `{DATE}`                                      |
| Status           | `{DRAFT / UNDER_REVIEW / COMPLETE}`           |
| Gate             | **AI Ready Gate**                             |
| Pass Threshold   | **90 / 100**                                  |

---

## Executive Summary

> _Provide a 2–3 paragraph overview of the review findings, overall quality of the PRD and Spec Breakdown, and a clear recommendation for whether the project is ready to proceed to the Architecture and Implementation phases with AI assistance._

{EXECUTIVE_SUMMARY}

---

## AI Ready Score

> _Each criterion is scored 0–10. The weighted score is calculated as `(Score / 10) × Weight`. Total maximum = 100._

| #  | Criterion                            | Weight | Score (0–10) | Weighted Score | Notes / Evidence                                  |
|----|--------------------------------------|--------|--------------|----------------|---------------------------------------------------|
| 1  | Requirements Clarity                 | 15     | {SCORE}      | {CALC}         | {NOTES}                                           |
| 2  | Acceptance Criteria Completeness     | 15     | {SCORE}      | {CALC}         | {NOTES}                                           |
| 3  | ATDD Scenario Coverage               | 10     | {SCORE}      | {CALC}         | {NOTES}                                           |
| 4  | Domain Model Accuracy                | 10     | {SCORE}      | {CALC}         | {NOTES}                                           |
| 5  | Technical Constraints Defined        | 10     | {SCORE}      | {CALC}         | {NOTES}                                           |
| 6  | Integration Points Documented        | 10     | {SCORE}      | {CALC}         | {NOTES}                                           |
| 7  | NFRs Measurable & Testable           | 10     | {SCORE}      | {CALC}         | {NOTES}                                           |
| 8  | Scope & Out-of-Scope Clear           | 10     | {SCORE}      | {CALC}         | {NOTES}                                           |
| 9  | Risk Register Populated              | 10     | {SCORE}      | {CALC}         | {NOTES}                                           |
|    | **TOTAL**                            | **100**| —            | **{TOTAL}**    | Pass ≥ 90 \| Fail < 90                            |

### Score Calculation Guide

| Score | Rating      | Meaning                                                                               |
|-------|-------------|---------------------------------------------------------------------------------------|
| 10    | Excellent   | Fully addressed, unambiguous, directly actionable by AI agent                         |
| 8–9   | Good        | Minor gaps or ambiguity; can proceed with low risk                                    |
| 6–7   | Acceptable  | Moderate gaps; should be resolved before AI implementation begins                     |
| 4–5   | Poor        | Significant gaps that will cause rework; must be addressed                            |
| 0–3   | Failing     | Missing or contradictory; blocks AI-assisted implementation entirely                  |

---

## Gate Status

```
┌─────────────────────────────────────────────────────────────────┐
│                      AI READY GATE STATUS                       │
│                                                                 │
│   Total Score: {TOTAL} / 100                                    │
│   Threshold:   90 / 100                                         │
│                                                                 │
│   Status: [ AI_READY APPROVED ✅ ]  or  [ AI_READY BLOCKED ❌ ] │
└─────────────────────────────────────────────────────────────────┘
```

---

## Detailed Analysis

### 1. Requirements Clarity (Weight: 15)

**Score: {SCORE} / 10**

**Findings:**
> {DETAILED_FINDINGS_FOR_REQUIREMENTS_CLARITY}

**Evidence (well-formed examples):**
- {EXAMPLE_GOOD_REQUIREMENT}

**Issues found:**
- {AMBIGUOUS_OR_INCOMPLETE_REQUIREMENT}

---

### 2. Acceptance Criteria Completeness (Weight: 15)

**Score: {SCORE} / 10**

**Findings:**
> {DETAILED_FINDINGS}

**Checklist:**
- [ ] Each functional requirement has at least one acceptance criterion
- [ ] Acceptance criteria are testable (not subjective)
- [ ] Edge cases and error states are included
- [ ] Criteria reference specific, measurable values (not "fast", "easy")

**Issues found:**
- {ISSUE}

---

### 3. ATDD Scenario Coverage (Weight: 10)

**Score: {SCORE} / 10**

**Findings:**
> {DETAILED_FINDINGS}

**Checklist:**
- [ ] Happy path scenarios defined for each feature
- [ ] Unhappy path / error scenarios defined
- [ ] Boundary condition scenarios defined
- [ ] Scenarios follow Given/When/Then format consistently
- [ ] Scenarios are independent and repeatable

**Issues found:**
- {ISSUE}

---

### 4. Domain Model Accuracy (Weight: 10)

**Score: {SCORE} / 10**

**Findings:**
> {DETAILED_FINDINGS}

**Checklist:**
- [ ] All key domain entities identified
- [ ] Aggregate boundaries are appropriate (not too large, not too small)
- [ ] Value objects correctly distinguished from entities
- [ ] Domain events identified for key state transitions
- [ ] Bounded contexts are clearly delineated
- [ ] Ubiquitous language is consistent throughout documents

**Issues found:**
- {ISSUE}

---

### 5. Technical Constraints Defined (Weight: 10)

**Score: {SCORE} / 10**

**Findings:**
> {DETAILED_FINDINGS}

**Checklist:**
- [ ] .NET version specified (must be .NET 8 LTS)
- [ ] Cloud platform and services identified
- [ ] Database technology chosen and justified
- [ ] Authentication/authorisation approach specified
- [ ] ORM and key NuGet packages listed
- [ ] CI/CD tooling specified
- [ ] Compliance/regulatory constraints noted

**Issues found:**
- {ISSUE}

---

### 6. Integration Points Documented (Weight: 10)

**Score: {SCORE} / 10**

**Findings:**
> {DETAILED_FINDINGS}

**Checklist:**
- [ ] All external systems listed in integration table
- [ ] Protocol and auth method specified for each integration
- [ ] Data direction (inbound/outbound/bidirectional) noted
- [ ] Owners of external systems identified
- [ ] API versions / contracts pinned where applicable
- [ ] Error handling strategy for integration failures noted

**Issues found:**
- {ISSUE}

---

### 7. NFRs Measurable & Testable (Weight: 10)

**Score: {SCORE} / 10**

**Findings:**
> {DETAILED_FINDINGS}

**Checklist:**
- [ ] Performance targets use specific numbers (e.g., p95 < 200ms, not "fast")
- [ ] Availability SLA is a percentage with measurement method
- [ ] Security NFRs reference standards (OWASP, TLS 1.2+)
- [ ] Scalability targets expressed as concrete metrics
- [ ] .NET-specific NFRs (nullable enabled, async/await) are listed
- [ ] Each NFR has a clear test/verification method

**Issues found:**
- {ISSUE}

---

### 8. Scope & Out-of-Scope Clear (Weight: 10)

**Score: {SCORE} / 10**

**Findings:**
> {DETAILED_FINDINGS}

**Checklist:**
- [ ] Out-of-scope section explicitly lists excluded items
- [ ] No ambiguous "maybe" features that could cause scope creep
- [ ] Phase 1 vs future phases clearly delineated
- [ ] Mobile / legacy system scope explicitly addressed

**Issues found:**
- {ISSUE}

---

### 9. Risk Register Populated (Weight: 10)

**Score: {SCORE} / 10**

**Findings:**
> {DETAILED_FINDINGS}

**Checklist:**
- [ ] At least 5 risks identified
- [ ] Each risk has probability, impact, and mitigation
- [ ] Technical risks (EF migrations, Azure quotas) included
- [ ] Dependency risks captured
- [ ] Risk owners assigned

**Issues found:**
- {ISSUE}

---

## Critical Issues (Blockers)

> _These issues MUST be resolved before AI-assisted implementation can begin, regardless of overall score._

| ID   | Issue Description                                              | Criterion Ref | Recommended Action                              |
|------|----------------------------------------------------------------|---------------|-------------------------------------------------|
| CI-1 | {CRITICAL_ISSUE_DESCRIPTION}                                   | #{CRITERION}  | {RECOMMENDED_ACTION}                            |
| CI-2 | {CRITICAL_ISSUE_DESCRIPTION}                                   | #{CRITERION}  | {RECOMMENDED_ACTION}                            |

> _If no critical issues: "No critical blockers identified."_

---

## Recommendations

> _Non-blocking improvements that will reduce risk or improve AI implementation quality._

| Priority | Recommendation                                                   | Criterion Ref | Effort |
|----------|------------------------------------------------------------------|---------------|--------|
| High     | {RECOMMENDATION}                                                  | #{CRITERION}  | Small / Med / Large |
| Medium   | {RECOMMENDATION}                                                  | #{CRITERION}  | Small / Med / Large |
| Low      | {RECOMMENDATION}                                                  | #{CRITERION}  | Small / Med / Large |

---

## Gate Decision

```
╔═══════════════════════════════════════════════════════════════════╗
║              AI READY GATE — FINAL DECISION                       ║
║                                                                   ║
║  Score: {TOTAL} / 100   |   Threshold: 90 / 100                  ║
║                                                                   ║
║  Critical Blockers: {0 / N}                                       ║
║                                                                   ║
║  ✅ AI_READY APPROVED — Proceed to Architecture Phase             ║
║     OR                                                            ║
║  ❌ AI_READY BLOCKED — Address issues then re-submit for review   ║
╚═══════════════════════════════════════════════════════════════════╝
```

**Conditions for approval (if partially approved):**
- {CONDITION_1}
- {CONDITION_2}

---

## Sign-off

| Role             | Name                    | Signature / Approval   | Date       |
|------------------|-------------------------|------------------------|------------|
| BA Reviewer      | {BA_REVIEWER_NAME}      | {APPROVED / BLOCKED}   | {DATE}     |
| Product Owner    | {PO_NAME}               | {APPROVED / BLOCKED}   | {DATE}     |
| Tech Lead        | {TECH_LEAD_NAME}        | {APPROVED / BLOCKED}   | {DATE}     |

---

## Revision History

| Version | Date    | Author        | Change Summary                                  |
|---------|---------|---------------|-------------------------------------------------|
| 0.1     | {DATE}  | {AUTHOR}      | Initial review draft                            |
| 1.0     | {DATE}  | {AUTHOR}      | Final gate decision published                   |
| {VER}   | {DATE}  | {AUTHOR}      | {CHANGE — e.g., re-review after issues resolved}|
