# Customer Churn Management

An end-to-end Big Data and AI-driven banking customer churn analysis and prediction system.

## Project Overview

Customer churn is a major challenge in the banking sector because losing existing customers can lead to revenue loss, reduced customer lifetime value, and increased customer acquisition costs.

This project analyzes customer demographic information, product usage, marketing offers, and customer support interactions to understand customer behavior and identify patterns associated with churn.

The project demonstrates a complete:

**Data Engineering → Big Data Processing → Data Modeling → Machine Learning → Business Intelligence**

pipeline.

## Business Problem

Banks often have customer information distributed across multiple systems. Customer profiles, product usage, marketing offers, and customer support interactions may exist in separate datasets, making it difficult to build a complete view of customer behavior.

The main objective of this project is to integrate these sources into a unified **Customer 360 View** and use the resulting data to answer questions such as:

- Which customer behaviors are associated with churn?
- Does product usage relate to customer retention?
- Is customer support friction associated with churn?
- Are customers who engage with offers less likely to churn?
- Which customers have higher relative business value?
- Can the bank identify customers who may require retention attention?

The final goal is to support more data-driven customer retention decisions.

## Why the Banking Sector?

The banking sector was selected because banks interact with customers through many products and services, including accounts, cards, loans, offers, and customer support.

These interactions generate different types of data that can be integrated to create a more complete understanding of each customer.

The sector also provides a strong business case for churn analysis because retaining an existing customer is generally more valuable than reacting after the customer has already left.

## Dataset

The project uses four related datasets:

### 1. Customer

Contains the main customer profile and churn information.

Important attributes include:

- Customer ID
- Credit score
- Geography
- Gender
- Age
- Tenure
- Number of products
- Credit card ownership
- Active membership status
- Estimated salary
- Churn status

The main target variable is `exited`:

- `0` → Customer stayed with the bank
- `1` → Customer exited the bank

Therefore, the Machine Learning task is primarily a **binary classification** problem.

### 2. Customer Usage

Contains information about customer product usage and monthly balances.

This dataset is used to analyze:

- Product engagement
- Number of active products
- Product diversity
- Monthly balance
- Changes in customer activity over time

### 3. Offers

Contains marketing and retention offers provided to customers.

This dataset is used to analyze:

- Number of offers received
- Offer types
- Number of accepted offers
- Offer acceptance rate
- Relationship between offer engagement and churn

### 4. Customer Support Tickets

Contains customer service interactions.

This dataset is used to analyze:

- Number of support tickets
- Issue types
- Issue severity
- Critical tickets
- Resolution time
- Support friction

## Data Architecture

The project follows a layered data architecture:

```text
Source Systems
    |
    |-- Customer Database
    |-- Customer Usage
    |-- Offers
    |-- Customer Support Tickets
    |
    v
Data Ingestion
    |
    |-- Apache Sqoop
    |-- Apache NiFi
    |
    v
HDFS - Raw Layer
    |
    v
PySpark Data Cleaning and Transformation
    |
    v
HDFS - Silver Layer
    |
    v
Dimensional Data Warehouse
    |
    |-- Dim_Customer
    |-- Dim_Date
    |-- Fact_Customer_Monthly_Activity
    |
    v
Hive External Tables
    |
    v
Business Insights + Dashboard + Machine Learning
```

## Data Engineering Pipeline

The pipeline performs the following tasks:

1. Design the relational banking database.
2. Load the source datasets into MariaDB/MySQL.
3. Ingest structured data using Apache Sqoop.
4. Ingest standalone files using Apache NiFi.
5. Store raw data in HDFS.
6. Validate schemas and data types.
7. Handle missing values using business rules.
8. Detect and remove duplicates where required.
9. Correct invalid and inconsistent values.
10. Standardize date formats.
11. Validate referential integrity.
12. Transform the cleaned data using PySpark.
13. Store the Silver Layer in Parquet format.
14. Build the dimensional data warehouse.
15. Register the warehouse tables in Apache Hive.
16. Generate business insights.
17. Prepare features for Machine Learning.
18. Present the results through a BI dashboard.

## Data Modeling

The project uses a customer-centric dimensional model.

### Fact Table

#### `Fact_Customer_Monthly_Activity`

**Grain:** One row represents the aggregated activity of one customer during one month.

Main measures include:

- Offers received count
- Offers accepted count
- Tickets opened count
- Critical tickets count
- Total resolution time
- Ending monthly balance
- Active products count

### Dimension Tables

#### `Dim_Customer`

Contains customer profile information and the churn label.

#### `Dim_Date`

Contains calendar information used for monthly analysis and time-based reporting.

## Business Insights

The project focuses on four main business insights.

### 1. Early Warning Churn Signals

Analyzes changes in customer behavior over time, such as:

- Declining monthly balance
- Reduction in active products
- Changes in product engagement

The purpose is to identify behavioral patterns associated with customers who eventually churn.

### 2. Support Friction vs. Churn

Analyzes whether customer support experience is associated with churn using:

