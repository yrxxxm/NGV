---
name: aspice-auditor
description: Use this skill when auditing or reviewing engineering work products (requirements docs, architecture/design docs, test specs/reports, review records, change requests, plans, baselines) against Automotive SPICE (A-SPICE) 4.1 Capability Level 2 (CL2) — i.e. PA2.1 Performance Management and PA2.2 Work Product Management. Load it before rating any process attribute or judging whether a work product satisfies CL2, and before the aspice-cl2-auditor agent does anything else.
---

# A-SPICE 4.1 — CL2 Auditor Reference

This skill is the reference material for assessing **Capability Level 2** under the
Automotive SPICE 4.1 Process Assessment Model (PAM). CL2 is reached at a process only
when both PA2.1 and PA2.2 are rated Largely (L) or Fully (F) achieved, **in addition to**
PA1.1 Process Performance already being L/F.

Do not assess from memory alone beyond what's below — always tie a rating to a concrete
artifact (a file, a section, a review record) found in the repository. If no artifact
exists for an indicator, the rating cannot be higher than P (Partially achieved), and
usually N (Not achieved) if nothing at all addresses it.

## Rating scale (per ISO/IEC 33020)

| Rating | Meaning | Achievement |
|---|---|---|
| N | Not achieved | 0–15% |
| P | Partially achieved | >15–50% |
| L | Largely achieved | >50–85% |
| F | Fully achieved | >85–100% |

CL2 = PA1.1 (L/F) **and** PA2.1 (L/F) **and** PA2.2 (L/F) at that process.

---

## PA2.1 Performance Management — generic practices to check for

For the process instance in scope, look for evidence of:

- **GP 2.1.1 — Objectives for performance identified.** A stated goal/scope for this
  process activity (e.g., in a project plan, quality plan, or process description) —
  not just "we do requirements elicitation" but what this instance is meant to achieve
  (scope, quality targets, exit criteria).
- **GP 2.1.2 — Performance planned and monitored.** A plan (schedule, milestones,
  effort) for the process activities, and monitoring records (status reports, milestone
  reviews, metrics/KPIs) showing actual vs. planned.
- **GP 2.1.3 — Performance adjusted.** Evidence that deviations from the plan were
  identified and acted on (re-planning, corrective action, escalation) — not just that
  a plan exists, but that it was actually used to steer the work.
- **GP 2.1.4 — Responsibilities and authorities defined.** Named roles/owners for the
  process activities and its work products (RACI, role assignment in a plan, or
  org/role definitions referenced by the project).
- **GP 2.1.5 — Resources and infrastructure identified, made available, used and
  maintained.** Tooling, environments, and staffing needed for the process are
  identified and actually in place (tool list, environment setup docs, licenses,
  training records) — not just assumed.

Typical evidence sources: project management plan, quality plan, status/progress
reports, meeting minutes, tool chain description, risk/issue logs, metrics dashboards.

Common CL2 failure pattern for PA2.1: a plan exists but there is no monitoring record,
or monitoring exists but no evidence of adjustment when off-track (plan is static/never
revisited).

---

## PA2.2 Work Product Management — generic practices to check for

For each **output work product** of the process in scope, look for evidence of:

- **GP 2.2.1 — Requirements for work products identified.** Content/structure
  requirements are defined (a template, a documented standard, a checklist) before the
  work product is produced — not invented after the fact.
- **GP 2.2.2 — Requirements for documentation and control of work products defined.**
  A defined scheme for identification (naming/IDs), version numbering, and where the
  work product is stored/controlled (repository, DMS, PLM).
- **GP 2.2.3 — Work products appropriately identified, documented, and controlled.**
  The actual work product has a unique ID, version, author, date, and is under
  configuration/version control (not a loose file with no history).
- **GP 2.2.4 — Work products reviewed against defined requirements/criteria.** A
  documented review (peer review, formal review, checklist-based review) with
  reviewer names, date, findings, and disposition — not just "it was discussed."
