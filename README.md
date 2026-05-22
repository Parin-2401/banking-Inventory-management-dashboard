# Banking Operations & Risk KPI Dashboard

## Overview

This project is a Power BI portfolio dashboard designed to simulate a Canadian banking operations reporting environment. It analyzes synthetic banking transaction data to monitor operational KPIs, SLA performance, exception handling, reconciliation breaks, branch/channel performance, and risk indicators.

The dashboard is built for BI Analyst, BI Developer, Data Analyst, Reporting Analyst, and banking analytics roles.

> **Note:** This project uses synthetic data only. No real customer, banking, account, employer, or confidential data is used.

---

## GitHub Repository Description

Power BI banking analytics dashboard for operations, risk, audit, SLA, reconciliation, and exception monitoring using SQL, DAX, and synthetic transaction data.

---

## Business Problem

Banking operations teams need timely visibility into transaction processing performance, operational exceptions, reconciliation issues, and SLA breaches. Manual reporting can delay insights and make it harder to identify high-risk branches, channels, or process gaps.

This project solves that problem by creating a centralized Power BI dashboard that helps stakeholders answer:

- Which branches or channels have the highest transaction volumes?
- Where are SLA breaches occurring most often?
- Which exception types create the highest operational risk?
- How many reconciliation breaks are unresolved?
- Which business areas require audit or compliance attention?
- Are operational KPIs improving or declining over time?

---

## Key Features

- Executive KPI overview
- Branch and province-level performance analysis
- Channel performance comparison
- SLA breach monitoring
- Exception and risk analytics
- Reconciliation break reporting
- Data quality issue tracking
- Drill-through views for branch and exception details
- Month-over-month trend analysis
- Interactive slicers for date, province, branch, channel, and severity

---

## Tools and Technologies

- **Power BI Desktop**
- **Power Query**
- **DAX**
- **SQL**
- **Python** for synthetic data generation
- **CSV / SQL database**
- **GitHub** for project documentation

---

## Data Model

The dashboard follows a star schema design.

```text
DimDate
   |
FactTransactions ----- DimBranch
   |                  DimChannel
   |                  DimCustomerSegment
   |
FactExceptions
```

### Main Tables

| Table | Description |
|---|---|
| FactTransactions | Synthetic banking transaction records |
| FactExceptions | Operational exceptions, SLA issues, reconciliation breaks |
| DimBranch | Branch, city, province, and regional details |
| DimChannel | Transaction channel details |
| DimCustomerSegment | Customer segment and risk category |
| DimDate | Calendar and fiscal reporting attributes |

---

## Dashboard Pages

### 1. Executive Overview

Shows high-level KPIs including total transactions, transaction amount, SLA breach rate, exception rate, reconciliation break rate, and risk indicators.

### 2. Branch & Regional Performance

Compares branches and provinces by transaction volume, SLA performance, exceptions, and operational trends.

### 3. Channel Performance

Analyzes transaction performance across mobile banking, online banking, ATM, branch, call centre, and back-office channels.

### 4. Exceptions & Risk Monitoring

Tracks exception type, severity, root cause, assigned team, open/resolved status, and aging buckets.

### 5. Reconciliation & Data Quality

Monitors matched/unmatched transactions, reconciliation breaks, pending reviews, duplicate records, and data quality issues.

---

## Sample KPIs

| KPI | Description |
|---|---|
| Total Transactions | Count of all transactions processed |
| Total Transaction Amount | Sum of transaction values |
| SLA Breach Rate | Percentage of transactions processed outside SLA |
| Exception Rate | Exceptions as a percentage of total transactions |
| Reconciliation Break Rate | Percentage of unmatched or unresolved reconciliation items |
| Average Processing Time | Average duration between processing start and end |
| Critical Exceptions | Count of critical severity exceptions |
| Average Days to Resolve | Average time required to close exceptions |

---

## Sample DAX Measures

