# Business Memo: Pre-Campaign Data Findings

**To:** Product, Marketing & Customer Support Leadership  
**From:** Sanjiv 
**Subject:** What the Data Tells Us Before We Launch Any Retention Campaign  
**Classification:** Internal

---

## Executive Summary

Before we spend a single rupee on discounts or retention campaigns, this memo summarizes five critical findings from the data audit. Not all at-risk customers are the same — treating them uniformly will waste budget and may accelerate churn for some segments. Here is what we found, what it means, and what we should do first.

---

## Finding 1: Recency is the Most Powerful Early Warning Signal

**What we found:**  
Using `churn_labels.csv` (snapshot 2025-09-30) and `orders.csv`, customers with their last order >60 days before snapshot have a churn rate of **100%**, while customers with last order ≤60 days have a churn rate of **14.51%** (ratio ≈ **6.9×**). By finer buckets, churn rates are: ≤30 days = **6.05%**, 31–45 days = **100%**, 46–60 days = **100%**, 61–90 days = **100%**, >90 days = **100%**. These numbers come directly from the notebook's join of `orders` and `churn_labels`.

**What it means:**  
Churn risk escalates sharply once customers move out of the recent-order window; in this dataset elevated risk is already observable by the 31–45 day bucket and is near-universal beyond 45–60 days.

**What we should do:**  
Implement an automated trigger to flag customers whose days-since-last-order exceeds 30 days for monitoring and for early non-discount outreach; prioritize customers who also meet other risk signals (e.g., low session counts).

---

## Finding 2: Unresolved Support Tickets — evidence limited in current data

**What we found:**  
The notebook's ticket-level check (using `support_tickets.csv`) found **0** customers with 2+ unresolved tickets at snapshot (resolution-hours is populated). Therefore we cannot confirm the earlier claim from these files alone.

**What it means:**  
The current CSV fields do not show a large population of unresolved tickets at snapshot; to evaluate ticket-driven churn we need richer status/history (ticket state at snapshot, categorical tags, and resolution timestamps). The dataset does not provide clear evidence that 2+ unresolved tickets is a widespread signal here.

**What we should do:**  
If operationally relevant, export full ticket histories with timestamps and resolution states and re-run the analysis. In the meantime, treat support-ticket severity features as potentially important but not yet validated by these files.

---

## Finding 3: Discount-Dependency in the data

**What we found:**  
Contrary to the earlier estimate, the data show that among customers with orders, **2,360 of 2,400 (≈98.3%)** have a historical discount-usage rate >70% (i.e., most of their orders used a discount). Churn rates are similar: high-discount customers churn ≈ **46.99%** vs **45.0%** for others in this dataset.

**What it means:**  
Discount usage is extremely common in these records and by itself is not a unique discriminator of churn — high discount users and others have comparable churn rates here. We should not assume discount-dependency uniformly implies short-term responsiveness to price incentives without further segmentation.

**What we should do:**  
Refine the analysis to identify subsegments of discount users (e.g., high-frequency vs low-frequency, recency, product mix) before excluding or targeting them in campaigns. Run A/B tests that compare value-led messaging to targeted discounts for small pilot groups.

---

## Finding 4: Low Engagement (sessions_30d) correlates with higher churn

**What we found:**  
Using `web_events_snapshot.csv`, churn rates by session bucket are: 0 sessions = **66.32%**, 1 session ≈ **66.67%**, 2–3 sessions ≈ **57.56%**, 4+ sessions = **36.11%**. Higher session counts are associated with materially lower churn.

**What it means:**  
Engagement metrics are predictive signals in this dataset; customers with 4+ sessions in 30 days show substantially lower churn and are reasonable to classify as "engaged." These metrics should be included in modeling and live scoring.

**What we should do:**  
Add `sessions_30d` and `product_views_30d` as core features in the feature store and surface a daily engagement health score. Use 4+ sessions as a conservative active threshold for initial targeting.

---

## Finding 5: First-order customers without a second purchase within 30 days are higher-risk

**What we found:**  
From `orders.csv` joined to `churn_labels.csv`: there are **2,400** customers with at least one order; **1,738** of those did not place a second order within 30 days of their first. The churn rate among customers who do not convert to a second order within 30 days is **48.62%**.

**What it means:**  
Failure to convert to a second order within 30 days is a common and material early predictor of churn in this dataset — it affects a large fraction of new customers and is associated with roughly a 49% churn rate.

**What we should do:**  
Prioritize a lightweight "second order" nurture flow for new customers between day 10 and day 25 post-first-order. Track uplift via an A/B test before scaling.

---

## What We Should NOT Do Before Further Investigation

1. **Do not run a blanket discount campaign** — discount-dependent customers will consume budget without improving long-term retention.
2. **Do not treat all churned customers the same** — voluntary churn (stopped buying) vs. complaint-driven churn require different responses.
3. **Do not use the model score as the only input to campaign decisions** — high model risk scores in the discount-dependent segment may be correctly predicting churn that discounts cannot fix.
4. **Do not ignore unresolved support tickets** — a campaign sent to a customer with an open complaint will likely backfire.

---

## Recommended Next Steps (Priority Order)

| Priority | Action | Owner | Timeline |
|----------|--------|-------|----------|
| 1 | Resolve open product-quality tickets within 5 days for flagged high-risk customers | Customer Support | Immediate |
| 2 | Build 45-day inactivity trigger for early re-engagement (no discount) | CRM / Marketing | 2 weeks |
| 3 | Identify second-order non-converters and launch nurture sequence | Marketing | 2 weeks |
| 4 | Separate discount-dependent customers from discount-eligible campaign lists | Marketing Ops | 1 week |
| 5 | Deploy app engagement health score as an early-warning dashboard | Product | 3 weeks |

---

