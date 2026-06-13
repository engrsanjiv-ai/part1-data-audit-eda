# Data Quality Report — D2C Churn Dataset

**Prepared by:** Sanjiv 
**Dataset Source:** D2C Personal Care Brand — Internal Data Package  
**Report Date:** 12 Jun 2026
**Scope:** All files in the dataset package

---


## 1. Dataset Inventory (synchronized with `eda_audit.ipynb`)

The notebook `eda_audit.ipynb` loads files from the `data/` directory into these variables and uses the following canonical dataset keys in the `datasets` dictionary. Use these names when referencing the notebook outputs.

| File (path) | Notebook variable / dataset key | Expected key column | Row Count | Notes |
|-------------|---------------------------------|---------------------|----------:|-------|
| `data/customers.csv` | `customers` (`customers`) | `customer_id` | 2,400 rows × 9 cols | Demographics, signup metadata |
| `data/orders.csv` | `orders` (`orders`) | `order_id` | 10,009 rows × 10 cols | Transaction history, amounts, returns |
| `data/support_tickets.csv` | `tickets` (`support_tickets`) | `ticket_id` | 1,921 rows × 8 cols | Customer issues and resolution status |
| `data/web_events_snapshot.csv` | `events` (`web_app_events`) | `customer_id` | 2,400 rows × 10 cols | Session/event logs (web/mobile) |
| `data/churn_labels.csv` | `churn_labels` (`churn`) | `customer_id` | 2,400 rows × 4 cols | Churn label + `snapshot_date` (target variable) |
| `data/intervention_history.csv` | `campaigns` (`campaigns`) | `customer_id` | 2,400 rows × 5 cols | Campaign exposures and responses |
| `data/rfm_modeling_snapshot.csv` | `snapshot` (`modeling_snapshot`) | `customer_id` | 2,400 rows × 29 cols | Precomputed RFM features used for modeling |
| `data/DATA_DICTIONARY.md` | n/a | — | — | Field definitions and descriptions |
| `data/STUDENT_FACING_PROBLEM_STATEMENT.md` | n/a | — | — | Project / problem statement |
| `outputs/merged_master.csv` | `merged_master` (derived) | `customer_id` | (derived) | Consolidated master used by notebooks |

> Note: The notebook uses `DATA_DIR = 'data'` and a helper `load_csv()` which prints actual row/column counts at runtime — run `eda_audit.ipynb` to populate exact counts and any missing-file warnings.

---

## 2. Missing Values

### data/customers.csv

- **Rows:** 2400 — **Columns:** 9

| Column | Missing Count | Missing % |
|--------|---------------:|----------:|
| `customer_id` | 0 | 0.00% |
| `signup_date` | 0 | 0.00% |
| `city_tier` | 0 | 0.00% |
| `age_group` | 0 | 0.00% |
| `acquisition_channel` | 0 | 0.00% |
| `loyalty_tier` | 1,386 | 57.75% |
| `preferred_category` | 0 | 0.00% |
| `skin_type` | 401 | 16.71% |
| `marketing_consent` | 0 | 0.00% |

### data/orders.csv

- **Rows:** 10,009 — **Columns:** 10

| Column | Missing Count | Missing % |
|--------|---------------:|----------:|
| `order_id` | 0 | 0.00% |
| `customer_id` | 0 | 0.00% |
| `order_date` | 0 | 0.00% |
| `category` | 0 | 0.00% |
| `quantity` | 0 | 0.00% |
| `gross_amount` | 0 | 0.00% |
| `discount_pct` | 0 | 0.00% |
| `delivery_days` | 0 | 0.00% |
| `returned` | 0 | 0.00% |
| `rating` | 80 | 0.80% |

### data/support_tickets.csv

- **Rows:** 1,921 — **Columns:** 8

