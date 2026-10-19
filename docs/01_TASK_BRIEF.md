# Phase 01 — Business Understanding (Team Assignment)

**Project:** Olist Retail Intelligence  
**Scenario:** Your data science team was asked by a hypothetical marketplace management group to
identify useful ways to understand commercial performance and customer experience.

### Today's goal
Agree on **what decision the company wants to improve** before looking for correlations, cleaning data,
or picking algorithms. You are not expected to calculate any metric today.

## Independent work (each person submits a different file)
Mahmoud → `docs/mahmoud_business_notes.md`  
Ahmed → `docs/ahmed_business_notes.md`

Each person should independently provide:

1. **Business objectives:** Propose 2–3 possible objectives. Select one and explain the business value.
2. **Stakeholders:** Who would use the findings (operations, customer success, management, etc.)?
3. **Business questions:** Propose at least **five** specific questions, rank the top three, and explain why.
4. **KPIs:** Propose one primary KPI and at least **three** supporting KPIs. Define each KPI precisely:
   numerator/denominator if relevant, observation grain, time period, and exclusions.
5. **Hypotheses:** Write at least two hypotheses to test later, clearly labeled **unverified**.
6. **Feasibility & limitations:** Identify what data you *think* is needed, what might be missing,
   and what cannot be claimed without additional data.
7. **Success criteria:** What would make this project useful to a manager, beyond model accuracy?

### Team review and decision
- Open one PR per teammate; each person reviews the other's PR.
- Agree on the single priority objective and 3–5 initial business questions.
- Document the shared decision in `docs/01_business_understanding.md`.
- Leave disagreements as open questions, not invented facts.

## Questions to consider (don't treat them as findings!)
- Is total paid value the same as sales of products?
- Are canceled/unavailable orders included in an order KPI?
- Can "customer" represent a person across multiple purchases?
- Can an average customer rating establish the *cause* of a bad experience?
- Can profit be computed without costs and refunds?

## Definition of Done
[ ] Two independent notes committed under two GitHub identities  
[ ] Two PRs reviewed  
[ ] Shared business brief approved  
[ ] Primary KPI and supporting metrics have definitions  
[ ] Assumptions, unavailable metrics, and open questions are visible  

Next phase: **Data Understanding & Data Audit** in Colab.