```DAX
Total Transactions = COUNTROWS(FactTransactions)

Total Transaction Amount = SUM(FactTransactions[transaction_amount])

SLA Breach Count =
CALCULATE(
    [Total Transactions],
    FactTransactions[is_sla_breached] = TRUE()
)

SLA Breach Rate =
DIVIDE([SLA Breach Count], [Total Transactions])

Total Exceptions = COUNTROWS(FactExceptions)

Open Exceptions =
CALCULATE(
    [Total Exceptions],
    FactExceptions[resolution_status] = "Open"
)

Exception Rate =
DIVIDE([Total Exceptions], [Total Transactions])

Reconciliation Break Count =
CALCULATE(
    [Total Transactions],
    FactTransactions[reconciliation_status] IN {"Unmatched", "Pending Review"}
)

Reconciliation Break Rate =
DIVIDE([Reconciliation Break Count], [Total Transactions])
```

---

## SQL Data Quality Checks

Example checks included in the project:

```SQL
-- Duplicate transaction IDs
SELECT transaction_id, COUNT(*) AS duplicate_count
FROM fact_transactions
GROUP BY transaction_id
HAVING COUNT(*) > 1;

-- SLA breach rate by channel
SELECT
    channel_id,
    COUNT(*) AS total_transactions,
    SUM(CASE WHEN is_sla_breached = 1 THEN 1 ELSE 0 END) AS sla_breaches,
    ROUND(
        100.0 * SUM(CASE WHEN is_sla_breached = 1 THEN 1 ELSE 0 END) / COUNT(*),
        2
    ) AS sla_breach_rate
FROM fact_transactions
GROUP BY channel_id;

-- Open exceptions by severity
SELECT
    severity,
    COUNT(*) AS open_exception_count
FROM fact_exceptions
WHERE resolution_status = 'Open'
GROUP BY severity
ORDER BY open_exception_count DESC;
```

---

## Repository Structure

```text
banking-operations-risk-dashboard/
├── data/
│   ├── raw/
│   ├── processed/
│   └── sample/
├── sql/
│   ├── create_tables.sql
│   ├── data_quality_checks.sql
│   └── aggregation_queries.sql
├── powerbi/
│   ├── dashboard.pbix
│   └── dashboard_screenshots/
├── docs/
│   ├── business_requirements.md
│   ├── data_dictionary.md
│   ├── dashboard_wireframe.md
│   └── metric_definitions.md
├── scripts/
│   └── generate_synthetic_data.py
├── README.md
└── CLAUDE.md
```

---

## Project Workflow

1. Generate synthetic banking data.
2. Store raw files in `data/raw`.
3. Clean and transform data using SQL and Power Query.
4. Build a star schema in Power BI.
5. Create DAX measures for operational KPIs.
6. Design report pages for executive, branch, channel, risk, and reconciliation analysis.
7. Validate results using SQL checks.
8. Add dashboard screenshots and documentation.
9. Publish project to GitHub.

---

## Business Impact

This dashboard demonstrates how banking teams can improve reporting efficiency, strengthen operational visibility, identify control issues, and support audit/risk stakeholders through automated BI reporting.

Potential business value:

- Reduced manual reporting effort
- Faster identification of SLA breaches
- Improved exception monitoring
- Better branch/channel performance visibility
- Stronger data quality and reconciliation oversight

---

## Skills Demonstrated

- Power BI dashboard development
- DAX measure creation
- SQL data validation
- Data modeling
- Star schema design
- Power Query transformation
- Banking KPI reporting
- Audit and risk analytics
- Data storytelling
- GitHub documentation

---

## Future Enhancements

- Add row-level security by region or branch.
- Add predictive SLA breach indicators.
- Add Power BI incremental refresh.
- Add deployment pipeline documentation.
- Connect dashboard to a SQL database instead of static CSV files.
- Create automated monthly refresh workflow.
