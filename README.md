# 📊 Marketing Funnel & Orders Analysis (SQL + Python)
### Overview
This project demonstrates how to audit, clean, and analyze messy marketing and transactional data using SQL inside Python notebooks.

It is split into two focused analyses:

	1.	Marketing event funnel analysis (behavioral metrics)
	2.	Orders and revenue analysis (transactional integrity)

The emphasis throughout is on data quality, metric correctness, and business reasoning.

### Problem Statement

Raw analytics data is often noisy and inconsistent across both event and transaction layers:
- Event names vary in casing and spelling
- Timestamps contain invalid or missing values
- Multiple events per user inflate naïve metrics
- Transactional fields mix text, symbols, and currencies

Without careful auditing and cleaning, derived metrics such as conversion rates or revenue can be misleading.

### Project Structure
This repository contains two complementary notebooks, each scoped to a specific business question:

	1.	Marketing Funnel Analysis (marketing_sql_python.ipynb)
    - Focuses on user behavior and conversion through the funnel.
	2.	Orders & Revenue Analysis (orders_analysis.ipynb)
    - Focuses on transaction validity and revenue metrics after cleaning.

This separation mirrors real-world analytics workflows and avoids mixing behavioral and financial logic.

### Dataset
The project uses synthetic but realistic data designed to reflect common analytics challenges.

##### Marketing Events
- Page views, signups, cart actions, checkout, purchases, refunds
- Inconsistent event naming (e.g. pageview, PageView, pv)
- Mixed timestamp formats and invalid values
- Multiple events per user

##### Orders
- Mixed-format monetary values (e.g. AED 70.41, US$ 128.85, free)
- Inconsistent currency representations
- Non-standardized order statuses
- Missing identifiers and currency attribution

### Analysis 1: Marketing Funnel Analysis

#### Approach

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

#### Key Insight
After cleaning and normalizing raw marketing events, a user-level funnel analysis showed that the primary drop-off occurs after signup but before checkout initiation. This suggests that post-signup friction, such as checkout UX, pricing clarity, or user readiness, may be a stronger barrier than top-of-funnel acquisition.

### Analysis 2: Orders & Revenue Analysis

#### Approach
The orders analysis focused on transactional integrity rather than behavior.

Key steps included:
- Normalizing inconsistent order statuses into a canonical paid state
- Extracting numeric values from mixed-format monetary fields using regex
- Standardizing currencies with fallback inference from raw amount strings
- Flagging orders with missing currency attribution or unparseable amounts

#### Key Insight
After auditing and cleaning the raw order data, all observed orders mapped to successful payments, while approximately 26% of orders lacked explicit currency attribution. Revenue metrics were therefore computed by currency where available, reinforcing the importance of separating behavioral funnel analysis from transactional revenue reporting.

### Tools Used
- Python
- Pandas
- DuckDB (SQL inside Jupyter)
- SQL (CTEs, CASE statements, regex-based cleaning)
- Matplotlib (visualization)

### Files
- Notebooks
  - 01_event_audit_and_cleaning.ipynb - full analysis notebook
  - 02_orders_analysis.ipynb - order cleaning and revenue metrics
- Data
  - marketing_events_raw.csv
  - orders_raw.csv
- README.md - project summary and insights
