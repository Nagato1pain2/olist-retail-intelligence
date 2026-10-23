# Business Understanding — Ahmed

**Author:** Ahmed  
**Phase:** 01  
**Status:** Draft — requires peer review

## A. Business objectives (2–3 proposals)
- Objective 1: Customer Experience 
- Objective 2: Delivery Performance
- Objective 3: Commercial Performance

**My chosen objective and justification:**

I choose Customer Experience as the initial priority. Customer experience may be related to several parts of the order journey, including delivery performance and the products customers receive.

The dataset documentation indicates that customer review scores and order delivery dates are available. This makes the objective a reasonable starting point, subject to validation during the Data Audit phase.

The analysis will investigate relationships between available data and customer review scores. It will not assume that any factor causes customer dissatisfaction before the evidence is reviewed.

## B. Stakeholders and decisions
Who is the stakeholder, and which decision should improve?

stakeholders:

Customer Experience Team  
Operations and Delivery Team  
Marketplace Management

Decision to improve:
Help the relevant teams identify which measurable aspects of the customer order experience deserve further investigation and targeted improvement.

## C. At least five business questions, ranked by priority
1.Is there a relationship between delivery delay duration and customer review scores?  
2.Do customer review scores vary across different customer states or regions?  
3.Do customer review scores differ between late-delivered and on-time-delivered orders?  
4.Do customer review scores vary across different product categories?  
5.Is shipping cost associated with customer review scores?

**Why are questions 1–3 the highest priority?**

## D. KPI definitions
| KPI | Exact definition/formula | Level (order/customer/item) | Filters / period | Why it matters |
|---|---|---|---|---|
| Primary KPI |Average Review Score |order |Orders purchased during the selected study period with valid review scores. Exclude missing or invalid scores. |Measures the overall level of customer ratings and supports comparisons across delivery performance, regions, and product categories. |
| Supporting 1 |Low Review Score Rate |order |

Orders purchased during the selected study period with valid review scores. Exclude orders without a valid score. |Measures the share of reviewed orders with low ratings and complements the average score. |
| Supporting 2 |Late Delivery Rate |order |Delivered orders purchased during the selected study period with valid actual and estimated delivery dates. Exclude canceled, undelivered, and missing-date orders. |Measures how common late delivery is and supports investigation of its relationship with customer ratings. |
| Supporting 3 |Average Delivery Delay |order 

Late-delivered orders purchased during the selected study period with valid actual and estimated delivery dates. |Measures the average severity of delivery delays and complements the late delivery rate. |

## E. Hypotheses — NOT YET TESTED
- H1:Orders delivered later than their estimated delivery dates may have lower average review scores than orders delivered on time.  
- H2:Average customer review scores may differ across product categories.

## F. Data needs, uncertainty and limitations

### Potential data sources and fields

- **Orders:** `order_id`, `customer_id`, `order_status`, `order_purchase_timestamp`, `order_delivered_customer_date`, and `order_estimated_delivery_date`.
- **Order reviews:** `order_id` and `review_score`.
- **Order items:** `order_id`, `product_id`, `price`, and `freight_value`.
- **Products:** `product_id` and `product_category_name`.
- **Customers:** `customer_id` and `customer_state`.

The availability, completeness, data types, and relationships of these fields must be validated during the Data Audit.

### Limitations and uncertainties

- Review scores do not identify the exact reason for customer satisfaction or dissatisfaction.
- An observed relationship between delivery delays and review scores does not prove that delays caused lower scores.
- Reviews may not represent every customer or every order.
- Multiple review records linked to the same order, if present, may affect KPI calculations and must be investigated.
- Orders containing multiple products create a challenge when attributing an order-level review score to a specific product category.
- Canceled, undelivered, and incomplete orders require explicit handling in delivery metrics.
- Shipping cost alone does not measure total customer cost or business profitability.
- The dataset represents historical activity; findings may not describe current marketplace performance.

## G. What a useful business deliverable looks like

The final deliverable should be a clear business report or dashboard that helps stakeholders understand customer review patterns and identify areas that deserve further investigation.

It should include:

- The primary KPI and supporting KPIs, with clear definitions and exclusions.
- Comparisons between review scores and delivery delay patterns.
- Regional and product-category comparisons where the data supports reliable analysis.
- Clear charts and concise explanations of the main findings.
- Practical recommendations linked to the evidence, with limitations stated clearly.

### Success criteria

The deliverable will be considered useful if it answers the agreed business questions, uses reproducible KPI definitions, explains data limitations, and provides actionable insights for stakeholders. Success will be judged by the usefulness and reliability of the analysis, not by model accuracy alone.

## H. Open questions for Mahmoud / tech-lead review

1. Should Customer Experience remain the primary objective, or should Delivery Performance receive higher priority?
2. Should a low review score be defined as 1–2 stars?
3. If multiple review records are linked to one order, how should they be aggregated?
4. Should the study period be defined by the order purchase date?
5. How should canceled, undelivered, and incomplete orders be handled in delivery metrics?
6. How should review scores be analyzed for orders containing products from multiple categories?
7. Should repeat purchasing be included in this phase or considered in a later analysis?

### Next steps

- Review these proposals with Mahmoud and agree on the priority objective and initial business questions.
- Validate the required fields, relationships, missing values, and aggregation rules during the Data Audit.
- Update the shared business understanding document after the peer review.