# Product Requirements Document (PRD)
## Cortex Agent for BigQuery (Snowflake Cortex-inspired Analytics Assistant)

## 1) Document Control
- **Product Name:** Cortex Agent for BigQuery
- **Author:** AI Agent
- **Date:** April 21, 2026
- **Version:** v0.1 (Draft)
- **Status:** In Progress

## 2) Problem Statement
Data scientists and analytics engineers working on BigQuery often switch across many tools to complete an end-to-end analytics workflow:
1. source discovery,
2. data mart modeling,
3. quality validation,
4. exploratory analysis,
5. visualization/reporting,
6. sharing decisions with stakeholders.

This context switching increases cycle time, creates reproducibility issues, and leaves non-technical consumers disconnected from how metrics are produced.

## 3) Vision
Create an AI-powered Cortex-style agent experience for BigQuery that acts as a guided copilot for the complete data science journey—from data mart construction to analysis and report generation—while enforcing governance, quality checks, and reproducibility.

## 4) Goals and Non-Goals
### Goals
- Enable conversational and guided workflow for building governed data marts in BigQuery.
- Accelerate analysis with AI-assisted SQL generation, validation, and interpretation.
- Produce stakeholder-ready reports with traceable metric lineage.
- Provide robust guardrails for security, access control, and cost management.
- Preserve reproducibility through versioned assets (SQL, semantic definitions, notebooks, reports).

### Non-Goals (Phase 1)
- Replacing BigQuery native administration workflows.
- Providing fully autonomous execution without approval for destructive operations.
- Building a complete BI platform replacement.

## 5) Personas
1. **Data Scientist (Primary):** Builds marts/features, runs analyses, publishes insights.
2. **Analytics Engineer:** Maintains dimensional models and data contracts.
3. **Business Analyst:** Consumes and tweaks reports via natural language.
4. **Data Governance Owner:** Monitors policy compliance and access boundaries.
5. **Engineering Manager / Executive:** Consumes KPI narratives and trend explanations.

## 6) User Journey (E2E)
### Stage A: Discovery & Scoping
- User asks: “Build a retention mart for subscription customers.”
- Agent discovers relevant datasets, schemas, freshness, and ownership metadata.
- Agent proposes mart grain, dimensions, and metrics with confidence notes.

### Stage B: Data Mart Design
- Agent drafts modeling plan (star/snowflake or wide table strategy).
- Generates SQL for staging and mart layers.
- Validates joins, duplication risk, nullability, and business key integrity.
- Requests user approval before create/replace operations.

### Stage C: Data Quality & Governance
- Agent runs quality checks (freshness, volume drift, uniqueness, accepted values).
- Applies policy checks (PII masking, row-level constraints, access roles).
- Produces a quality scorecard and blocks report generation on critical failures.

### Stage D: Analysis
- User asks analytical questions in natural language.
- Agent converts to optimized SQL and shows query rationale.
- Returns tables/charts and plain-language interpretation.
- Suggests deeper cuts (cohorts, seasonality, segmentation, anomalies).

### Stage E: Report Generation
- Agent builds report sections:
  - executive summary,
  - KPI snapshots,
  - trend charts,
  - metric definitions,
  - caveats.
- Exports report in markdown/PDF/BI handoff format.
- Includes metric lineage and query references for auditability.

### Stage F: Collaboration & Iteration
- Stakeholders comment: “Break down churn by region and plan.”
- Agent updates report, re-runs dependent queries, and versions output.
- Maintains change log across prompts, SQL revisions, and report versions.

## 7) Functional Requirements
### FR-1 Conversational Planning
- Parse user objective and generate structured plan (inputs, outputs, assumptions).
- Ask clarifying questions when metric definitions are ambiguous.

### FR-2 Metadata-Aware SQL Generation
- Generate BigQuery SQL using discovered schemas and semantic hints.
- Annotate generated SQL with rationale and expected output schema.

### FR-3 Data Mart Build Orchestration
- Support staged pipelines (raw → staging → mart).
- Support incremental and full refresh modes.
- Include dry-run mode for cost estimation and syntax validation.

### FR-4 Data Quality Validation
- Run configurable checks with severity levels (info/warn/error).
- Persist validation results with timestamps and run context.

### FR-5 Governance & Security Enforcement
- Enforce role-based constraints before query execution.
- Detect and redact sensitive columns from generated outputs where required.

### FR-6 Analysis Assistant
- Convert natural language to SQL + chart recommendations.
- Provide uncertainty indicators and alternative query patterns.

### FR-7 Report Composer
- Auto-generate narrative summaries tied to result sets.
- Support report templates by audience (exec, product, operations).

### FR-8 Traceability
- Link every result to source SQL, data timestamp, and semantic metric version.

## 8) Non-Functional Requirements
- **Performance:** Typical analytical response under 10 seconds for cached/small workloads; clear async handling for long queries.
- **Scalability:** Support concurrent analysts without cross-session leakage.
- **Reliability:** Graceful retries and resumable jobs.
- **Security:** Principle of least privilege; audit logs for actions and data access.
- **Cost Control:** Query cost estimation, budget alerts, and optimization suggestions.
- **Observability:** Logs, traces, and quality metrics for each workflow stage.

## 9) Success Metrics
- **Time-to-first-mart:** reduce by 40%.
- **Analysis turnaround:** reduce median question-to-insight time by 30%.
- **Quality compliance:** >95% marts passing critical quality checks.
- **Adoption:** weekly active analyst usage and report reuse rates.
- **Trust:** stakeholder confidence score from periodic survey.

## 10) Risks & Mitigations
- **Risk:** Ambiguous business definitions lead to incorrect metrics.
  - **Mitigation:** mandatory metric confirmation prompts + semantic catalog.
- **Risk:** High BigQuery costs from generated SQL.
  - **Mitigation:** default dry-run and query cost caps.
- **Risk:** Sensitive data exposure.
  - **Mitigation:** policy-aware column filtering and output redaction.

## 11) Release Plan
### Phase 1 (MVP)
- Conversational planning, SQL generation, mart orchestration, quality checks, markdown report generation.

### Phase 2
- Advanced semantic layer integration, collaborative workflows, richer BI publishing connectors.

### Phase 3
- Proactive insights, anomaly detection, and autonomous recommendation loops.

## 12) Open Questions
- Which semantic/metrics store is the source of truth?
- What report export formats are mandatory for initial users?
- How should approval workflows integrate with existing CI/CD and data governance gates?
