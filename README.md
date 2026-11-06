# Customer Churn Analytics

**SQL · Python · Power BI | OTT Subscriber Retention Analysis**

An end-to-end analytics project examining **5,000 OTT subscribers** to identify churn patterns, quantify recurring revenue exposure and prioritize retention opportunities. The analysis combines customer demographics, subscription records and support interactions into a customer-level dataset, with findings presented through Python visualizations and an interactive Power BI dashboard.

## Business Challenge

OTT platforms compete for recurring subscriber spend in a market with numerous alternatives. Understanding which customer groups cancel, how much monthly revenue is associated with those cancellations and how service experience relates to churn helps focus retention efforts.

This project addresses five questions:

- What proportion of subscribers have canceled their subscriptions?
- Which plans, contracts and acquisition channels have the highest churn?
- How much monthly recurring revenue is associated with canceled accounts?
- How do support escalations relate to cancellation?
- Which customer segments and locations should receive retention attention first?

## Results at a Glance

| Metric | Result |
| --- | ---: |
| Total subscribers | 5,000 |
| Churned subscribers | 1,432 |
| Active subscribers | 3,568 |
| Churn rate | 28.64% |
| Retention rate | 71.36% |
| Monthly charges associated with canceled subscriptions | 17,707.68 |
| Average monthly charges across the customer base | 18.83 |
| Support interactions | 5,000 |
| Customers with support history | 2,856 |

Monetary figures are expressed in currency units. Churn rate describes cancellations across the customer base; it is not a monthly churn rate.

## Dashboard

![Customer Churn Analytics dashboard](Screenshot%202026-11-05%20145907.png)

The dashboard brings together subscriber KPIs, cancellation trends, monthly revenue exposure, contract and plan comparisons, and geographic distribution. A country slicer supports analysis across India and Nepal.

## Key Findings

### Subscription plans

| Plan | Subscribers | Cancellations | Churn rate |
| --- | ---: | ---: | ---: |
| Basic | 1,156 | 701 | 60.64% |
| Standard | 2,136 | 482 | 22.57% |
| Premium | 1,708 | 249 | 14.58% |

Basic subscribers contribute **48.95% of all cancellations**, making the plan a priority for reviewing customer value, pricing and service experience.

### Contract types

| Contract | Subscribers | Cancellations | Churn rate |
| --- | ---: | ---: | ---: |
| Monthly | 2,145 | 1,183 | 55.15% |
| Annual | 2,855 | 249 | 8.72% |

Monthly subscribers churn at approximately **6.32 times** the annual-contract rate. Targeted annual-plan offers are a retention opportunity to evaluate through controlled campaigns.

### Acquisition channels

| Channel | Subscribers | Cancellations | Churn rate |
| --- | ---: | ---: | ---: |
| Organic | 2,150 | 0 | 0.00% |
| Paid | 1,449 | 249 | 17.18% |
| Referral | 1,401 | 1,183 | 84.44% |

Referral acquisition has the highest churn. Offer terms, onboarding and the distribution of plans and contracts within this channel warrant further investigation.

### Service experience, geography and timing

- **Support:** The latest escalation indicator has a correlation of approximately **0.42** with churn. Escalated complaints are a priority for service recovery and retention investigation.
- **Geography:** Rajasthan records the highest location churn rate at **32.92%**. Delhi records the largest cancellation volume at **281 subscribers**.
- **Timing:** September 2024 records the highest monthly cancellation count at **144**, associated with **1,729.56** in monthly recurring charges.

## Technology Stack

| Component | Tools | Application |
| --- | --- | --- |
| Database integration | SQLite, `sqlite3`, pandas | Extract related tables through SQL queries |
| Data preparation | pandas, NumPy | Clean records, standardize fields and engineer features |
| Exploratory analysis | pandas, NumPy | Aggregate customer metrics and compare segments |
| Visualization | Matplotlib, Seaborn | Explore trends, segment differences and correlations |
| Business intelligence | Power BI | Build an interactive churn dashboard |
| Analysis environment | Jupyter Notebook | Document and execute the analytical workflow |

## Data Model

The `customer_churn` database contains three tables linked by `customerid`.

| Table | Records | Unit of observation | Main fields |
| --- | ---: | --- | --- |
| `db_customer` | 5,000 | Customer | `customerid`, `name`, `country`, `state`, `gender`, `dob`, `interests`, `pincode` |
| `db_subscription` | 5,000 | Subscriber | `customerid`, `subscription_start_date`, `subscription_type`, `renewal_date`, `plan_type`, `contract_type`, `cancellation_date`, `cancellation_reason`, `monthly_charges`, `cltv`, `churn_score` |
| `db_support` | 5,000 | Support interaction | `customerid`, `complaint_date`, `escalations`, `csat_score`, `col_1`, `comment` |

Customer and subscription records have a one-to-one relationship. Support records have a one-to-many relationship with customers, covering **2,856 distinct subscribers**.

## Analytical Workflow