- **GP 2.2.5 — Changes to work products managed / traceability maintained.** Change
  history is visible (diffs, revision log, change requests linked), and
  bidirectional traceability to related work products is maintained (e.g.,
  requirement → design element → test case → test result), including impact
  analysis when a source item changes.

Common CL2 failure pattern for PA2.2: the work product itself is fine in content
(satisfies base practices / PA1.1) but has no visible review record, no version
history, or a broken/missing traceability link — this caps PA2.2 at P even if the
content is excellent.

---

## Process-specific work product checklists

Use these as the concrete "what should exist" list per process when checking GP
2.2.1–2.2.5. Content items are the expected minimum; adapt to project context but
flag missing items as gaps.

### SYS.2 System Requirements Analysis
- System requirements specification: unique ID per requirement, source/rationale,
  verification criteria, priority, status, version/baseline.
- Bidirectional trace: stakeholder requirement ↔ system requirement.
- Review record for the requirements specification (criteria: correctness,
  completeness, consistency, verifiability, feasibility).

### SYS.3 System Architectural Design
- Architecture description: elements, interfaces, dynamic behavior, allocation of
  requirements to elements.
- Trace: system requirement ↔ architectural element.
- Review record against defined architecture evaluation criteria (e.g., resource
  usage, testability, modularity).

### SYS.4 System Integration and Integration Test / SYS.5 System Qualification Test
- Test strategy/spec: test cases linked to requirements, pass/fail criteria,
  environment/tooling.
- Test results/report: actual results, defects raised, retest evidence.
- Trace: requirement ↔ test case ↔ test result.

### SWE.1 Software Requirements Analysis
- Software requirements spec with unique IDs, verification criteria, version.
- Trace: system requirement ↔ software requirement.
- Review record (completeness/consistency/verifiability vs. system requirements).

### SWE.2 Software Architectural Design / SWE.3 Software Detailed Design
- Design description at each level with interfaces and dynamic behavior.
- Trace: software requirement ↔ architectural element ↔ detailed design element.
- Review record against design/coding evaluation criteria.

### SWE.4 Software Unit Verification
- Unit test spec/cases, static analysis results, coverage results.
- Trace: detailed design/unit ↔ unit test case.
- Evidence of defect logging and closure.

### SWE.5 Software Integration and Integration Test / SWE.6 Software Qualification Test
- Integration test strategy/spec and results, test environment description.
- Trace: architecture element / software requirement ↔ test case ↔ result.
- Regression test evidence when a change occurs.

### SUP.1 Quality Assurance
- QA plan/records, audit reports, non-conformance records with disposition and
  closure evidence.

### SUP.8 Configuration Management
- CM plan, baseline records, identification of configuration items, change history,
  evidence that baselines are protected from unauthorized change.

### SUP.9 Problem Resolution Management
- Problem records with unique ID, status, priority, root cause, resolution,
  verification of fix, trace to affected work products.

### SUP.10 Change Request Management
- Change requests with unique ID, impact analysis, approval/decision record, trace
  to the work products actually changed, status tracking to closure.

### MAN.3 Project Management
- Project plan(s) with scope, schedule, resources, risks; progress
  tracking/monitoring records; evidence of re-planning/corrective action
  (this doubles as the primary PA2.1 evidence source across the project).

---

## Reporting format

Report one row per process assessed:

| Process | PA1.1 (context only) | PA2.1 | PA2.2 | Key gaps | Evidence needed |
|---|---|---|---|---|---|

Keep gap descriptions concrete: name the missing artifact or the specific indicator
(e.g., "no reviewer sign-off found on SRS v1.2" rather than "review process weak").
If PA1.1 itself is not L/F, say so explicitly and note that CL2 cannot be claimed
regardless of PA2.1/PA2.2, since CL2 requires PA1.1 as a prerequisite.