| Column | Missing Count | Missing % |
|--------|---------------:|----------:|
| `ticket_id` | 0 | 0.00% |
| `customer_id` | 0 | 0.00% |
| `ticket_date` | 0 | 0.00% |
| `issue_type` | 0 | 0.00% |
| `support_channel` | 0 | 0.00% |
| `resolution_hours` | 0 | 0.00% |
| `sentiment_score` | 0 | 0.00% |
| `reopened` | 0 | 0.00% |

### data/web_events_snapshot.csv

- **Rows:** 2400 — **Columns:** 10

| Column | Missing Count | Missing % |
|--------|---------------:|----------:|
| `customer_id` | 0 | 0.00% |
| `snapshot_date` | 0 | 0.00% |
| `sessions_30d` | 0 | 0.00% |
| `product_views_30d` | 0 | 0.00% |
| `cart_adds_30d` | 0 | 0.00% |
| `wishlist_adds_30d` | 0 | 0.00% |
| `abandoned_carts_30d` | 0 | 0.00% |
| `email_opens_30d` | 0 | 0.00% |
| `campaign_clicks_30d` | 0 | 0.00% |
| `last_visit_days_ago` | 0 | 0.00% |

### data/churn_labels.csv

- **Rows:** 2400 — **Columns:** 4

| Column | Missing Count | Missing % |
|--------|---------------:|----------:|
| `customer_id` | 0 | 0.00% |
| `snapshot_date` | 0 | 0.00% |
| `churn_next_60d` | 0 | 0.00% |
| `split` | 0 | 0.00% |

### data/intervention_history.csv

- **Rows:** 2400 — **Columns:** 5

| Column | Missing Count | Missing % |
|--------|---------------:|----------:|
| `customer_id` | 0 | 0.00% |
| `snapshot_date` | 0 | 0.00% |
| `last_campaign_received` | 507 | 21.12% |
| `last_campaign_cost` | 0 | 0.00% |
| `manual_priority_bucket` | 0 | 0.00% |

### data/rfm_modeling_snapshot.csv

- **Rows:** 2400 — **Columns:** 29

| Column | Missing Count | Missing % |
|--------|---------------:|----------:|
| `customer_id` | 0 | 0.00% |
| `snapshot_date` | 0 | 0.00% |
| `city_tier` | 0 | 0.00% |
| `age_group` | 0 | 0.00% |
| `acquisition_channel` | 0 | 0.00% |
| `loyalty_tier` | 1,386 | 57.75% |
| `preferred_category` | 0 | 0.00% |
| `marketing_consent` | 0 | 0.00% |
| `recency_days` | 0 | 0.00% |
| `frequency_180d` | 0 | 0.00% |
| `monetary_180d` | 0 | 0.00% |
| `return_rate_180d` | 0 | 0.00% |
| `avg_discount_pct_180d` | 0 | 0.00% |
| `avg_rating_180d` | 0 | 0.00% |
| `category_diversity_180d` | 0 | 0.00% |
| `ticket_count_90d` | 0 | 0.00% |
| `negative_ticket_rate_90d` | 0 | 0.00% |
| `avg_resolution_hours_90d` | 0 | 0.00% |
| `days_since_signup` | 0 | 0.00% |
| `sessions_30d` | 0 | 0.00% |
| `product_views_30d` | 0 | 0.00% |
| `cart_adds_30d` | 0 | 0.00% |
| `wishlist_adds_30d` | 0 | 0.00% |
| `abandoned_carts_30d` | 0 | 0.00% |
| `email_opens_30d` | 0 | 0.00% |
| `campaign_clicks_30d` | 0 | 0.00% |
| `last_visit_days_ago` | 0 | 0.00% |
| `churn_next_60d` | 0 | 0.00% |
| `split` | 0 | 0.00% |

---

## 3. Duplicate Records

Observed duplicate counts (computed from source CSVs):

