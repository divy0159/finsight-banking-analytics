## Finsight-banking-analytics
End-to-end banking analytics on Snowflake and Power BI: a cloud data warehouse, a cleaned and modelled dataset, and a validated Executive Overview dashboard covering transactions, fraud and lending, with a one-click summary using power automate as an added feature.

## Contents
1. Overview
2. Business Problem
3. Objectives
4. Scope
5. Solution Overview
6. Stage 1: Source Data
7. Stage 2: Data Warehouse (Snowflake)
8. Stage 3: Data Import and Cleaning (Power Query)
9. Stage 4: Data Model (Power BI)
10. Stage 5: Dashboard
11. Stage 6: Validation
12. Stage 7: One-Click Summary Feature
13. Limitations
14. Roadmap
15. How to Reproduce
16. What I Learned
17. Repository Structure

## Overview
FinSight takes a synthetic retail banking dataset (about 1 million transactions across seven related tables) and turns it into a single, trusted view of bank performance. The project follows the full analytics path, in order:
1. Load the raw data into Snowflake.
2. Clean it in Power Query.
3. Model it in Power BI with DAX measures.
4. Present it in an Executive Overview dashboard.
5. Validate the results.
An added feature, a one-click summary built with Power Automate, lets users log or email a KPI summary straight from the dashboard.

## Business Problem
Context
A retail bank records activity across transactions, accounts, loans, credit cards, customers, branches and employees. In practice this data sits in separate files or systems, often owned by different teams.

##	Business question	KPIs used to answer it
1. Is the transaction platform performing reliably?	Success rate; failed, pending and reversed transactions
2. What is our fraud exposure?	Fraud rate (% of transactions), fraud amount, fraud share of value
3. How much credit risk are we carrying?	Total loan amount, outstanding loans, outstanding share, credit card outstanding
4. Where is value generated?	Transaction value by state, branch, payment channel and transaction type

## Objectives
The primary objective of FinSight is to provide a consolidated and reliable view of retail banking performance, enabling management to monitor operational efficiency, assess financial risk, and make informed business decisions.

## The project aims to:
1. **Improve visibility into banking performance** by bringing transaction activity, customer information, and lending metrics into a unified view of overall bank operations.
2. **Monitor transaction reliability** by identifying transaction success and failure patterns, pending transactions, and reversals to highlight potential operational inefficiencies.
3. **Assess fraud exposure** by analysing fraud frequency, transaction value, and the distribution of fraudulent activity to identify areas requiring further investigation.
4. **Evaluate lending and credit exposure** by examining loan disbursements, outstanding loan balances, and credit card outstanding amounts to understand the bank's existing credit commitments.
5. **Identify business performance patterns** across time periods, regions, branches, transaction types, and payment channels to support comparisons and highlight areas of interest.
6. **Enable consistent, data-driven decision-making** by establishing a common view of key performance indicators and providing management with accessible, filterable business insights.

## Scope
1. Analysis of synthetic retail banking data covering transactions, customers, accounts, loans, credit cards, branches, and employees.
2. Evaluation of transaction performance, including success rates, failed transactions, pending transactions, and reversals.
3. Assessment of fraud exposure using fraud rates, fraudulent transaction amounts, and fraud's share of total transaction value.
4. Analysis of lending exposure through total loan amounts, outstanding loans, and credit card outstanding balances.
5. Comparison of transaction performance across time periods, regions, branches, transaction types, and payment channels.
6. Development of a consolidated Executive Overview to support interactive exploration of banking performance and risk indicators.

## Solution Overview
FinSight follows an end-to-end analytics approach to transform raw banking data into meaningful business insights. The solution brings together cloud data warehousing, data preparation, data modelling, and business intelligence to provide a consolidated view of banking performance.

## Analytics Workflow
1. **Data Source:** Seven synthetic banking datasets provide information on transactions, customers, accounts, loans, credit cards, branches, and employees.
2. **Data Warehouse:** Snowflake stores the source tables in a centralized cloud data warehouse.
3. **Data Cleaning:** Power Query handles data quality issues, including duplicate records, missing identifiers, and invalid placeholder dates.
4. **Data Modelling:** Power BI relationships, a calendar table, and DAX measures establish consistent KPIs across the report.
5. **Data Visualization:** An interactive Executive Overview dashboard presents key metrics and trends related to transactions, fraud, and lending.
6. **Validation:** Dashboard results are compared against independent counts from the underlying data to verify accuracy.
7. **Automated Summary:** An additional Power Automate feature allows users to log a KPI summary, while a mail button prepares an email containing the summary.

## Technology Stack
| Technology | Purpose |
|----------|-----------|
| Snowflake | Centralized cloud data storage and SQL-based data management |
| Power Query | Data cleaning and transformation |
| Power BI | Data modelling, DAX calculations, and interactive dashboards |
| Power Automate | One-click KPI summary logging |
| Excel Online | Storage of timestamped KPI summaries |
The primary deliverable is the validated Executive Overview dashboard. The one-click summary feature is an additional convenience for users who need a quick overview of key banking metrics.

## Stage 1: Source Data

