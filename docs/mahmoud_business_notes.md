# Business Understanding — Mahmoud

**Project:** Olist Retail Intelligence  
**Author:** Mahmoud  
**Phase:** 01 — Business Understanding  
**Status:** Individual proposal — awaiting Ahmed's independent review  
**Data context:** Historical Olist Brazilian e-commerce data (2016–2018); no analysis has been run for this document.

> **Working principle:** Business questions and hypotheses are not findings. Any suggested improvement or relationship must be validated against the data before the team recommends an intervention.

## A. Business objectives (three proposals)

1. **Improve customer satisfaction (primary objective).** Reduce negative customer experiences and strengthen trust in the marketplace. An average rating alone may hide a substantial group of dissatisfied customers; the team should track the *negative review rate* directly.
2. **Improve delivery performance.** Reduce late deliveries and improve compliance with estimated delivery dates. Delivery issues may be an important contributor to dissatisfaction, but they are not assumed to be its only cause.
3. **Support sustainable commercial performance.** Investigate how customer experience relates to repeat purchasing, order volume, and future sales opportunities. Better reviews do **not** automatically imply increased sales; any such connection requires evidence.

**My chosen objective and justification:**  
Customer satisfaction is my first priority. I would focus on reducing the share of 1- and 2-star reviews instead of optimizing the overall average score alone. I expect improvements in delivery and seller experience to help, while recognizing that product quality, expectations, and service recovery can also matter. Long-term growth is a potential business benefit, not a proven outcome.

## B. Stakeholders and decisions

| Stakeholder | Why they need the analysis | Decisions supported |
|---|---|---|
| **Operations & Logistics** | Understand delivery delays and where they are concentrated. | Investigate bottlenecks (seller dispatch, hand-off, routes, or other causes); prioritize high-impact areas; evaluate delivery process changes. Do not blame a carrier without carrier-specific evidence. |
| **Customer Experience / Support** | Understand which customers report negative experiences and whether delays are associated with those experiences. | Define proactive delay notifications, support follow-up, and targeted compensation policies within cost and approval limits. Do not promise zero future delays. |
| **Marketplace / Seller Management** (secondary) | Identify sellers whose orders are frequently associated with dissatisfaction. | Review sellers with sufficient evidence; investigate and improve processes before making punitive decisions. |
| **Executive Management** (secondary) | Decide where limited resources will create the most measurable value. | Approve priorities, budgets, and criteria for evaluating proposed actions. |

**How I would approach a delivery problem:** First investigate root causes rather than assume one. Examples to examine *if appropriate data exists* include address problems, seller processing time, and logistics capacity. Prioritize causes by the number of orders affected, severity, and intervention cost; provide proactive communication to affected customers and use compensation selectively.

## C. Business questions — five proposals, ranked

1. **Q1 — Delivery performance (priority 1):** Is late delivery associated with a higher negative review rate than on-time delivery? How large is the difference?
2. **Q3 — Seller performance (priority 2):** Which sellers are associated with the highest *counts* and *rates* of negative reviews, after accounting for review volume and orders containing multiple sellers?
3. **Q2 — Product categories (priority 3):** Which product categories are associated with higher negative review rates, accounting for category volume and multi-product orders?
4. **Q4 — Geography:** How do late-delivery rates and negative review rates vary by customer region?
5. **Q5 — Repeat purchasing:** Are customers associated with negative reviews less likely to make another observed purchase within a fixed follow-up window than customers without negative reviews?

**Why are Q1, Q3, and Q2 prioritized?**  
Q1 directly connects to a process Operations can investigate and uses delivery timestamps and review scores available in the dataset. Q3 can help prioritize seller reviews by *total customer impact*, not only the highest percentage: 150 negative experiences are more numerous than 8 even when the first seller's negative review rate is lower. Q2 can reveal category-level patterns after seller-level analysis. These are **initial priorities** and may change if exploratory analysis shows another area has greater impact or cleaner evidence.

**Deferred question — customer support effectiveness:** Does timely, effective complaint resolution reduce negative review rates? This is commercially important, but the present Olist tables do not contain direct support-ticket, resolution, or compensation records. Revisit it if such data becomes available.

## D. KPI definitions (proposed; pending data audit)

| KPI | Exact definition / formula | Unit / grain | Filters, period, and data-quality rules | Why it matters |
|---|---|---|---|---|
| **Primary — Negative Review Rate (NRR)** | `100 × (# valid reviews scored 1–2) / (# valid reviews scored 1–5)` | **Review** (unique review event; validate review identifiers and duplicate behavior first) | Report monthly by **review creation month**. Exclude missing/out-of-range scores and investigate duplicate review records. Show review count and coverage; a review is not necessarily a unique customer. | Tracks the share of submitted reviews that are negative. |
| **Supporting 1 — On-Time Delivery Rate** | `100 × (# eligible delivered orders with actual delivery date ≤ estimated delivery date) / (# eligible delivered orders)` | **Order** (unique `order_id`) | Report by **actual delivery month**. Include only delivered orders with both actual and estimated delivery dates. Compare **calendar dates**, not midnight timestamps; exclude canceled, non-delivered, and date-incomplete orders from this denominator, and report excluded counts. | Measures compliance with promised delivery dates. |
| **Supporting 2 — Negative Review Rate Gap by Delivery Status** | `NRR among late delivered orders − NRR among on-time delivered orders`, in **percentage points** | **Delivered order** with eligible review(s); review-to-order mapping to be validated | Use orders with valid score and both delivery dates, grouped by actual delivery month. Establish a consistent one-review-per-order rule during the audit if multiple reviews exist. Report each group's numerator/denominator. | Checks whether negative feedback is more common with late deliveries; it is **association, not causation**. |
| **Supporting 3 — Seller Negative Review Rate** | `100 × (# reviewed orders linked to the seller with negative feedback) / (# reviewed orders linked to the seller)` | **Seller × reviewed order**, deduplicated within seller | Group by review month; **for initial attribution use single-seller orders**, with multi-seller orders analyzed separately. Show both negative count and reviewed-order count. Set a minimum sample threshold after inspecting seller volumes; do not rank tiny samples as definitive. | Identifies sellers worth investigating without relying on counts or percentages alone. |