| Dataset | Total rows | Exact duplicate rows | Duplicate primary-key rows | Other duplicate groups observed | Action |
|---------|-----------:|--------------------:|---------------------------:|--------------------------------:|--------|
| `data/customers.csv` | 2,400 | 0 | 0 (`customer_id`) | — | No action needed; `customer_id` unique |
| `data/orders.csv` | 10,009 | 0 | 0 (`order_id`) | 12 rows duplicate by (`customer_id`,`order_date`,`gross_amount`) | Investigate 12 grouped duplicates; deduplicate or reconcile with business (12/10,009 ≈ 0.12% > 0.1% threshold) |
| `data/support_tickets.csv` | 1,921 | 0 | 0 (`ticket_id`) | 0 by (`customer_id`,`issue_type`,`ticket_date`) | No action needed; `ticket_id` unique |
| `data/web_events_snapshot.csv` | 2,400 | 0 | 0 (`customer_id`) | — | No action needed; snapshot appears per-customer |

Recommended next steps:
- For the 12 `orders` grouped duplicates: export the matching rows and verify whether these are true duplicates (system replays) or legitimate separate orders with identical values; if true duplicates, drop duplicates keeping the earliest `order_id` and log changes.


---

## 4. Invalid and Unusual Values

| Dataset | Check | Observed (count) | Notes / Action |
|---------|-------|-----------------:|----------------|
| `data/orders.csv` | Negative `gross_amount` | 0 | No negative order amounts observed. |
| `data/orders.csv` | Zero `gross_amount` | 0 | No zero-value orders. |
| `data/orders.csv` | `gross_amount` > 10× median | 7 | Median = 597.06; 7 transactions > 10× median — investigate high-ticket orders, consider capping or log-transform. |
| `data/orders.csv` | `gross_amount` > 99th percentile | 101 | 99th pct ≈ 2,307.57; 101 rows exceed this — treat as outliers for revenue aggregation. |
| `data/customers.csv` | `signup_date` in future | 0 | No future signup dates. |
| `data/customers.csv` | `signup_date` < 2015-01-01 | 0 | No pre-2015 signup dates. |
| `data/support_tickets.csv` | Negative `resolution_hours` | 0 | No negative resolution hours. |
| `data/web_events_snapshot.csv` | `sessions_30d` > 99th pct | 18 | 99th pct = 18 sessions; 18 customers exceed this — review heavy users. |
| `data/web_events_snapshot.csv` | `product_views_30d` > 99th pct | 24 | 99th pct = 87 views; 24 customers exceed this — check for scraping or power users. |
| `data/web_events_snapshot.csv` | `cart_adds_30d` > 99th pct | 17 | 99th pct = 7; 17 exceed — inspect for bot/test behavior. |
| `data/web_events_snapshot.csv` | `wishlist_adds_30d` > 99th pct | 10 | 99th pct = 4; 10 exceed. |
| `data/web_events_snapshot.csv` | `campaign_clicks_30d` > 99th pct | 8 | 99th pct = 4; 8 exceed. |
| `data/web_events_snapshot.csv` | `last_visit_days_ago` > 99th pct | 0 | 99th pct = 60 days; none beyond this threshold were flagged as extreme here. |
| `data/intervention_history.csv` | Negative `last_campaign_cost` | 0 | No negative campaign costs. |
| `data/rfm_modeling_snapshot.csv` | `recency_days` > 99th pct | 24 | p99 = 346 days; 24 rows exceed. |
| `data/rfm_modeling_snapshot.csv` | `monetary_180d` > 99th pct | 24 | p99 ≈ 4,600.78; 24 rows exceed — treat as monetary outliers. |

Recommended actions:
- Treat the ~100 `orders` above the 99th percentile as outliers for revenue-related aggregations — either cap at p99, log-transform, or review individually.
- Review the small sets of high-activity web users (`sessions_30d`, `product_views_30d`, `cart_adds_30d`) for scraping/test accounts; consider filtering or separate modeling for power users.


---


## 5. Join and Key Issues

Join/key verification results (computed from CSVs):

