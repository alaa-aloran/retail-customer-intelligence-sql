# Retail Customer Intelligence — Findings & Business Implications

**Dataset:** dunnhumby _The Complete Journey_ | **Coverage:** 2,500 purchasing households, 711 relative days  
**Project context:** Portfolio analysis for a hypothetical retail client. Amounts are recorded sales in dataset units, not verified profit or net revenue.

## Executive Summary

- **Sales concentration:** The top 20% of households generated **53.05%** of recorded sales.
- **Repeat purchasing matters:** Shopping frequency has a stronger linear association with spending (**0.703**) than average basket value (**0.390**).
- **Retention priorities:** Champions generated **48.05%** of sales, while the At-Risk segment contributed another **16.26%**; these are historical contributions, not forecasts.
- **Growth candidates:** **328 households** met screening criteria for possible repeat-purchase initiatives.
- **Promotions:** Coupon redemption varied by segment, but purchasing changed only modestly around first redemption. **Engagement does not automatically mean increased purchasing.**

## Objective 1 — Understand Purchasing Behavior

### Q1. How often do households purchase?

Across **2,500 households**, the analysis identified **275,539 qualifying baskets** over 711 days. The median household made **78 purchases**, while the median time between consecutive _purchasing days_ was **3 days**.

**Business implication:** Understanding how frequently households purchase and how regularly they return can help retailers identify changes in customer engagement and detect early signs of declining activity.

### Q2. How did purchasing and spending change over time?

Weekly purchasing activity grew during roughly the first **17 weeks**, then broadly stabilized. Recorded weekly sales and average basket values were generally somewhat higher later in the observation period.

**Business implication:** Monitor purchase volume alongside basket value rather than relying on sales totals alone.

### Q3. Which households show declining activity or inactivity?

| Activity status     | Households |  Share |
| ------------------- | ---------: | -----: |
| Active — Increasing |      1,075 | 43.00% |
| Active — Declining  |      1,065 | 42.60% |
| Active — Stable     |        186 |  7.44% |
| Inactive — 90+ Days |        174 |  6.96% |

**Business implication:** **49.56%** were either purchasing less often across the two latest 90-day periods or had been inactive for at least 90 days. These are investigation signals, **not confirmed churn**.

**Objective 1 charts:** [Weekly purchasing](../outputs/charts/01_obj1_weekly_purchasing_activity.png) · [Weekly spending](../outputs/charts/02_obj1_weekly_customer_spending.png) · [Activity status](../outputs/charts/03_obj1_customer_activity_status.png)

## Objective 2 — Identify Customer Value Drivers

### Q4. How is recorded sales value distributed?

Recorded sales totaled **8,057,462.65**. The top **10%** of households contributed **34.29%**, and the top **20%** contributed **53.05%**. The bottom half contributed **15.66%**. Average household spending (**3,222.99**) exceeded the median (**2,157.75**).

**Business implication:** Revenue concentration makes engagement among high-spending households particularly important.

[Revenue distribution by spending decile](../outputs/charts/04_obj2_customer_revenue_by_decile.png)

### Q5. Which purchasing measure is more closely associated with spending?

Household spending correlated more strongly with **purchase frequency (0.703)** than with **average basket value (0.390)**.

**Business implication:** Repeat-purchase initiatives warrant investigation, but these correlations do **not** demonstrate causation. Spending is mathematically related to both measures.

### Q6. How do department spending and variety differ?

**High-Value** refers to the top 20% of households by spending (500); **Lower-Value** refers to the remaining 80% (2,000). Department shares are calculated _within each group_.

| Department | High-Value | Lower-Value |
| ---------- | ---------: | ----------: |
| Grocery    |     49.01% |      52.84% |
| Drug GM    |     13.49% |      12.66% |
| Kiosk-Gas  |      8.30% |       5.01% |
| Produce    |      6.99% |       6.84% |
| Meat       |      6.48% |       7.19% |

Both groups spend heavily in Grocery, but Kiosk-Gas accounts for a larger share among High-Value households. High-Value households purchased from **16.14 departments on average**, compared with **11.31** for Lower-Value households.

**Business implication:** Exploring broader department engagement may be useful, though frequent shoppers naturally have more opportunities to buy across departments. Kiosk-Gas should not be interpreted as ordinary grocery merchandise.

