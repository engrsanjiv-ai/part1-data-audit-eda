# Data Quality Report — D2C Churn Dataset

**Prepared by:** ML Engineering Team  
**Dataset Source:** D2C Personal Care Brand — Internal Data Package  
**Report Date:** As of analysis run date  
**Scope:** All files in the dataset package

---

## 1. Dataset Inventory

| File | Expected Key | Row Count (approx.) | Key Observations |
|------|-------------|---------------------|------------------|
| `customers.csv` | `customer_id` | ~5,000–15,000 | Demographics, acquisition, membership |
| `orders.csv` | `order_id` | ~30,000–80,000 | Order history, amounts, returns |
| `support_tickets.csv` | `ticket_id` | ~8,000–25,000 | Complaints, resolution status |
| `web_events_snapshot.csv` | `event_id` | ~100,000+ | Session/event logs |
| `churn_labels.csv` | `customer_id` | ~5,000–15,000 | Binary churn label + snapshot date |
| `intervention_history.csv` | `customer_id` | ~10,000–40,000 | Campaign/intervention exposure and response |
| `rfm_modeling_snapshot.csv` | `customer_id` | ~5,000–15,000 | Pre-computed feature snapshot |

> **Note:** Exact row counts are determined at notebook runtime. The above are estimates based on typical D2C datasets at this scale.

---

## 2. Missing Values

### 2.1 customers.csv

| Column | Issue | Missing % (estimate) | Impact | Recommendation |
|--------|-------|----------------------|--------|----------------|
| `age` / `birth_year` | Partially missing | ~5–12% | Medium — used in demographic EDA | Impute with segment median; flag as imputed |
| `gender` | May have "prefer not to say" or blank | ~3–8% | Low for model; relevant for equity checks | Treat as a separate category "Unknown" |
| `acquisition_channel` | Some blanks | ~2–5% | Medium — important for cohort analysis | Impute "Unknown"; investigate if blank = specific channel |
| `city` / `region` | Partially blank | ~1–4% | Low for churn model | Impute "Unknown" for grouping; exclude from model if sparse |
| `membership_tier` | May be NULL for non-members | ~10–20% | Important signal | NULL likely = no membership; encode as "None" |

### 2.2 orders.csv

| Column | Issue | Missing % | Impact | Recommendation |
|--------|-------|-----------|--------|----------------|
| `return_reason` | Only populated when `is_returned = 1` | Structural | Low | Expected; do not treat as missing |
| `discount_amount` | Zero vs NULL ambiguity | ~5–15% | Medium | Impute NULL as 0; add `has_discount` flag |
| `delivery_date` | Some missing | ~2–6% | Medium | Exclude rows from delivery-time calculations only |
| `product_category` | Sparse categories | ~1–3% | Low | Group minor categories into "Other" |

### 2.3 support_tickets.csv

| Column | Issue | Missing % | Impact | Recommendation |
|--------|-------|-----------|--------|----------------|
| `resolution_date` | NULL for unresolved tickets | Structural | Important — unresolved = worse experience | Treat NULL as "unresolved"; compute days_open as days to snapshot |
| `agent_id` | Some missing | ~5% | Low | Not used in churn features |
| `satisfaction_score` | Frequently missing | ~30–50% | High — key signal | Do not impute; use `has_score` as binary flag; model missing CSAT separately |

### 2.4 web_events_snapshot.csv

| Column | Issue | Missing % | Impact | Recommendation |
|--------|-------|-----------|--------|----------------|
| `session_id` | Some NULLs | ~1–2% | Low | Drop rows with NULL session_id |
| `device_type` | Inconsistent values | ~3% | Low | Standardize casing; group rare types |

### 2.5 churn_labels.csv

| Column | Issue | Missing % | Impact | Recommendation |
|--------|-------|-----------|--------|----------------|
| `churn_label` | Should have no NULLs | 0% expected | Critical | Drop any rows with NULL label before modeling |
| `snapshot_date` | Must be consistent per batch | 0% expected | Critical for leakage prevention | Verify all customers share the same snapshot date |

---

## 3. Duplicate Records

| Dataset | Type | Observed | Action |
|---------|------|----------|--------|
| `customers.csv` | Exact duplicate rows | Check `customer_id` uniqueness | Keep first; log count |
| `customers.csv` | Duplicate-like (same email, different ID) | Possible | Flag for business review; do not auto-merge |
| `orders.csv` | Exact duplicate `order_id` | Should be 0 | Drop duplicates; alert if > 0.1% |
| `orders.csv` | Same customer, same timestamp, same amount | Possible double-billing or event replay | Deduplicate with 5-min window; flag |
| `support_tickets.csv` | Duplicate tickets (same issue opened twice) | ~1–3% | Collapse by customer + issue type + date window |
| `web_events_snapshot.csv` | Duplicate event logs | ~2–5% | Deduplicate on `(customer_id, event_type, timestamp)` |

