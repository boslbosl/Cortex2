# Weekly KPI Report Template

## Report Metadata
- **Report Week:** {{week_start}} to {{week_end}}
- **Prepared For:** {{audience}}
- **Prepared By:** Cortex Agent
- **Data Freshness Timestamp:** {{data_freshness_ts}}
- **Semantic Contract Version:** {{semantic_version}}

## 1) Executive Summary
{{executive_summary_3_to_5_bullets}}

## 2) KPI Snapshot
| KPI | Current Week | Prior Week | WoW Change | Target | Status |
|-----|--------------:|-----------:|-----------:|-------:|--------|
| Active Users (WAU) | {{wau}} | {{wau_prev}} | {{wau_wow}} | {{wau_target}} | {{wau_status}} |
| New Subscribers | {{new_subs}} | {{new_subs_prev}} | {{new_subs_wow}} | {{new_subs_target}} | {{new_subs_status}} |
| Churned Subscribers | {{churned}} | {{churned_prev}} | {{churned_wow}} | {{churn_target}} | {{churn_status}} |
| Net Revenue | {{net_rev}} | {{net_rev_prev}} | {{net_rev_wow}} | {{net_rev_target}} | {{net_rev_status}} |
| Trial→Paid Conversion | {{conv}} | {{conv_prev}} | {{conv_wow}} | {{conv_target}} | {{conv_status}} |

## 3) Trend Analysis
### 3.1 User Growth and Engagement
{{wau_trend_narrative}}

### 3.2 Monetization
{{revenue_trend_narrative}}

### 3.3 Retention and Churn
{{retention_churn_narrative}}

## 4) Segment Highlights
- **Top positive segment:** {{top_segment_positive}}
- **Top negative segment:** {{top_segment_negative}}
- **Notable regional trend:** {{regional_trend}}

## 5) Risks & Caveats
- {{risk_or_data_quality_note_1}}
- {{risk_or_data_quality_note_2}}

## 6) Recommended Actions (Next 7 Days)
1. {{recommended_action_1}}
2. {{recommended_action_2}}
3. {{recommended_action_3}}

## 7) Evidence & Lineage
- **Primary queries:**
  - `{{query_ref_1}}`
  - `{{query_ref_2}}`
- **Source marts/tables:** {{source_assets}}
- **Quality checks passed:** {{quality_checks_passed}}
- **Quality checks failed/warned:** {{quality_checks_issues}}

## 8) Appendix
### Metric Definitions
- WAU: `active_users_wau`
- Net Revenue: `net_revenue`
- Churned Subscribers: `churned_subscribers`

### Change Log
- {{report_version}} generated at {{generated_timestamp}}
