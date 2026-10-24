# Phase 01 — Agreed Business Understanding

**Project:** Olist Retail Intelligence  
**Team:** Mahmoud & Ahmed  
**Phase:** 01 — Business Understanding  
**Status:** Consolidated team proposal — ready for peer review; KPI implementation rules require Data Audit validation  
**Basis:** Independent business proposals by Mahmoud and Ahmed. Ahmed delegated the choice of final recommendations; this document records the technical synthesis, not findings from analyzed Olist data.  
**Scope:** Historical Brazilian Olist marketplace dataset (approximately 2016–2018). We are modeling a hypothetical management decision, not describing Olist's present-day performance.

> **Guiding principle:** Questions and hypotheses are not findings. A relationship in historical data is not proof of causation, and a portfolio analysis does not by itself produce a real-world improvement.

## 1. Management problem statement

Marketplace management wants to understand which observable parts of the purchasing and delivery journey are associated with negative customer experiences. The team should produce credible, reproducible evidence to help stakeholders prioritize investigations and potential service improvements, rather than relying on a visually attractive dashboard or one headline statistic.

**Decision we want to improve:** Where should Operations, Customer Experience, and Marketplace Management investigate first to reduce the proportion of negatively reviewed orders while protecting overall customer experience?

## 2. Business objectives and priority

1. **Primary — Improve Customer Experience:** Reduce the share of 1–2-star ratings among reviewed orders and monitor the overall distribution of review scores. These are *feedback proxies*, not direct measures of satisfaction among all customers.
2. **Secondary — Improve Delivery Performance:** Investigate missed estimated delivery dates, delay severity, and variations across regions and sellers.
3. **Secondary — Support Sustainable Commercial Performance:** Identify service-improvement opportunities that could support retention and commercial performance in the future. Do not presume review improvement will automatically generate more orders, referrals, or profit.

**Decision rationale:** Negative Review Rate directly reflects the type of experience the team aims to reduce. Average Review Score must be monitored alongside it, because either measure alone can conceal a deteriorating part of the rating distribution.

## 3. Stakeholders and decisions supported

| Stakeholder | Decision supported | Boundaries |
|---|---|---|
| **Customer Experience / Support** | Prioritize problem categories and investigate proactive customer notifications or appropriate recovery policies. | This dataset cannot establish which support responses or compensation offers actually worked. |
| **Operations & Logistics** | Investigate late deliveries and regions/process stages where delivery performance is weak. | Do not attribute delays to a particular carrier, address issue, or operational bottleneck without corresponding evidence. |
| **Marketplace / Seller Management** | Review sellers whose *reviewed orders* show high negative counts and/or rates, with adequate sample sizes. | An order-level review does not independently prove a seller's fault, especially for multi-seller orders. |
| **Executive / Commercial Management** | Choose which proposed interventions merit resources and define evaluation measures. | The public data do not provide the full cost-and-benefit information needed to substantiate ROI or profit gains. |

## 4. Business questions (ranked)

| Rank | Business question | Why it matters / how it can inform a decision |
|---|---|---|
| **Q1** | **Are late deliveries, and longer delays, associated with more negative reviews or lower average review scores than on-time deliveries?** | Directly tests a plausible and actionable experience problem using order delivery dates and reviews. Analyze both a late/on-time flag and delay duration. |
| **Q2** | **Which sellers are associated with the largest counts and rates of negatively reviewed orders, accounting for review volume?** | Combines *impact* (number of negative experiences) and *risk* (rate) to prioritize seller investigations; restrict attribution appropriately. |
| **Q3** | **How do late-delivery rates and negative-review rates differ across customer states/regions?** | Identifies geographic concentrations that Operations and Customer Experience could investigate and helps contextualize seller results. |
| Q4 | Which product categories are associated with more negative reviews, considering category volume and multi-product orders? | Identifies categories for investigation; category association is not a direct product-quality measurement. |
| Q5 | Is the amount charged for shipping (`freight_value`, aggregated correctly to order level) associated with review outcomes, after considering other observable factors? | Explores Ahmed's shipping-cost idea without claiming freight charges represent carrier cost or profit. |

**Why Q1–Q3 first:** They are reasonably observable in Olist data, relevant to the central dissatisfaction objective, and can help identify practical owners for follow-up. Their final operational priority can change if Data Audit or EDA finds data gaps, small samples, or a much larger issue elsewhere.

**Exploratory / deferred questions:**

- **Observed repeat purchasing after a low rating (Mahmoud):** investigate only if `customer_unique_id` and follow-up windows support credible comparisons; right-censoring near the dataset end date is a major limitation.
- **Effectiveness of support responses and compensation (Mahmoud):** defer until support-ticket, response-time, resolution, and compensation records are available.
- **Price versus review scores within the same category (Mahmoud):** exploratory; consider single-product orders and account for seller, delivery, and category effects. Low price does **not** imply counterfeit goods.

## 5. KPI framework (agreed business definitions; validate technical assumptions in Data Audit)