FinSight uses a synthetic retail banking dataset consisting of seven related tables. The dataset represents banking activities across transactions, customers, accounts, loans, credit cards, branches, and employees.

### Dataset Overview

| Table | Approximate Row Count | Description |
|---|---:|---|
| CUSTOMERS | 10,000 | Customer details and demographic information |
| ACCOUNTS | 15,000 | Customer accounts and associated banking information |
| TRANSACTIONS | 1,000,000 | Transaction records, including dates, amounts, status, and fraud indicators |
| BRANCHES | 100 | Branch information and geographical details |
| EMPLOYEES | 500 | Employee information associated with branches |
| LOANS | 5,000 | Loan details, including amounts and outstanding balances |
| CREDIT_CARDS | 8,000 | Credit card information and outstanding balances |

## Data Characteristics
- **Data type:** Synthetic banking data created for analytical purposes.
- **Structure:** Seven related tables connected through identifiers and business relationships.
- **Volume:** Approximately one million transaction records, alongside customer, account, lending, and operational data.
- **Analytical coverage:** Transaction performance, fraud exposure, lending, credit card balances, and geographical comparisons.

## Data Privacy
The dataset does not represent actual bank customers. Sensitive-looking fields, such as phone numbers, email addresses, PAN, and Aadhaar, are excluded from dashboard visuals.

## Data Quality Considerations
The source data contains quality issues, including duplicate identifiers, missing IDs, and placeholder transaction dates. These issues are addressed during the data-cleaning stage before the data is used for analysis.

## Stage 2: Data Warehouse (Snowflake)
Snowflake was used as the centralized cloud data warehouse for FinSight. The seven source datasets were loaded into structured tables to provide a consistent foundation for data cleaning, modelling, and analysis.

## Warehouse Setup
The project uses the following Snowflake environment:

| Component | Configuration |
|---|---|
| Database | `FINANCE_DB` |
| Schema | `SALES_SCHEMA` |
| Virtual Warehouse | `BI_WH` |
| Warehouse Size | X-Small |
| Auto-Suspend | 60 seconds |

## Implementation
The data warehouse setup involved the following steps:
1. **Environment setup:** Created the virtual warehouse, database, and schema.
2. **Table creation:** Created seven tables corresponding to the source banking datasets.
3. **Data staging:** Configured a CSV file format and stage to prepare the source files for loading.
4. **Data loading:** Loaded the CSV data into the corresponding Snowflake tables.
5. **Initial validation:** Used SQL queries to check row counts, inspect records, and examine relationships between tables.

## Data Integrity Considerations
* Primary and foreign key constraints were defined as part of the table structure.
* However, Snowflake does not enforce standard primary and foreign key constraints, so additional data quality checks were required to identify duplicate identifiers and other inconsistencies.
* The raw data was retained in Snowflake, while subsequent cleaning and transformation were performed in Power Query.

## Outcome
All seven banking datasets were available in a centralized warehouse, providing a structured source for the downstream analytics workflow.

## Stage 3: Data Import and Cleaning (Power BI)

### 3.1 Connecting Snowflake to Power BI
* The seven banking tables stored in Snowflake were connected to Power BI Desktop for further data preparation, modelling, and analysis.
* The connection was configured using the following Snowflake details:

| Configuration | Value |
|---|---|
| Database | `FINANCE_DB` |
| Schema | `SALES_SCHEMA` |
| Warehouse | `BI_WH` |
| Storage Mode | Import |

### 3.2 Why Import Mode Was Selected
Power BI offers different connectivity modes, including Import and DirectQuery. For FinSight, **Import mode** was selected because the project focuses on historical banking analysis rather than real-time transaction monitoring.

The main reasons for this choice were:
- **Faster report interactions:** Data is stored in Power BI's in-memory VertiPaq engine, enabling responsive filtering and visual interactions.
- **Reduced dependency on live queries:** Once the data is imported, report interactions do not need to send a new query to Snowflake for every slicer selection.
- **Flexible data transformation:** Power Query transformations and the project's DAX calculations can be applied within the imported model.
- **Controlled warehouse usage:** Snowflake compute is generally needed during data retrieval and refresh, rather than for every report interaction.
- **Suitable for historical analysis:** The dashboard analyses existing banking data, so continuously querying the source is not necessary.
- **Trade-off:** Imported data represents a snapshot of the source at the last successful refresh. New or updated source records will not appear until the dataset is refreshed.

DirectQuery may be more suitable for use cases requiring fresher source data, although its performance and capabilities depend on the source, query design, and report requirements.

### 3.3 Data Cleaning Using Power Query Editor
* After connecting to Snowflake, the banking tables were loaded into Power Query Editor for data preparation.
* The cleaning process addressed duplicate identifiers, missing values, and invalid placeholder dates while retaining records that remained useful for analysis.

