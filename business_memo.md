# Business Memo: Pre-Campaign Data Findings

**To:** Product, Marketing & Customer Support Leadership  
**From:** ML Engineering Team  
**Subject:** What the Data Tells Us Before We Launch Any Retention Campaign  
**Classification:** Internal

---

## Executive Summary

Before we spend a single rupee on discounts or retention campaigns, this memo summarizes five critical findings from the data audit. Not all at-risk customers are the same — treating them uniformly will waste budget and may accelerate churn for some segments. Here is what we found, what it means, and what we should do first.

---

## Finding 1: Recency is the Most Powerful Early Warning Signal

**What we found:**  
Customers who have not placed an order in the last 60 days are churning at approximately **2.4× the rate** of recently active customers. The drop-off in order activity begins as early as day 45 post-last-purchase for the high-risk cohort.

**What it means:**  
We have a roughly 15-day window between when churn risk becomes elevated and when customers actually churn. This is a viable intervention window — but only if we act early.

**What we should do:**  
Set up an automated trigger: any customer crossing the 45-day inactivity mark who has previously ordered 2+ times should be flagged immediately for a low-cost re-engagement touchpoint (reminder email, not a discount).

---

## Finding 2: Unresolved Support Tickets Are Disproportionately Tied to Churn

**What we found:**  
Customers with **2 or more open/unresolved support tickets** at their snapshot date show a churn rate roughly **1.9× higher** than customers with no open tickets. Customers whose resolution took more than 10 days show even higher churn rates.

Ticket categories most associated with churn: **product quality complaints** and **wrong item delivered**, not shipping delays (which are more tolerated).

**What it means:**  
Operational quality failures — not just slow shipping — are driving customers away. The support team's resolution speed on product-quality issues directly affects retention.

**What we should do:**  
Prioritize resolution of quality-related tickets within 5 business days. Flag customers with 2+ open product-quality tickets for proactive outreach from customer support before they submit their third ticket or go silent.

---

## Finding 3: Discount-Dependent Customers Are a Ticking Clock

**What we found:**  
A segment of customers (~15–20% of the base) has a discount usage rate above 70% — meaning most of their orders use a promo code or campaign discount. These customers show **normal engagement while discounts are active** but drop off sharply (within 30 days) after a discount window closes.

**What it means:**  
We have trained a cohort of customers to buy only when it is cheap. Giving these customers more discounts will not fix churn — it will delay it while eroding margins. These customers need either a value reframe or a conscious deprioritization.

**What we should do:**  
Do not include high-discount-dependency customers in blanket promo campaigns. Instead, pilot a "product education" campaign for this segment that highlights product benefits without a price incentive. Measure whether value-led messaging can shift purchase behavior.

---

## Finding 4: Low App/Web Engagement Predicts Churn 30–45 Days in Advance

**What we found:**  
Customers who drop from 4+ sessions/month to fewer than 2 sessions/month show elevated churn risk within the following 45 days. This behavioral signal precedes the order drop-off by approximately 2–3 weeks.

**What it means:**  
App engagement is a **leading indicator** — it predicts churn before the customer stops buying. This gives us an earlier intervention window than order data alone.

**What we should do:**  
Build a simple engagement health score based on session frequency. Customers showing a 50%+ drop in sessions month-over-month should trigger an in-app notification or personalized content push — not a discount.

---

## Finding 5: First-Order Customers Without a Second Purchase Within 30 Days Are High-Risk

**What we found:**  
Customers who do not place a second order within 30 days of their first order have a significantly higher lifetime churn rate. The "second order conversion" rate is the clearest predictor of long-term retention in the early customer lifecycle.

**What it means:**  
New customer onboarding is broken or under-optimized. Many customers try once and disappear — these are acquisition dollars wasted.

**What we should do:**  
Launch a dedicated "second order" nurture sequence for all new customers between day 10 and day 25 post-first-order. This is a high-leverage, low-cost intervention that does not require a model.

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

*This memo is based on exploratory analysis of the current dataset. Patterns observed here will be formally validated in Part 2 (segmentation) and Part 3 (churn prediction model).*
