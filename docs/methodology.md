# Analytical Methodology

**Project:** Retail Customer Intelligence  
**Dataset:** dunnhumby — _The Complete Journey_ (via Kaggle)  
**Coverage:** 2,500 purchasing households; transaction days 1–711 (102 dataset weeks)

This document explains the analytical rules and key limitations behind the project's findings. Business results and recommendations are presented in [`final_insights.md`](final_insights.md); SQL techniques are explained in [`sql_learning_notes.md`](sql_learning_notes.md).

## 1. Shared Data Rules

### What counts as a purchase?

A basket is a **qualifying purchase** if **at least one** transaction line has `quantity > 0 AND sales_value > 0`.

- **275,539** baskets qualify; **945** do not.
- Purchase frequency counts each qualifying `basket_id` once.
- Recorded spending sums **all transaction lines within qualifying baskets**, including lines that do not individually meet the condition.
- Original source data was not modified. Recorded sales are **not verified net revenue or profit**, and no currency is assumed.

All analyses use the dataset's relative day numbers; no calendar dates were inferred. Some SQL queries repeat basket, household-spending, and segment calculations intentionally so they can run independently with consistent definitions.

## 2. Objective 1 — Understand Purchasing Behavior (Q1–Q3)

**Q1 · Frequency and time between purchases.** Frequency is the number of qualifying baskets per household. Time between shopping occasions uses `LAG()` on **distinct purchasing days** per household; multiple baskets on one day count as one occasion for this measure. The median is calculated across all observed gaps, so households with more shopping days contribute more gaps.

**Q2 · Weekly trends.** Weekly activity counts qualifying baskets and purchasing households. Average baskets and average spending per active household use only households purchasing in that week as their denominator. Weekly sales include all lines of qualifying baskets. **Weeks 1 and 102** are excluded from full-week trend comparisons because they contain five and six observed purchasing days, respectively.

**Q3 · Inactivity.** Basket counts are compared across two non-overlapping 90-day periods: **days 532–621** and **622–711**. Each household receives one status in this priority order:

| Status              | Rule                                                         |
| ------------------- | ------------------------------------------------------------ |
| Inactive — 90+ Days | Last qualifying purchase was at least 90 days before day 711 |
| Active — Declining  | Fewer qualifying baskets in the recent 90 days               |
| Active — Increasing | More qualifying baskets in the recent 90 days                |
| Active — Stable     | Equal basket counts across the two periods                   |

**The 90-day cutoff is an analytical assumption, not confirmed churn.** Even small changes in basket counts affect the active status.

## 3. Objective 2 — Identify Customer Value Drivers (Q4–Q6)

**Q4 · Revenue distribution.** Households are ranked by total recorded spending. `NTILE(10)` divides them into ten approximately equal-sized spending groups to measure sales concentration without fixed spending thresholds.

**Q5 · Spending drivers.** Pearson correlation (`CORR`) compares household spending with (a) qualifying basket frequency and (b) average spending per basket. Correlation measures **linear association, not causation**; total spending is mathematically related to both measures.

**Q6 · Department preferences and diversity.** `NTILE(5)` identifies **High-Value households** (top 20% by spending; 500 households) and **Lower-Value households** (remaining 80%; 2,000). These are analytical comparison groups, separate from the RFM segments in Objective 3.

- Each department's spending share uses **that customer group's total recorded sales** as the denominator.
- Department diversity counts distinct nonblank departments with **positive quantity and positive sales** for each household.
- Fuel and miscellaneous departments remain included. Greater department variety may partly reflect more opportunities to purchase among frequent shoppers.

## 4. Objective 3 — Identify Retention and Growth Opportunities (Q7–Q11)

### Q7 · RFM scoring and segments

RFM combines three household measures over the observation period:

| Measure       | Meaning                                               | Direction of higher score |
| ------------- | ----------------------------------------------------- | ------------------------- |
| Recency (R)   | Days since last qualifying purchase: `711 - MAX(day)` | More recent = higher      |
| Frequency (F) | Number of qualifying baskets                          | More baskets = higher     |
| Monetary (M)  | Total recorded sales in qualifying baskets            | More spending = higher    |