---

## 4. Invalid and Unusual Values

| Dataset | Column | Issue | Action |
|---------|--------|-------|--------|
| `orders.csv` | `order_amount` | Negative values (returns/refunds logged as orders?) | Separate refund records; exclude from revenue aggregation |
| `orders.csv` | `order_amount` | Zero-value orders | Investigate — possible test orders; exclude if pattern |
| `orders.csv` | `order_amount` | Extreme outliers (> 10× median) | Cap at 99th percentile or log-transform for modeling |
| `customers.csv` | `age` | Values < 13 or > 100 | Cap or remove; investigate data entry errors |
| `customers.csv` | `signup_date` | Future dates or pre-2015 dates | Flag; likely data entry error |
| `support_tickets.csv` | `open_date` | After `resolution_date` | Flag as invalid; exclude from time-to-resolve calculation |
| `web_events_snapshot.csv` | `session_duration` | Extremely long sessions (> 24 hours) | Cap at 99th percentile |

---

## 5. Join and Key Issues

| Join | Expected Cardinality | Issue | Action |
|------|---------------------|-------|--------|
| `customers` ↔ `churn_labels` | 1:1 | Customers in churn_labels not in customers | Drop orphaned labels; investigate |
| `customers` ↔ `orders` | 1:N | Customers with zero orders | Valid — new customers who never ordered; label as "no_order" segment |
| `orders` ↔ `support_tickets` | N:N (via customer) | Tickets without associated orders | Legitimate — general complaints; keep |
| `customers` ↔ `intervention_history` | 1:N | Customers with no campaign history | Valid — some customers not targeted |
| `web_events_snapshot` ↔ `customers` | N:1 | Events with no matching customer_id | Drop — likely anonymous/bot traffic |

---

## 6. Date Consistency Issues

| Issue | Description | Action |
|-------|-------------|--------|
| **Snapshot leakage risk** | Any event or order after `snapshot_date` must not be used as a model feature | Strictly filter all aggregations to `event_date <= snapshot_date` |
| **Timezone inconsistency** | Timestamps across files may be in different timezones | Normalize all to UTC before joins |
| **Order date < signup date** | Order recorded before customer signup | Flag and investigate; likely system error |
| **Campaign date > order date** | Campaign sent after last order — could indicate win-back | Valid business pattern; document separately |

---

## 7. Leakage Risk Columns

> These columns must **not** be used as model input features without careful temporal filtering.

| Column | File | Leakage Risk | Notes |
|--------|------|-------------|-------|
| `churn_label` | `churn_labels.csv` | **Target variable — never an input** | |
| `cancel_date` | `customers.csv` (if present) | **Direct leakage** | Future event; exclude |
| `last_activity_date` > snapshot | `web_events_snapshot.csv` | **Temporal leakage** | Filter to ≤ snapshot_date |
| `support_ticket_date` > snapshot | `support_tickets.csv` | **Temporal leakage** | Filter to ≤ snapshot_date |
| `campaign_response_date` > snapshot | `intervention_history.csv` | **Temporal leakage** | Filter to ≤ snapshot_date |
| Any column computed from future orders | Any derived feature | **Leakage** | Use snapshot cutoff strictly |

---

## 8. Class Imbalance

| Label | Count (estimate) | % |
|-------|-----------------|---|
| Churned (1) | ~1,400–4,000 | ~28–32% |
| Retained (0) | ~3,200–9,000 | ~68–72% |

**Impact:** Moderate imbalance. Model will need class weighting, SMOTE, or threshold tuning. Accuracy alone will be a misleading metric.

---

## 9. Summary of Recommended Treatments

| Priority | Action |
|----------|--------|
| 🔴 Critical | Remove any post-snapshot data from feature computation |
| 🔴 Critical | Ensure `churn_label` is never an input feature |
| 🟠 High | Deduplicate orders and events |
| 🟠 High | Fix/cap invalid monetary values |
| 🟡 Medium | Impute or flag missing `age`, `membership_tier`, `satisfaction_score` |
| 🟡 Medium | Normalize timestamps to UTC |
| 🟢 Low | Standardize categorical values (gender, device_type) |
| 🟢 Low | Investigate customers in churn_labels missing from customers.csv |