1. **Extract data:** Connect to SQLite, identify database tables and load records into pandas using SQL queries.
2. **Clean records:** Convert date fields, standardize gender labels, fill missing country values using location mappings and remove fields outside the analytical scope.
3. **Resolve the support relationship:** Calculate complaint counts and retain the latest support interaction per customer. This reduces an initial 7,144-row join to the intended **5,000 customer records**.
4. **Engineer features:** Create churn flags, complaint counts, customer tenure, age and risk bands based on `churn_score`.
5. **Analyze segments:** Calculate churn, retention, monthly revenue exposure and support metrics across customer groups.
6. **Visualize findings:** Build Python charts and a Power BI dashboard to communicate retention priorities.

### Metric definitions

| Metric | Calculation |
| --- | --- |
| Churn flag | `1` when `cancellation_date` exists; otherwise `0` |
| Churn rate | Churned subscribers / total subscribers × 100 |
| Retention rate | 100% − churn rate |
| Segment churn rate | Churned subscribers in a segment / total subscribers in that segment × 100 |
| Lost monthly recurring charges | Sum of `monthly_charges` for churned subscribers |
| Average monthly charges | Mean of `monthly_charges` across all subscribers |
| Complaint frequency | Number of support interactions per customer |
| Latest escalation prevalence | Customers whose latest escalation status is `Y` / all subscribers × 100 |
| Risk bands | Low: score < 50; Medium: 50 ≤ score < 70; High: score ≥ 70 |

Age and active-customer tenure use the execution date and therefore change when the analysis is rerun. Risk bands group the existing churn score. Support associations describe relationships with cancellation rather than causal effects.

## Repository Files

Keep the following files in the repository root so the links and setup instructions work as written.

| File | Purpose |
| --- | --- |
| [churn_analysis-II.ipynb](churn_analysis-II.ipynb) | Python analysis and visualizations |
| [customer_churn_data_5000_rows_each(1).xlsx](customer_churn_data_5000_rows_each%281%29.xlsx) | Customer, subscription and support data |
| [Churn Analysis Dashboard.pbix](Churn%20Analysis%20Dashboard.pbix) | Interactive Power BI report |
| [Screenshot 2026-11-05 145907.png](Screenshot%202026-11-05%20145907.png) | Dashboard preview |
| [Customer_Churn_Analytics_Report.docx](Customer_Churn_Analytics_Report.docx) | Detailed project report |
| `customer_churn.db` | SQLite database created during setup |
| `churn_5000.csv` | Customer-level dataset exported during analysis |

## Run the Project

### 1. Install dependencies

Use Python 3 and install the analysis libraries and Jupyter:

```bash
python -m pip install pandas numpy matplotlib seaborn openpyxl notebook
```

`sqlite3` is included with Python. Power BI Desktop is used to open the `.pbix` report.

### 2. Initialize the database

Run the following code from the repository root before executing the analysis notebook. It creates `customer_churn.db` from the Excel tables if the database does not already exist.

```python
from pathlib import Path
import sqlite3
import pandas as pd

workbook = Path("customer_churn_data_5000_rows_each(1).xlsx")
database = Path("customer_churn.db")
table_names = ("db_customer", "db_subscription", "db_support")

if database.exists():
    with sqlite3.connect(database) as connection:
        existing_tables = {
            row[0]
            for row in connection.execute(
                "SELECT name FROM sqlite_master WHERE type = 'table'"
            )
        }
    missing_tables = set(table_names) - existing_tables
    if missing_tables:
        raise ValueError(
            f"Database is missing tables: {sorted(missing_tables)}. "
            "Rename the incomplete database and rerun setup."
        )
    print("Database ready.")
else:
    sheets = pd.read_excel(workbook, sheet_name=list(table_names))
    with sqlite3.connect(database) as connection:
        for name, frame in sheets.items():
            frame.to_sql(name, connection, index=False, if_exists="fail")
    print("Created customer_churn.db.")
```

### 3. Run the notebook

Launch Jupyter from the repository root:

```bash
python -m notebook
```

Open `churn_analysis-II.ipynb` and run the cells in order. The workflow loads the SQLite tables, prepares the customer-level data, calculates metrics and generates visualizations. It also exports `churn_5000.csv`.

### 4. Open the dashboard

Open `Churn Analysis Dashboard.pbix` in Power BI Desktop. If the report uses a local path from another machine, update the data-source connection to the matching project file before refreshing. Use the country slicer to explore customer segments.

## Recommended Business Actions

- **Review Basic-plan retention:** Investigate pricing, perceived value and customer experience within the segment contributing the largest share of cancellations.
- **Test annual-plan offers:** Target eligible monthly subscribers and evaluate incremental retention and revenue after incentives.
- **Strengthen service recovery:** Prioritize active customers with unresolved or repeated complaints, using account value and risk bands to guide outreach.
- **Review referral acquisition:** Examine offer design, onboarding and customer fit to address elevated channel churn.
- **Target geographic priorities:** Focus on cancellation volume in Delhi and relative churn in Rajasthan.

Track campaign performance through retained customers, retained monthly revenue, resolution time and customer satisfaction.

## Author

**Swayam Dalal**  
Data Analytics & Business Intelligence