**Important reporting choices:** Use consistent comparison windows and explain any differences between a review-month KPI and a delivery-month KPI. Monitor the review coverage rate (reviewed orders / eligible orders) as a quality/context measure; fewer negative reviews do not necessarily indicate happier customers if dissatisfied customers stop leaving reviews. Estimated delivery targets could also be made artificially lenient, so review actual delivery duration alongside on-time performance during the analysis.

**No numerical baselines or improvement claims have been calculated yet.** Any percentages used during our mentoring discussion were hypothetical exercises, not Olist findings.

## E. Business hypotheses — NOT YET TESTED

- **H1 — Delivery:** Orders delivered after their estimated delivery date are associated with a higher negative review rate than orders delivered on or before that date. **Unverified; do not interpret association as proof of causation.**
- **H2 — Price within product category:** Lower-priced products may be associated with higher negative review rates than higher-priced products **within the same category**. **Unverified; lower price does not prove low quality or counterfeit products.** Start with carefully selected single-product orders where interpretation is clearer; account for seller and delivery differences where possible.

## F. Data needs, uncertainty, and limitations

**Candidate Olist data sources (column availability and linkages must be confirmed in the next phase):**

- Orders: `order_id`, `order_status`, `order_purchase_timestamp`, `order_delivered_customer_date`, `order_estimated_delivery_date`.
- Reviews: `review_id`, `order_id`, `review_score`, `review_creation_date`, `review_comment_message` (where present).
- Order items and sellers: `order_id`, `order_item_id`, `seller_id`, `product_id`, `price`, shipping-related timestamps as available.
- Products and category translation: product category identifiers / names.
- Customers: `customer_id`, `customer_unique_id`, state/city (useful for region-level analysis and observed repeat orders).

**Limits and caveats:**

1. Star ratings measure submitted feedback, **not** satisfaction for all customers. Reviews may be missing, and review text may be incomplete or biased.
2. Comments may report damaged, wrong, or allegedly counterfeit goods, but **do not independently establish** a defect or counterfeit status. Verified inspection, return reasons, and confirmed complaint records would be needed.
3. Olist review scores are linked to **orders**; an order may contain multiple products or sellers. A bad review should not automatically be assigned as blame to any single item or seller.
4. The available public dataset lacks direct customer-support tickets, resolution timestamps, compensation records, verified product inspections, and carrier-level operational causes needed to test some proposed solutions.
5. Historical data cannot measure the real-world impact of interventions that the team has **not actually deployed**. Likewise, it does not establish current business performance.
6. Observed repurchasing is limited by the dataset end date: customers who buy later may appear not to return. Referral behavior and full customer lifetime value are not directly measured.
7. Revenue, freight charges, costs, discounts, refunds, and profit must not be treated as interchangeable. Net profit or ROI cannot be calculated credibly without appropriate cost and outcome data.
8. Seasonal demand, changing order mix, geography, and review participation could distort simple before-and-after comparisons.

## G. What a useful business deliverable looks like

**Success of this portfolio analysis (within the historical data):**

1. Publish documented, validated KPI calculations at the correct grain with explicit denominators, exclusions, and missing-data checks.
2. Identify and quantify patterns in delivery, seller, and category performance; distinguish evidence from hypotheses and clearly state uncertainty.
3. Deliver a decision-ready dashboard or report showing the magnitude of issues, affected groups, and actionable next investigations for Operations and Customer Experience.
4. Propose interventions with an owner, a testable expectation, implementation cost considerations, and a monitoring plan.

**Success of a future real-world intervention (not something already demonstrated by this dataset):**

- Agree a realistic baseline, target, and time horizon *after* measuring actual performance.
- Compare Negative Review Rate and On-Time Delivery Rate before and after, alongside reviewed-order volume, review coverage, and customer/order mix.
- Where feasible, use a comparison/control group or another credible evaluation design to separate intervention effects from seasonal or unrelated changes.
- Assess costs against measurable benefits; avoid claiming that improved metrics alone prove financial return.

*Illustration only:* Moving NRR from 15% to 10% and on-time delivery from 85% to 92% would be encouraging, **if achieved and credibly measured**. These numbers are examples, not measured Olist results or approved targets.

## H. Open questions for Ahmed / tech-lead review

1. Should the primary KPI count unique review events or unique reviewed orders if one order can have more than one review? Confirm the data grain during Data Audit.
2. Should the primary NRR cover all valid review scores or only delivered orders? Which definition best matches our business objective while preserving comparability?
3. What is an appropriate minimum denominator for comparing sellers, and how should multi-seller orders be handled in secondary analyses?
4. Can we separate pricing effects from delivery and seller effects sufficiently to test H2, or should H2 remain exploratory?
5. What realistic success thresholds and intervention costs should be proposed once we establish the actual baseline?
6. What additional data would be required to study customer support effectiveness and verified quality issues?

---

**Next step:** Ahmed prepares an independent proposal. Compare both proposals through pull requests, then record only the **jointly approved** decisions in `docs/01_business_understanding.md`. Do not fill in Ahmed's file or the agreed brief on his behalf.