- Number of support tickets
- Number of critical tickets
- Total resolution time
- Average resolution time

Customers are grouped according to support friction levels and their churn rates are compared.

### 3. Marketing Offer Effectiveness

Analyzes the relationship between marketing offer engagement and churn using:

- Total offers received
- Total offers accepted
- Offer acceptance rate

This insight helps evaluate whether customers who engage more with offers show different retention patterns.

### 4. Customer Value Analysis / Simulated LTV

Estimates relative customer value using available information such as:

- Average monthly balance
- Active products
- Customer tenure

Because the datasets do not contain actual revenue or profit data, this project uses a **simulated customer value score** rather than actual Customer Lifetime Value.

## Important Analytical Considerations

The analysis identifies associations and patterns in the dataset. It does not prove that a specific factor directly causes churn.

For example:

- A high offer acceptance rate is associated with lower churn in the analyzed data.
- Support friction does not show a simple direct relationship with churn.
- Higher-value customers show lower churn, but the high-value group is small.
- No single customer behavior is sufficient to explain churn.

These findings should therefore be interpreted together rather than as isolated rules.

## Technology Stack

| Area | Technologies |
|---|---|
| Database | MariaDB / MySQL |
| Data Ingestion | Apache Sqoop, Apache NiFi |
| Distributed Storage | HDFS |
| Big Data Processing | Apache Spark, PySpark |
| Data Format | Parquet |
| Data Warehouse | Dimensional Star Schema |
| SQL Query Engine | Apache Hive |
| Query Interface | Hue |
| Machine Learning | PySpark ML |
| Business Intelligence | BI Dashboard |
| Development Environment | Linux VM, Docker, Jupyter Notebook |

## Project Structure

```text
Customer-Churn-Management/
│
├── README.md
│
├── data/
│   ├── customer/
│   ├── customer_usage/
│   ├── offers/
│   └── tickets/
│
├── database/
│   └── database_schema.sql
│
├── ingestion/
│   ├── sqoop_commands.sh
│   └── nifi_templates/
│
├── spark/
│   └── Customer_Churn.py
│
│
├── ml/
│   └── churn_model.py
│
├── dashboard/
│   └── dashboard_screenshots/
│
└── documentations/
    └── project_documentation.docx
    └── project_presentation.pptx
```

## Example HDFS Layers

```text
/user/student/Capstone_Project/
│
├── customer/
├── customer_usage/
├── offers/
├── tickets/
│
├── silver/
│   ├── customer/
│   ├── customer_usage/
│   ├── offers/
│   └── tickets/
│
└── gold/
    ├── dim_customer/
    ├── dim_date/
    ├── fact_customer_monthly_activity/
    └── insights/
        ├── early_warning_churn/
        ├── support_friction_churn/
        ├── offer_effectiveness/
        └── customer_value_analysis/
```

## How to Run the Project

### 1. Prepare the Environment

Install or configure:

- Hadoop and HDFS
- Apache Spark
- Apache Hive
- MariaDB/MySQL
- Apache Sqoop
- Apache NiFi
- Python
- PySpark

### 2. Create the Source Database

Create the banking database and its related tables in MariaDB/MySQL.

Load the source datasets into the appropriate tables.

### 3. Ingest the Data

Use Apache Sqoop to import structured database tables into HDFS.

Use Apache NiFi to ingest standalone files and convert them into Parquet where required.

### 4. Run the PySpark Pipeline

Execute the main Spark script:

```bash
spark-submit Customer_Churn.py
```

The script performs data cleaning, transformation, warehouse preparation, and analytical processing.

### 5. Query the Hive Tables

After the pipeline finishes, the generated Parquet datasets are registered as external Hive tables.

Example:

```sql
SHOW DATABASES;

USE customer_churn_db;

SHOW TABLES;

SELECT *
FROM dim_customer
LIMIT 10;
```

### 6. Analyze the Business Insights

Query the generated insight tables:

```sql
SELECT *
FROM insight_early_warning_churn
LIMIT 10;

SELECT *
FROM insight_support_friction_churn
LIMIT 10;

SELECT *
FROM insight_offer_effectiveness
LIMIT 10;

SELECT *
FROM insight_customer_value
LIMIT 10;
```

### 7. View the Dashboard

The curated warehouse and insight tables are used as the source for the BI dashboard.

The dashboard presents the main churn patterns and supports business-oriented analysis.

## Results Summary

The analysis produced the following main findings:

- Customer behavior indicators alone were not strong enough to explain churn.
- Medium support friction showed the highest churn rate in the analyzed dataset.
- High offer acceptance was associated with a lower churn rate.
- Higher-value customers showed lower churn, although the high-value group was very small.
- Combining multiple customer signals is more useful than relying on a single indicator.

## Team

- Sara Mohamed
- Ahmed Mohamed
- Adham Mohamed
- Youssef Khaled

## Disclaimer

This project is developed for educational and training purposes as part of the Samsung Innovation Campus Big Data course.

The business insights represent associations observed in the available datasets and should not be interpreted as direct causal conclusions.
