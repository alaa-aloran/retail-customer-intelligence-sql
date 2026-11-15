# SQL Learning Notes

A quick reference to the PostgreSQL techniques used in the **Retail Customer Intelligence** project, and the business questions they helped answer.

## SQL techniques in practice

| Technique            | What it does                                                | How it was used                                                   |
| -------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------- |
| `CTE (WITH)`         | Organizes complex analysis into readable steps              | Qualifying baskets, household metrics, and RFM scoring            |
| `LAG()`              | Compares each row with a preceding row                      | Days between consecutive purchasing days                          |
| `FILTER (WHERE ...)` | Applies conditions within an aggregate                      | Activity status and segment-level counts                          |
| `PERCENTILE_CONT()`  | Calculates percentiles, including medians                   | Typical purchase gaps and household spending                      |
| `NTILE()`            | Divides ranked rows into near-equal groups                  | Spending deciles, value groups, and RFM Frequency/Monetary scores |
| `CUME_DIST()`        | Calculates cumulative rank while preserving ties            | RFM Recency scores for households with the same last-purchase day |
| `ROW_NUMBER()`       | Numbers rows in a defined order                             | Household spending rankings                                       |
| `CORR()`             | Measures the strength and direction of a linear association | Spending versus purchase frequency and average basket value       |
| `CASE WHEN`          | Classifies records using ordered conditions                 | Customer activity statuses and six RFM segments                   |
| `EXISTS`             | Checks whether at least one related record exists           | Campaign assignment and coupon redemption by household            |
| `COUNT(*) OVER ()`   | Adds a total count without collapsing rows                  | Reporting all 328 growth candidates while displaying the top 20   |
| `MIN()` and `HAVING` | Find an earliest event and filter grouped results           | First coupon redemption and eligible before/after windows         |
| `COALESCE()`         | Replaces missing values with a specified fallback           | Retaining zero-purchase windows in promotional comparisons        |

## Four important distinctions

**`NTILE()` vs. `CUME_DIST()`**  
`NTILE(5)` places ranked households into five approximately equal-sized groups; equal values can be split across groups. `CUME_DIST()` gives tied values the same cumulative rank, which is why it was used for Recency scoring. These scores describe **relative behavior**, not fixed spending or inactivity thresholds.

**`EXISTS` vs. `JOIN`**  
Use `EXISTS` to answer _“Did this household redeem a coupon at least once?”_ without duplicating households that have multiple matching records. Use joins when details from related rows are needed.

**`CORR()` vs. causation**  
`CORR()` measures linear association, not whether one variable causes another to change. In this project, household spending was more strongly associated with purchase frequency (`0.703`) than with average basket value (`0.390`).

**`COUNT(*) OVER ()` vs. `COUNT(*)`**  
A window count shows the size of the qualifying population alongside individual rows. It allowed the growth shortlist to display **20 example households** while retaining the **328-household total**.

> **Interpretation note:** RFM labels are analytical segments, not confirmed loyalty or churn. Coupon redemption and 30-day before/after comparisons are descriptive; they do not establish promotional impact.

For methodological rules and assumptions, see [Analytical Methodology](methodology.md). For findings and recommendations, see [Findings & Business Implications](final_insights.md).
