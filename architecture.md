# Architecture Document
## Cortex Agent for BigQuery

## 1) Architecture Objectives
- Provide an end-to-end AI workflow from data mart build to analysis/reporting.
- Ensure governance, reproducibility, and cost-aware execution in BigQuery.
- Keep human-in-the-loop approval at sensitive or high-impact steps.

## 2) High-Level Architecture
```text
[User (DS/Analyst/Stakeholder)]
          |
          v
[Conversation UI / API Layer]
          |
          v
[Agent Orchestrator]
  |      |         |         |
  |      |         |         +--> [Report Composer]
  |      |         +------------> [Analysis Engine]
  |      +----------------------> [Mart Builder]
  +-----------------------------> [Governance + Quality Engine]
               |
               v
       [BigQuery Execution Layer]
               |
     +---------+---------+
     |                   |
[Metadata/Semantic Store] [Artifact Store (SQL, plans, reports, logs)]
```

## 3) Component Design
### 3.1 Conversation UI / API Layer
- Captures prompts, session context, user identity, and role.
- Presents plans, SQL previews, quality reports, and generated narratives.

### 3.2 Agent Orchestrator
- Core state machine managing lifecycle:
  1. Understand intent,
  2. Plan,
  3. Execute,
  4. Validate,
  5. Explain,
  6. Persist artifacts.
- Routes tasks to specialized engines.
- Handles retries, user confirmations, and failure recovery.

### 3.3 Metadata & Semantic Resolver
- Reads dataset schemas, table metadata, lineage, owners, freshness indicators.
- Resolves business metric definitions, dimensions, and governance tags.

### 3.4 Mart Builder
- Produces transformation plans and SQL scripts.
- Supports batch run, incremental run, and backfill strategies.
- Executes dry-run before materialization.

### 3.5 Governance + Quality Engine
- Enforces policies:
  - RBAC/ABAC checks,
  - PII constraints,
  - row/column masking.
- Executes data quality suite:
  - schema checks,
  - uniqueness,
  - null constraints,
  - distribution drift,
  - freshness SLAs.

### 3.6 Analysis Engine
- Converts natural language into BigQuery SQL.
- Applies SQL optimization heuristics and cost hints.
- Returns structured result sets and suggested visual encodings.

### 3.7 Report Composer
- Creates audience-specific report templates.
- Generates plain-language narratives with linked evidence.
- Exports versioned outputs (markdown/PDF compatible payload).

### 3.8 Artifact Store
Stores:
- prompt history,
- execution plans,
- generated SQL,
- quality outputs,
- report versions,
- audit logs.

## 4) Data Flow (E2E)
1. User provides objective (e.g., “Build churn mart and weekly report”).
2. Orchestrator fetches metadata + metric context.
3. Planner proposes mart blueprint and asks for confirmation.
4. Mart Builder runs SQL dry-run + cost estimate.
5. On approval, mart SQL is executed in BigQuery.
6. Quality Engine validates output tables.
7. Analysis Engine answers follow-up analytical questions.
8. Report Composer produces final narrative report.
9. All artifacts are persisted with lineage and timestamps.

## 5) Deployment View
- **Control Plane Services:** API layer, orchestrator, policy/quality services.
- **Data Plane:** BigQuery datasets, views, marts, temporary execution artifacts.
- **State Plane:** metadata cache, session state, artifact repository.

Environment tiers:
- dev,
- staging,
- production.

## 6) Security Architecture
- Federated identity (SSO/OIDC) for user authentication.
- Token propagation to enforce user-scoped data access.
- Policy checks pre- and post-query execution.
- Central audit trail for all read/write operations.

## 7) Reliability & Operations
- Idempotent execution keys for long-running workflows.
- Retry strategies for transient query failures.
- Dead-letter queue for failed orchestration steps.
- Observability:
  - structured logs,
  - distributed traces,
  - quality and latency dashboards.

## 8) Cost & Performance Controls
- BigQuery dry-run before execution to estimate bytes scanned.
- Partition and clustering recommendations.
- Query rewriting hints for expensive joins/subqueries.
- Session-level and workspace-level budget thresholds.

## 9) API/Contract Outline
### Core endpoints (illustrative)
- `POST /sessions`
- `POST /sessions/{id}/plan`
- `POST /sessions/{id}/execute-mart`
- `POST /sessions/{id}/analyze`
- `POST /sessions/{id}/report`
- `GET /sessions/{id}/artifacts`

## 10) Failure Modes
- Missing metric definitions → system pauses and asks for disambiguation.
- Policy violation detected → execution blocked, remediation suggestions returned.
- Quality check failure → report marked “draft - quality issues”.

## 11) Extensibility
- Plug-in adapters for semantic layers and BI tools.
- Custom quality test packs per domain.
- Optional advanced modules for forecasting/anomaly detection.
