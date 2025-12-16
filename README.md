# 📊 Marketing Funnel Analysis (SQL + Python)
### Overview
This project demonstrates how to audit, clean, and analyze messy marketing event data using SQL inside a Python notebook, followed by user-level funnel analysis and conversion metrics.

The focus is on data quality, metric correctness, and business reasoning.

### Problem Statement

Raw marketing event logs are often noisy and inconsistent
- Event names vary in casing and spelling
- Timestamps contain invalid or missing values
- Multiple events per user inflate naive metrics

The goal of this project was to:

1. Audit and clean raw event data
2. Normalize event types into a coherent funnel
3. Build a user-level funnel (not event-level)
4. Compute and interpret conversion rates

### Data Scope & Assumptions

This analysis focuses exclusively on marketing event data (`events_raw`) to construct a user-level funnel.

Although an `orders_raw` table is available, it was intentionally excluded to:
- Avoid mixing event-level and transactional logic
- Keep funnel metrics user-centric rather than revenue-centric
- Ensure conversion rates reflect behavior, not order artifacts

Order data would be more appropriate for a separate revenue or LTV-focused analysis.

This setup mirrors common real-world analytics challenges.

### Dataset
The dataset consists of synthetic but realistic marketing events:
- Page views, signups, cart actions, checkout, purchases, refunds
- Inconsistent event naming (e.g. pageview, PageView, pv)
- Mixed timestamp formats and invalid values
- Multiple events per user

### Approach

##### 1. Data Auditing
Using SQL inside a Jupyter notebook (via DuckDB), I audited:
- Distinct event name variants
- Timestamp formats and invalid values
- Missing user identifiers

This step informed all cleaning decisions.

##### 2. Data Cleaning
Key cleaning steps:
- Removed events with invalid or missing timestamps (~1%)
- Normalized event names into canonical funnel stages:
  - page_view
  - add_to_cart
  - signup
  - begin_checkout
  - purchase
  - refund_or_chargeback
- Retained unmapped events as other rather than forcing them

 ##### 3. User-Level Funnel Construction
 To avoid event inflation:
 - Events were deduplicated to distinct (user_id, event_type) pairs
 - Funnel metrics were computed at the user level, not event level

This ensures each user is counted once per funnel stage.

##### 4. Conversion Analysis
From 450 unique users:
- ~51% reached purchase
- The largest drop-off occurred between signup and checkout initiation

Conversion rates were computed in SQL for metric correctness, then visualized in Python.

### Key Insight
After cleaning and normalizing raw marketing events, a user-level funnel analysis showed that the primary drop-off occurs after signup but before checkout initiation. This suggests that post-signup friction, such as checkout UX, pricing clarity, or user readiness, may be a stronger barrier than top-of-funnel acquisition.

### Tools Used
- Python
- Pandas
- DuckDB (SQL inside Jupyter)
- SQL (CTEs, CASE statements, aggregation)
- Matplotlib (visualization)

### Files
- marketing_sql_python.ipynb - full analysis notebook
- README.md - project summary and insights

### Future Work
- Analyze transactional order data for revenue integrity
- Join funnel events to orders for post-conversion analysis