- **R score:** `CUME_DIST()` mapped to scores 1–5, ordered by decreasing days since last purchase. Equal Recency days receive equal scores.
- **F and M scores:** `NTILE(5)` on ascending values, with `household_key` as a tie-breaker. Each score contains approximately one-fifth of households; tied F or M values can be split between groups.
- **Key distinction:** `NTILE()` groups customers by **rank**, not by manually selected spending or frequency thresholds. Scores describe relative behavior, not fixed purchasing standards.

Observed Recency score ranges: **R5: 0–1 days; R4: 2–4; R3: 5–9; R2: 10–27; R1: 28–657**. The wide R1 range is one reason segment labels should not be interpreted as confirmed churn.

The following **ordered SQL `CASE` rules** assign every household to one segment. These project-specific rules were refined after reviewing the first segmentation results.

| Segment                       | RFM rule (evaluated in order) | Typical behavior                            | Potential business action        |
| ----------------------------- | ----------------------------- | ------------------------------------------- | -------------------------------- |
| Champions                     | R ≥ 4, F ≥ 4, M ≥ 4           | Recent, frequent, high-spending             | Reward and retain                |
| Loyal Households              | R ≥ 3, F ≥ 4; not Champions   | Frequent, reasonably recent                 | Maintain loyalty; cross-sell     |
| Potential Loyalists           | R ≥ 4, F ≤ 3                  | Recent, less frequent                       | Encourage repeat purchases       |
| Regular / Moderate Engagement | R = 3, F ≤ 3                  | Intermediate activity                       | Explore engagement opportunities |
| At-Risk Households            | R ≤ 2 and (F ≥ 3 or M ≥ 3)    | Less recent, some historical activity/value | Investigate disengagement        |
| Hibernating Households        | R ≤ 2, F ≤ 2, M ≤ 2           | Less recent, historically low activity      | Consider low-cost reactivation   |

These are **working behavioral labels**, not verified loyalty, churn, or response likelihood.

### Q8 · Revenue and inactivity by segment

Segment revenue is the sum of household recorded spending; contribution is measured against the total across all **2,500 households**. Each segment's actual `recency_days` is also summarized (mean, median, and share inactive **90+ days**). This separately checks whether the relative RFM labels correspond to prolonged inactivity; it does not redefine segment membership.

### Q9 · Growth-candidate screening

Potential Loyalists and Regular / Moderate Engagement households are compared with Loyal Households. Average basket value is calculated **per household**, then averaged across households, so each household has equal weight. Candidates meet **R ≥ 3, F ≤ 3, M ≥ 3** (328 households); the SQL preview shows only the 20 highest-spending matches. This is a screening rule, **not a growth forecast**.

### Q10 · Campaign assignment and coupon redemption (supporting)

Campaign associations come from `campaign_table`; redemptions from `coupon_redempt`; campaign dates from `campaign_desc`. All **2,318** redemptions matched an assigned campaign and fell within its recorded dates.

`EXISTS` identifies **unique households** assigned to at least one campaign or redeeming at least one coupon, avoiding double-counting. Assignment rate is the share of households in each RFM segment assigned to a campaign. Redemption rate is the share of **assigned households** that redeemed at least one coupon. **Assignment does not prove exposure or participation.** RFM segments use the full observation period, not historical scores at each campaign date.

### Q11 · Purchasing before and after redemption (exploratory supporting)

Each household's **first recorded coupon redemption** anchors two equal 30-day windows: one before and one after, excluding redemption day. Only first redemptions on **days 31–681** qualify, leaving **432 of 434** redeeming households with complete windows. Households with no purchases in a window remain included.

This is a **descriptive before/after comparison**, not a causal estimate. There is no non-redeeming control group, and seasonal effects, concurrent campaigns, and household selection are not controlled. Some campaigns extend to day 719, beyond the last transaction day (711).

## 5. Boundaries and Interpretation

- Basket counts do not necessarily represent distinct in-store visits.
- Observation covers only the dataset period; activity before or after it is unknown.
- Demographics are available for only **801 of 2,500** purchasing households and are not used to generalize results to the entire customer base.
- Associations and suggested marketing actions are **opportunities for investigation**, not demonstrated effects or guaranteed results.
- **Q10–Q11 are retained as supporting analyses**; Q11 is explicitly exploratory. The agreed questions remain in [`business_case.md`](business_case.md), which serves as the scope baseline.