**Time basis:** For the core monthly overview, group orders by `order_purchase_timestamp` month so that the KPI cohorts are aligned. Track observation completeness because reviews and deliveries can occur later than purchase, particularly close to the dataset's end. Report cohort size and missing/excluded counts; do not compare immature cohorts to fully observed cohorts without adjustment.

**Preferred unit:** One `order_id` for the main customer-experience measures. The team must audit duplicate review records before implementing a deterministic rule to select a **single valid review score per order**. Proposed starting rule: keep valid scores (integers 1–5), use the latest available `review_answer_timestamp`, break ties with `review_creation_date` and then a stable `review_id`; inspect examples and run a sensitivity check before approval. Do not silently double-count orders with multiple review rows. The raw *review event* count should remain available separately.

| KPI | Business definition and formula | Unit / denominator | Filters, interpretation and decision |
|---|---|---|---|
| **Primary — Negative Review Rate (NRR)** | `100 × (reviewed orders with canonical score in {1,2}) / (reviewed orders with canonical valid score in {1,2,3,4,5})` | **Reviewed order**; denominator is **not all orders**. | Include any purchase-cohort order with a valid canonical review, regardless of delivery status; show denominator and review coverage. Tracks unfavorable submitted feedback, not dissatisfaction among every customer. **Lower is better.** |
| **Supporting 1 — Average Review Score (ARS)** | `sum(canonical valid order review scores) / number of reviewed orders with a valid canonical score` | **Reviewed order**; same review cohort as NRR. | Report mean on 1–5 scale, preferably alongside a rating-distribution chart. Captures movements missed by a binary negative threshold. **Higher is better**, interpreted jointly with NRR. |
| **Supporting 2 — On-Time Delivery Rate (OTDR)** | `100 × (eligible delivered orders with calendar date(actual delivery) ≤ calendar date(estimated delivery)) / eligible delivered orders` | **Delivered order** with both dates; one order counted once. | Require delivered status and both valid dates; exclude canceled, not-delivered, and missing-date orders from this denominator, but report counts. Compare **calendar dates** because estimated dates may be recorded at midnight. **Higher is better.** |
| **Supporting 3 — Seller Negative Review Rate** | For each seller: `100 × (negatively reviewed eligible seller-orders) / (reviewed eligible seller-orders)` | **Seller × order**, deduplicated per seller. | Initially restrict the comparison to **single-seller orders** with a valid canonical review, flag this population explicitly, and analyze multi-seller orders separately. Show both negative-review count and denominator; set a minimum sample-size rule after inspecting seller volume. It is an investigation signal, **not seller-fault proof**. |

**Additional diagnostic metrics (not extra headline KPIs):**

- **Late Delivery Rate** = `100 − OTDR` for the *same eligible delivered-order population*.
- **Average Delay Days (late orders)** = average of positive calendar-day differences `actual delivery date − estimated delivery date` among eligible late orders; report the number of such orders.
- **Negative Review Rate Gap** = `NRR(late delivered, reviewed orders) − NRR(on-time delivered, reviewed orders)` in **percentage points**; report both denominators, and never describe the difference as causal evidence.
- **Review Coverage Rate** = `100 × (purchase-cohort orders with a valid canonical review) / (purchase-cohort orders)`; show whether lower NRR coincides with changes in the fraction providing reviews. Stratify by order status when useful.
- **Seller negative-review counts** and **category/region sample sizes** are necessary context when making comparisons.

**Metrics interpretation rule:** No metric is allowed to declare success on its own. A fall in NRR with declining review coverage, a worse ARS, or a major change in the order mix requires investigation. A rising OTDR can also be misleading if promised delivery dates are simply extended; inspect actual delivery duration and customer outcomes.

## 6. Business hypotheses (not tested)

- **H1 — Delivery:** Among comparable reviewed orders, late delivery is associated with a higher NRR and/or lower ARS than on-time delivery. Investigate delay severity as well. **Unverified; association is not causation.**
- **H2 — Geography:** Review and delivery outcomes vary across customer regions, even after basic differences in order volume and product mix are considered. **Unverified.**
- **H3 — Seller variation (secondary):** Some sellers are associated with higher NRR than peers with sufficient review volume. This does not imply fraud or seller responsibility. **Unverified.**
- **H4 — Category/price (exploratory):** Negative-review patterns vary by product category, and price may be associated with review outcomes *within* a category. Neither rating nor price verifies physical quality or counterfeiting. **Unverified.**

## 7. Candidate data sources and required fields

These are **expected** fields from the public Olist tables; availability, completeness, uniqueness, and relationships will be checked in the Data Audit.