| Join | Expected Cardinality | Observed | Notes / Action |
|------|---------------------|--------:|----------------|
| `customers` ↔ `churn_labels` | 1:1 | 2,400 ↔ 2,400 — no orphan labels | Good: every `churn_labels.customer_id` maps to a `customers.customer_id`. No action required. |
| `customers` ↔ `orders` | 1:N | All 2,400 customers have ≥1 order | No customers without orders were found. If business expects new users without orders, confirm the data extraction logic. |
| `orders` ↔ `support_tickets` | N:N (via customer) | All ticket `customer_id`s present in orders/customers | No tickets orphaned from orders; treat tickets as general customer interactions. |
| `customers` ↔ `intervention_history` | 1:N | All customers present in `intervention_history` (but many `last_campaign_received` values are NULL) | Action: do not assume campaign exposure when `last_campaign_received` is NULL — treat NULL as "no recent campaign" and surface rows with NULL `last_campaign_received` (507 rows) for business review. |
| `web_events_snapshot` ↔ `customers` | N:1 | All event rows map to existing `customer_id`s | No anonymous/bot events detected by missing `customer_id`. |

Recommended next steps:
- Add automated join-checks to `eda_audit.ipynb` that assert expected cardinalities (1:1, 1:N) and emit counts of orphaned keys; fail the notebook run if unexpected or write a review CSV with orphans.
- For `intervention_history`, export the 507 rows with NULL `last_campaign_received` for product/marketing to verify whether NULL means "no exposure" or a data extraction gap.


---

## 6. Date Consistency Issues

| Issue | Observed | Notes / Action |
|-------|--------:|----------------|
| **Snapshot date(s)** | `churn_labels.snapshot_date` = 2025-09-30 (single value found) | Use this as the canonical cutoff for feature computation and leakage checks. |
| **Orders after snapshot (leakage risk)** | 1,872 orders with `order_date > 2025-09-30` | High — these orders must be excluded from any feature aggregations computed at or before the snapshot. Filter `orders` to `order_date <= snapshot_date` before feature creation. Log and save excluded rows for audit. |
| **Support tickets after snapshot** | 0 tickets after snapshot | No action required for tickets with respect to snapshot leakage. Continue to verify in future runs. |
| **Campaigns after snapshot** | 0 `last_campaign_received` dates after snapshot (most are NULL) | NULL `last_campaign_received` should be treated as "no recent campaign"; surface NULL rows (507) for business clarification. |
| **Order date < signup date** | 0 orders where `order_date < signup_date` | No historical inconsistencies detected for order vs signup dates. |
| **Timezone indicators in timestamps** | 0 timestamp strings with timezone-like markers detected (e.g., `Z`, `+HH:MM`) | Timestamps appear to be date-only or timezone-less ISO strings. Still recommended to normalize to UTC if timezones are introduced. |

Recommended remediation:
- Strictly enforce `event_date <= snapshot_date` when aggregating features; implement this filter in `eda_audit.ipynb` and unit tests.
- Persist excluded post-snapshot rows into `outputs/excluded_post_snapshot/*.csv` for auditing and reproducibility.
- Clarify semantics of NULL `last_campaign_received` with the product/marketing team and treat NULL consistently in feature pipelines (explicit `no_campaign` flag).
- Add periodic checks that assert `orders_after_snapshot / total_orders` is near 0; alert if it grows unexpectedly.


---

## 7. Leakage Risk Columns

> These columns must **not** be used as model input features without careful temporal filtering. Below are the actual column names and findings from the dataset.