**Objective 2 charts:** [Department spending mix](../outputs/charts/05_obj2_department_spending_mix.png) · [Department diversity](../outputs/charts/06_obj2_category_diversity_by_customer_group.png)

## Objective 3 — Identify Retention and Growth Opportunities

### Q7. How were households segmented?

RFM (Recency, Frequency, Monetary) scores classified all **2,500 households** into six groups:

| Segment                       | Households |  Share |
| ----------------------------- | ---------: | -----: |
| Champions                     |        539 | 21.56% |
| Loyal Households              |        306 | 12.24% |
| Potential Loyalists           |        414 | 16.56% |
| Regular / Moderate Engagement |        266 | 10.64% |
| At-Risk Households            |        426 | 17.04% |
| Hibernating Households        |        549 | 21.96% |

**Business implication:** Different purchasing profiles call for different engagement strategies. Segment rules and suggested actions are in [Analytical Methodology](methodology.md). Labels indicate **relative behavior**, not verified loyalty or churn.

### Q8. Which segments contribute most sales, and which show inactivity?

- **Champions:** **48.05%** of recorded sales; with Loyal Households, **62.76%** combined.
- **At-Risk:** **16.26%** of sales, but only **38 of 426 (8.92%)** were inactive for 90+ days; median inactivity was **20 days**.
- **Hibernating:** **4.30%** of sales; **136 of 549 (24.77%)** were inactive for 90+ days; median inactivity was **41 days**.

All **174 households** meeting the assumed 90-day inactivity threshold fall into the At-Risk or Hibernating groups, reconciling with Objective 1.

**Business implication:** Protect high-contributing segments and investigate inactivity among previously engaged households. RFM labels alone should not be treated as confirmed customer loss.

**Objective 3 charts:** [Sales versus household share](../outputs/charts/07_obj3_rfm_segment_revenue_vs_households.png) · [Inactivity by segment](../outputs/charts/08_obj3_rfm_segment_inactivity.png)

### Q9. Which households are candidates for more engagement?

Potential Loyalists (**414**) and Regular / Moderate Engagement (**266**) purchased less frequently than Loyal Households (about **56–58** versus **160** baskets per household on average), but had higher average basket values (**33.65** and **35.05** versus **25.30**).

An RFM screening rule (**R ≥ 3, F ≤ 3, M ≥ 3**) identified **328 households (13.12%)** for further investigation.

**Business implication:** Test approaches focused on return visits. These are **screened candidates, not predicted marketing responders**.

### Q10. How does promotional engagement vary by segment? _(Supporting)_

Among campaign-assigned households, coupon redemption was highest for **Champions (43.56%)**, followed by **Loyal Households (28.13%)**, and lowest for **Hibernating Households (1.49%)**. At-Risk Households had **81.22% campaign assignment** but **18.79% redemption** among those assigned.

Across the dataset, **1,584 households** were associated with campaigns and **434** redeemed coupons.

**Business implication:** Promotional usage differs by customer segment. Campaign assignment is **not proof of exposure or participation**, and redemption differences do not establish effectiveness.

[Promotional engagement by segment](../outputs/charts/09_obj3_rfm_promotional_engagement.png)

### Q11. Did purchasing change after coupon redemption? _(Exploratory supporting)_

For **432 households** with complete 30-day windows around their first recorded coupon redemption (excluding redemption day):

| Household average  | 30 days before | 30 days after | Average change |
| ------------------ | -------------: | ------------: | -------------: |
| Qualifying baskets |           9.04 |          8.90 |          −0.15 |
| Recorded spending  |         297.31 |        305.34 |          +8.04 |

**Business implication:** Frequency remained broadly stable and spending increased modestly. **Promotional engagement does not automatically correspond to a meaningful increase in purchasing.** Without a control group, these changes cannot be attributed to coupon redemption.

## Overall Business Takeaway

The retailer should **protect strong existing relationships**, **investigate disengagement among historically valuable households**, and **test repeat-purchase strategies for selected lower-frequency customers**. Promotions are worth analyzing, but coupon use alone is not evidence of incremental sales.

_Interpretation notes:_ A qualifying purchase basket contains at least one line with positive quantity and sales; all its recorded sales lines contribute to spending. The 90-day threshold is an analytical assumption. The data uses relative days, and findings are descriptive rather than causal. For full definitions, see [Analytical Methodology](methodology.md). The business-question scope remains documented in [Business Case](business_case.md).
