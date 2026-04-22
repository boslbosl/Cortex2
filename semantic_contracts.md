# Semantic Contracts v0.1
## Cortex Agent for BigQuery

## Purpose
Define the first set of business metrics so the agent can disambiguate terms, generate consistent SQL, and provide traceable reporting outputs.

## Global Contract Rules
- Every metric must include: owner, grain, canonical SQL logic, filters, and caveats.
- Default timezone: `UTC` unless overridden per metric.
- Currency normalization: USD for aggregated financial metrics in v0.1.
- Exclusion policy: internal test accounts excluded where `is_test_account = TRUE`.

## Metric Catalog (Top 10)

### 1) Active Users (DAU)
- **Metric ID:** `active_users_dau`
- **Owner:** Product Analytics
- **Grain:** day
- **Definition:** Count of distinct users with at least one qualifying product event on a calendar day.
- **Canonical Logic:** `COUNT(DISTINCT user_id)` where `event_name IN ('session_start','page_view','feature_use')`.
- **Dimensions:** date, country, platform, plan_tier.
- **Caveat:** Event ingestion delays can undercount same-day values.

### 2) Active Users (WAU)
- **Metric ID:** `active_users_wau`
- **Owner:** Product Analytics
- **Grain:** week
- **Definition:** Distinct users active at least once in trailing 7 days.
- **Canonical Logic:** rolling distinct count over 7-day window.
- **Dimensions:** week_start, country, platform, plan_tier.

### 3) New Subscribers
- **Metric ID:** `new_subscribers`
- **Owner:** Growth
- **Grain:** day
- **Definition:** Count of users whose first paid subscription starts on date D.
- **Canonical Logic:** `COUNT(DISTINCT user_id)` where `subscription_start_date = D` and `is_paid = TRUE`.
- **Dimensions:** date, acquisition_channel, plan_tier.

### 4) Churned Subscribers
- **Metric ID:** `churned_subscribers`
- **Owner:** Lifecycle
- **Grain:** day
- **Definition:** Count of paid subscribers whose subscription ended on date D and did not renew within grace period.
- **Canonical Logic:** ended subscriptions with `renewed_within_14_days = FALSE`.
- **Dimensions:** date, region, plan_tier, tenure_bucket.

### 5) Gross Revenue
- **Metric ID:** `gross_revenue`
- **Owner:** Finance Analytics
- **Grain:** day
- **Definition:** Sum of all recognized subscription and one-time payments before refunds.
- **Canonical Logic:** `SUM(amount_usd)` for successful payments.
- **Dimensions:** date, country, plan_tier, product_line.

### 6) Net Revenue
- **Metric ID:** `net_revenue`
- **Owner:** Finance Analytics
- **Grain:** day
- **Definition:** Gross revenue minus refunds and chargebacks.
- **Canonical Logic:** `SUM(amount_usd) - SUM(refund_usd) - SUM(chargeback_usd)`.
- **Dimensions:** date, country, plan_tier, product_line.

### 7) Average Revenue Per User (ARPU)
- **Metric ID:** `arpu`
- **Owner:** Finance Analytics
- **Grain:** day
- **Definition:** Net revenue divided by active paid users.
- **Canonical Logic:** `net_revenue / NULLIF(active_paid_users,0)`.
- **Dimensions:** date, region, plan_tier.

### 8) Retention Rate (D30)
- **Metric ID:** `retention_d30`
- **Owner:** Lifecycle
- **Grain:** cohort month
- **Definition:** Percentage of users active on day 30 after cohort start.
- **Canonical Logic:** `retained_users_day30 / cohort_size`.
- **Dimensions:** cohort_month, acquisition_channel, plan_tier.

### 9) Conversion Rate (Trial → Paid)
- **Metric ID:** `trial_to_paid_conversion`
- **Owner:** Growth
- **Grain:** week
- **Definition:** Share of trial users converting to paid within 14 days.
- **Canonical Logic:** `converted_trial_users_14d / total_trial_users`.
- **Dimensions:** week_start, channel, region, plan_tier.

### 10) Query Cost per Insight Run
- **Metric ID:** `query_cost_per_run`
- **Owner:** Data Platform
- **Grain:** execution run
- **Definition:** Estimated BigQuery cost for a single analysis/report run.
- **Canonical Logic:** derived from bytes scanned in dry-run x pricing factor.
- **Dimensions:** workspace, user_role, workflow_type.

## Disambiguation Prompts (Initial)
- “When you say retention, should I use D7, D30, or monthly retention?”
- “For revenue, do you want gross or net?”
- “Should churn include users who reactivated within 14 days?”
- “Do you want all users or paid users only?”

## Next Steps
1. Validate metric definitions with Product, Finance, and Lifecycle owners.
2. Map metric IDs to physical tables/views in BigQuery.
3. Add versioning (`v0.2`) after stakeholder sign-off.
