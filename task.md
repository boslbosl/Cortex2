# Task Plan
## Cortex Agent for BigQuery

## 1) Purpose
Translate the PRD and architecture into an executable implementation roadmap and begin immediate execution.

## 2) Current Status
- **Started On:** April 21, 2026
- **Overall Status:** In Progress
- **Current Sprint Focus:** Foundation and design alignment
- **Latest Update:** Initial execution artifacts for Epic 2 / 3 / 5 drafted.

## 3) Work Breakdown Structure

### Epic 1: Product and Architecture Baseline
- [x] T1.1 Draft PRD with complete E2E user journey.
- [x] T1.2 Draft system architecture with component/data flow.
- [x] T1.3 Define initial success metrics and risks.

### Epic 2: Semantic & Metadata Foundation
- [x] T2.1 Define semantic contracts for core metrics (retention, churn, revenue). *(v0.1 draft complete in `semantic_contracts.md`)*
- [ ] T2.2 Implement metadata discovery service contract.
- [x] T2.3 Create metric disambiguation question templates. *(included in `semantic_contracts.md`)*

### Epic 3: Mart Builder
- [x] T3.1 Implement planning schema for mart design (grain, dimensions, metrics). *(v0.1 draft schema in `mart_planning_schema.json`)*
- [ ] T3.2 Implement SQL generation module for BigQuery.
- [ ] T3.3 Add dry-run cost estimation and execution guardrails.

### Epic 4: Governance and Quality
- [ ] T4.1 Integrate policy checks (access, PII, masking).
- [ ] T4.2 Implement quality checks (freshness, uniqueness, drift).
- [ ] T4.3 Build quality scorecard and blocking rules.

### Epic 5: Analysis and Reporting
- [ ] T5.1 Implement NL-to-SQL analysis interface.
- [ ] T5.2 Generate chart recommendations and interpretation.
- [x] T5.3 Implement report composer templates and exports. *(weekly KPI template draft in `report_template_weekly_kpi.md`)*

### Epic 6: Observability and Reliability
- [ ] T6.1 Add execution tracing and audit logs.
- [ ] T6.2 Define retry strategy and dead-letter handling.
- [ ] T6.3 Define SLO dashboards for latency, error rate, and quality compliance.

## 4) Self-Review
### What was done well
- Covered full E2E data scientist journey from mart creation to reporting.
- Included governance, quality, and traceability requirements.
- Mapped architecture components directly to workflow stages.
- Converted roadmap into concrete starter artifacts (semantic contracts, schema, report template).

### Gaps / Improvements Needed
- Need explicit API schemas and payload examples.
- Need implementation-level decisions for auth token propagation.
- Need prioritized backlog by business value and technical risk.
- Need executable prototypes that validate schema and metric logic against sample BigQuery data.

### Risks in Current Plan
- Scope may exceed MVP if all quality features are included up-front.
- Semantic ambiguity can delay analysis accuracy without metric governance.
- Draft metric definitions may diverge from finance/source-of-truth unless formally approved.

## 5) “Start the Task” Actions Completed
- [x] Created foundational artifacts: `prd.md`, `architecture.md`, `task.md`.
- [x] Marked baseline tasks complete for Epic 1.
- [x] Drafted semantic contracts (`semantic_contracts.md`).
- [x] Drafted mart planning schema (`mart_planning_schema.json`).
- [x] Drafted weekly KPI report template (`report_template_weekly_kpi.md`).

## 6) Immediate Next Actions (Next 1–2 Iterations)
1. Validate `semantic_contracts.md` with Product/Finance/Lifecycle owners and freeze v1.0.
2. Implement metadata discovery service contract and sample response payloads.
3. Build SQL generation prototype that accepts `mart_planning_schema.json` input.
4. Add dry-run cost estimator wrapper for generated BigQuery SQL.

## 7) Progress Log
- **2026-04-21:** Completed planning docs (`prd.md`, `architecture.md`, `task.md`).
- **2026-04-21:** Started execution by drafting semantic metric contracts and disambiguation prompts.
- **2026-04-21:** Drafted mart planning JSON schema and weekly KPI report template.
