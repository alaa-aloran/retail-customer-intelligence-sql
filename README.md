# Retail Customer Intelligence

**A business-focused SQL analysis of purchasing behavior, customer value, and retention opportunities**

## The business challenge

How can a retailer use transaction data to understand shopping habits, identify valuable customers, and prioritize opportunities to improve engagement?

I explored this question using the **dunnhumby _The Complete Journey_** dataset, treating the retailer as a hypothetical client. The project moves from purchasing patterns to customer value and, finally, actionable retention and growth opportunities.

**At a glance:** 2,500 purchasing households · 275,539 qualifying purchase baskets · 711 days of transactions · 3 analytical objectives

## Key findings

### 1. Understand purchasing behavior

**Question:** How often do customers purchase, and which households show signs of reduced activity?

- The median household made **78 purchases**; the median time between consecutive purchasing days was **3 days**.
- **49.56% of households** showed declining purchase frequency or had no qualifying purchase in the latest 90 days. Of these, **174 households (6.96%)** met the 90-day inactivity threshold.

**Business implication:** Monitoring both purchase frequency and how regularly customers return can help identify changes in engagement before prolonged inactivity develops.

![Customer purchasing activity status](outputs/charts/03_obj1_customer_activity_status.png)

### 2. Identify customer value drivers

**Question:** Where is customer spending concentrated, and what distinguishes higher-spending households?

- The **top 20% of households contributed 53.05%** of recorded sales.
- Purchase frequency had a stronger positive association with total spending (**0.703 correlation**) than average basket value (**0.390**).
- Higher-spending households purchased from **16.14 departments on average**, compared with **11.31** for other households.

**Business implication:** Retaining higher-spending households matters, while encouraging repeat purchases and broader department engagement offers opportunities worth testing.

![Customer spending distribution](outputs/charts/04_obj2_customer_revenue_by_decile.png)

### 3. Prioritize retention and growth opportunities

**Question:** Which customer groups deserve attention, and what can promotional behavior tell us?

**RFM segmentation** groups customers based on how recently they purchased (Recency), how often they purchased (Frequency), and how much they spent (Monetary). See the [methodology](docs/methodology.md) for scoring and segment definitions.

- **Champions** represented **21.56% of households** but contributed **48.05% of recorded sales**.
- RFM analysis identified **328 households** for further investigation as potential repeat-purchase growth candidates.
- Coupon redemption varied across customer segments. However, a 30-day before/after comparison found **no clear increase in purchase frequency** after redemption; it cannot establish promotional impact.

**Business implication:** Use segmentation to prioritize customer engagement tests, then evaluate whether those initiatives actually improve purchasing behavior.

![RFM segments: sales contribution versus household share](outputs/charts/07_obj3_rfm_segment_revenue_vs_households.png)

## Approach and skills demonstrated

**Tools:** PostgreSQL · SQL · VS Code / SQLTools · Git & GitHub

- **Data preparation and validation:** Imported related retail tables, checked data quality, and applied consistent rules for qualifying purchase baskets.
- **Analytical SQL:** Used joins, common table expressions (CTEs), window functions, ranking, segmentation, and statistical aggregates to answer business questions.
- **Business interpretation:** Translated transaction-level patterns into customer metrics, risk indicators, and testable recommendations, while documenting assumptions and limitations.

The SQL scripts are organized by objective and can be reviewed independently; detailed explanations live in the supporting documentation rather than in this README.

## Explore the project

| Resource                                                   | What you'll find                                    |
| ---------------------------------------------------------- | --------------------------------------------------- |
| [Business case](docs/business_case.md)                     | Retail scenario, objectives, and business questions |
| [Findings & business implications](docs/final_insights.md) | Results for all questions and links to the charts   |
| [Analytical methodology](docs/methodology.md)              | Metrics, segmentation rules, and limitations        |
| [SQL learning notes](docs/sql_learning_notes.md)           | SQL techniques applied and what they helped solve   |
| [SQL scripts](sql/)                                        | Executable analysis organized by objective          |
| [Database diagram](docs/database_erd.png)                  | Relationships among the project tables              |

**Data note:** The original dataset files are not included in the repository. Sales figures represent recorded dataset values, not verified profit or net revenue. The 90-day inactivity cutoff and RFM labels are analytical indicators—not confirmed churn—and observational results do not establish causation.

**AI-assisted workflow:** AI tools supported SQL learning, query refinement, analytical checks, and documentation. Business framing, methodological choices, interpretation, and final review were developed through an iterative analytical process.