| Table | Candidate fields | Main use |
|---|---|---|
| Orders | `order_id`, `customer_id`, `order_status`, `order_purchase_timestamp`, `order_delivered_customer_date`, `order_estimated_delivery_date` | Order grain, cohort, status and delivery outcome |
| Order reviews | `review_id`, `order_id`, `review_score`, `review_creation_date`, `review_answer_timestamp`, `review_comment_message` | Ratings, potential duplicate review events and optional complaint-text exploration |
| Order items | `order_id`, `order_item_id`, `seller_id`, `product_id`, `price`, `freight_value` | Seller linkage, products, shipping charge; **aggregate before joining** to avoid multiplying order metrics |
| Sellers | `seller_id`, `seller_state` | Seller grouping and geographic context |
| Products and category translation | `product_id`, `product_category_name`, category translation columns | Category comparisons |
| Customers | `customer_id`, `customer_unique_id`, `customer_state`, `customer_city` | Customer region; exploratory observed repeat purchases |

**Join and grain safety:** `orders` is expected to be one row per order, while `order_items` is commonly one row per order item. Never compute the total number of orders by counting rows after a one-to-many join. Use validated unique order IDs, audit review duplicates, and document any deduplication/aggregation.

## 8. Limitations and exclusions

1. A rating is **observed feedback** and may not represent all customers. Missing reviews, rating-selection effects, and timing can bias comparisons.
2. An order-level review may relate to the delivery experience, any of several products, or another issue. A low score cannot prove which product, category, or seller caused it.
3. Text comments (where available) are in Portuguese and may signal issues but do not independently verify counterfeit products, defects, or fraud. Verified inspections/return reasons would be needed.
4. Direct customer support, ticket-resolution, compensation, carrier-specific performance, and detailed logistics cause data are not available in the proposed core dataset. Recommendations in these areas remain proposals, not measured effects.
5. The dataset is historical and limited in time. No current-market claims, observed effect of unimplemented interventions, true referral behavior, or complete customer lifetime value can be inferred.
6. Commercial amounts such as item price and `freight_value` are not equivalent to net sales, costs, margin, or profit. Financial return on interventions cannot be credibly measured without additional cost and outcome data.
7. Seasonality, differences in review participation, order volume, seller mix, and product/region mix can bias direct month-to-month or before-and-after comparisons.
8. Delivery and review outcomes for purchases close to the dataset's end may not be fully observed. Document lag/censoring and exclusions; do not silently label undelivered orders 'on time' or 'late'.

## 9. Success criteria

### Success for this historical portfolio project

- **Reproducibility:** Build and validate four agreed headline KPIs at the declared grain; document filters, missingness, deduplication, cohort periods, and limitations in SQL/Python notebooks and reporting.
- **Decision usefulness:** Answer the top three questions (or explicitly explain insufficient evidence), showing effect sizes, rates, counts, sample sizes, and uncertainty wherever practical.
- **Actionability:** Produce a concise report/dashboard with at least two evidence-linked investigation or intervention proposals, each with an identified stakeholder, monitoring metric, and implementation caveat.
- **Honesty:** Separate observed findings from hypotheses, potential causes, and suggested actions. Do not claim commercial growth, service improvement, or ROI that has not been observed.

### Success for a future intervention (outside historical dataset scope)

Establish real baselines first; then set a realistic target and measurement window for NRR, ARS and delivery performance. Track reviewed-order counts and review coverage, examine comparable customer/order groups, and where feasible use a control/comparison group. Evaluate implementation costs and benefits separately. **No numeric improvement target is approved before the actual baseline is audited.**

## 10. Agreed scope, unresolved data-audit checks and review record

**Business decisions selected in this joint synthesis:** customer experience first; NRR primary; ARS, OTDR, and seller NRR supporting; prioritize delivery → seller → geography; keep category and freight analyses within scope. These are decisions on **what to measure and investigate**, not claims about what the dataset will show.

**Technical checks for Phase 02 — Data Audit:**

1. Audit `order_id` and `review_id` uniqueness, exact duplicates, repeated `order_id` in reviews, and whether the proposed canonical-review selection changes KPI results.
2. Measure missing/invalid ratings, dates, status, product category, `seller_id` and region; establish a consistent cohort completion policy.
3. Verify delivered order status against actual delivery timestamps, estimated delivery timestamp semantics and date parsing.
4. Count single- versus multi-seller and single- versus multi-product orders; report representativeness of restricted seller/category samples.
5. Inspect seller review-volume distribution and approve minimum sample-size and uncertainty-reporting rules *before* ranking sellers.
6. Test whether category/region comparisons are meaningfully supported given volume and missingness. Check for join multiplication.

**Ownership and sign-off:**

- **Mahmoud:** independent proposal supplied; participated in KPI/prioritization review.
- **Ahmed:** independent proposal supplied; delegated final recommendation to ChatGPT for technical synthesis (no personal approval or GitHub review asserted).
- **Reviewer/author of synthesis:** ChatGPT (technical recommendation based on the two proposals; not a substitute for a GitHub reviewer approval).
- **GitHub merge:** pending branch commit, human PR review and merge. Keep the individual proposal files unchanged as provenance.

**Next step:** Complete a brief PR review for this file, merge through GitHub after the team's required approvals, then begin the Olist Data Understanding & Data Audit phase in Google Colab. Revisit implementation choices above using real audit findings without retroactively rewriting hypotheses as predictions.