| Column (actual) | File | Leakage Risk | Observations / Notes |
|-----------------|------|-------------|---------------------|
| `churn_next_60d` | `data/churn_labels.csv` | **Target variable — never an input** | This is the target column (no NULLs observed). Do not include as a feature. |
| `cancel_date` (if present) | `data/customers.csv` | **Direct leakage** | No `cancel_date` column found in current `customers.csv`. If added, exclude it or derive only using dates ≤ snapshot. |
| `last_visit_days_ago` / `sessions_30d` | `data/web_events_snapshot.csv` / `data/rfm_modeling_snapshot.csv` | **Temporal leakage** | Values are snapshot-based; ensure any per-customer event aggregation uses `<= snapshot_date`. We found 18 `sessions_30d` values above p99 and 24 `product_views_30d` above p99 — handle as power-user outliers, not leakage. |
| `ticket_date` | `data/support_tickets.csv` | **Temporal leakage if > snapshot** | No support tickets were observed after snapshot in this run. Filter tickets to `ticket_date <= snapshot_date` when building features. |
| `last_campaign_received` | `data/intervention_history.csv` | **Temporal leakage if > snapshot** | Most values are NULL (507 NULLs). No campaign dates after snapshot were observed. Treat NULL as `no_recent_campaign` unless product clarifies otherwise. |
| Any feature derived from future `orders` | `data/orders.csv` (derived) | **Leakage** | 1,872 orders occur after the canonical snapshot (2025-09-30) — exclude these from training-time aggregations. Persist excluded rows for audit. |

Recommended safeguards:
- Always apply a global cutoff using `snapshot_date` (from `churn_labels.csv`) when computing features; enforce in `eda_audit.ipynb`.
- Add automated assertions in the notebook to fail if any candidate feature uses data with `event_date > snapshot_date`.
- Surface NULL/ambiguous fields (e.g., 507 `last_campaign_received` NULLs) to the business for semantic clarification; encode as explicit `no_campaign` where appropriate.


---

## 8. Class Imbalance

| Label | Count | % |
|-------|------:|---:|
| Churned (1) | 1,127 | 46.96% |
| Retained (0) | 1,273 | 53.04% |

**Impact:** Near-balanced classes (slightly more retained than churned). Still advisable to monitor performance across classes, but aggressive oversampling like SMOTE may not be necessary — prefer class-weighting, calibration, and careful threshold selection. Report per-class precision/recall and use AUC/PR as primary metrics.

---


## 9. Summary of Recommended Treatments

Priority actions and operational tasks :

- **Critical — Snapshot enforcement:** Filter every feature aggregation to `event_date <= 2025-09-30` (canonical `churn_labels.snapshot_date`). Persist excluded rows to `outputs/excluded_post_snapshot/` for audit. 

- **Critical — Target safety:** Ensure `churn_next_60d` (target) is never used as a feature. 

- **High — Deduplication:** Export suspect duplicate `orders` rows (the 12 grouped duplicates) to `outputs/suspect/orders_duplicates.csv`, review with business, then deduplicate (keep earliest `order_id`) in the pipeline. 

- **High — Outliers (revenue):** Export `orders` rows above the 99th percentile (~101 rows) to `outputs/outliers/orders_p99.csv`. For modeling, either cap monetary features at p99 or apply log-transform; document decision in the notebook and `README.md`.

- **High — Campaign semantics:** Export 507 `intervention_history` rows with NULL `last_campaign_received` to `outputs/suspect/intervention_null_last_campaign.csv` and confirm semantics with product/marketing (NULL = no exposure vs missing). Encode as explicit `no_campaign` if confirmed.

- **Medium — Missing categorical treatment:** `loyalty_tier` has 1,386 missing — encode as `None`/`Unknown` and add `has_loyalty` boolean. `skin_type` missing 401 rows — treat as `Unknown` or impute by cohort if required. Add these transformations to the feature engineering notebook.

- **Medium — Ratings and small missing values:** `orders.rating` missing 80 values — create `has_rating` flag; consider leaving rating missing as informative for modeling.

- **Medium — Numeric checks & monitoring:** Add numeric outlier checks (p99, >10x median) and export flagged rows to `outputs/outliers/`. Alert if the fraction of post-snapshot orders or extreme outliers increases substantially in future runs.

- **Low — Standardization & timezones:** Standardize categorical casing (`device_type`, `acquisition_channel`) and normalize timestamps to UTC if timezone info is introduced. Add a small lookup/cleaning cell in `eda_audit.ipynb`.

- **Operational — Tests & documentation:** Add automated assertions and summary exports to `eda_audit.ipynb`: missing-value summary, duplicate summary, join assertions, date-consistency checks, and outlier exports. Create `outputs/README.md` listing produced files and their purpose.