| Data quality issue | Transformation applied | Outcome |
|------------|--------------|--------------|------------|
| Duplicate `Transaction_ID` and `Customer_ID` records | Removed duplicates using the relevant identifiers | Retained 1,000,000 transactions and 10,000 customers |
| Missing identifiers, such as `Branch_ID` and `Account_ID` | Replaced blank identifiers with `NA` | Preserved records with incomplete relationship information |
| Placeholder transaction date (`1899-12-31`) | Replaced the placeholder with `null` | Prevented invalid dates from being treated as genuine transaction dates |

### 3.4 Repeatable Transformations
Power Query records each transformation as an applied step. These steps can be executed again when the data is refreshed, making the preparation process repeatable.
The raw data remains in Snowflake, while the cleaning logic is applied in Power BI during data preparation.

### 3.5 Data Quality Considerations
* Records with missing account or branch identifiers may not be included in analyses filtered by those dimensions.
* Transactions without valid dates remain in overall totals but cannot be assigned to a specific year.
* Cleaning and validation are necessary to ensure that dashboard metrics reflect the intended business definitions.

## Outcome
The banking tables were imported into Power BI using Import mode and prepared through Power Query Editor, creating a consistent foundation for the data model and Executive Overview dashboard.

## Stage 4: Data Model (Power BI)
After data preparation, the cleaned banking tables were modelled in Power BI to establish relationships between transactions, customers, accounts, branches, loans, and credit cards.

### 4.1 Data Model Design
The model follows a galaxy schema (fact constellation) design, in which multiple fact tables share relevant dimensions.

* Fact tables: TRANSACTIONS, LOANS, CREDIT_CARDS
* Dimension tables: CUSTOMERS, ACCOUNTS, BRANCHES, EMPLOYEES, Calendar
* Helper tables: Measures1, Fraud Summary, Metric Selector

The fact tables represent different business processes and operate at different levels of detail. The shared dimensions support analysis across the relevant banking activities.

### 4.2 Relationships
* Relationships were established between the tables using their relevant identifiers.
* The model uses active, many-to-one, single-direction relationships to control how filters propagate through the model and reduce ambiguity.
* The complete relationship list and design decisions are documented in docs/data-model.md.

### 4.3 Calendar Table
* A dedicated Calendar table was created to support time-based analysis.
* Daily date coverage from 2005 to 2026.
* Relationship with TRANSACTIONS through TRANSACTION_DATE.
* Relationship with LOANS through LOAN_START_DATE.
* Supports year-based filtering and time-related analysis.
* Customer and credit card measures are not directly linked to the Calendar table, so their values do not change when the Year slicer is selected.

### 4.4 DAX Measures
* DAX measures were created to calculate the KPIs used throughout the dashboard.
* These include transaction counts and values, transaction success rate, fraud metrics, total loan amounts, outstanding loans, and credit card outstanding balances.
* Centralising KPI calculations helps maintain consistent definitions across the dashboard and the one-click summary feature.
Outcome: A structured Power BI model that supports consistent calculations and interactive analysis of banking performance.

## Stage 5: Executive Overview Dashboard
The Executive Overview is the first dashboard developed for FinSight. It consolidates key banking performance and risk indicators into a single interactive report page.

### 5.1 Key Performance Indicators
The dashboard presents eight headline KPIs:
KPI- Purpose
* Total Customers- Shows the customer base represented in the dataset
* Total Transactions- Measures transaction volume
* Total Transaction Value- Measures the monetary value of transactions
* Success Rate- Indicates the proportion of successful transactions
* Fraud Amount- Measures the value attributed to fraudulent transactions
* Total Loan Amount- Shows the total loan portfolio amount
* Outstanding Loans- Shows the amount of loans still outstanding
* Credit Card Outstanding- Shows outstanding credit card balances

### 5.2 Interactive Filters
Four slicers allow users to explore the available data:

* Year: Analyses transactions and loans across the supported date periods.
* Region: Enables geographical comparisons.
* Branch Category: Supports analysis by branch classification.
* Account Type: Allows transaction analysis by account category.

Filter behaviour depends on the relationships between the tables and the availability of data for the selected period.

### 5.3 Visualizations

Business question- Visualization
* How reliably are transactions processed? - Transactions by status
* How does transaction value change over time? - Monthly transaction value trend
* Which transaction types contribute to transaction value? - Transaction value by transaction type
* Which payment channels contribute to transaction value? - Transaction value by payment channel
* How does transaction value vary geographically? - Branch revenue by state
* Which branches contribute the most transaction value? - Transaction value by branch
* How does fraud rate vary over time? - Fraud rate by year

1. TRANSACTION PROCESSING: HIGH SUCCESS RATE
Total transactions
1M
Success rate
90%

What the data shows
* Approximately 90% of transactions are successful, while the dashboard shows a failed transaction segment of around 5%.
Why this matters
*The bank can monitor transaction reliability and investigate the causes of failed, pending, and reversed transactions to identify opportunities to improve the customer experience.
Business takeaway
Track transaction success alongside failure and reversal rates to identify processing issues and prioritise further investigation.

## Future Enhancements
1. Customer segmentation and transaction risk analysis.
2. Detailed loan portfolio and credit card utilisation analysis.
3. Branch and employee performance analysis.
4. Month-over-month and year-over-year comparisons, moving averages, and trend forecasting.
