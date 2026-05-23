# Cloud-Based Energy Consumption Analytics using AWS S3, Snowflake & Tableau

## Project Overview

This project demonstrates an end-to-end cloud analytics workflow using AWS S3, Snowflake SQL, and Tableau for analyzing household energy consumption patterns across different countries and regions.

The project includes cloud storage integration, data warehousing, SQL-based data transformation, and interactive dashboard visualization.

---

## Technologies Used

- AWS S3
- Snowflake
- SQL
- Tableau

---

## Project Workflow

CSV Dataset → AWS S3 → Snowflake External Stage → SQL Transformations → Tableau Dashboard

---

## Dataset Description

The dataset contains household energy consumption data including:

- Household ID
- Region
- Country
- Energy Source
- Monthly Usage (KWH)
- Household Size
- Income Level
- Urban/Rural Classification
- Adoption Year
- Subsidy Status
- Cost Savings

---

## Snowflake Integration

The dataset was uploaded to AWS S3 and integrated with Snowflake using:

- Storage Integration
- IAM Role Configuration
- External Stages

### Storage Integration SQL

```sql
CREATE OR REPLACE STORAGE INTEGRATION tableau_Integration
TYPE = EXTERNAL_STAGE
STORAGE_PROVIDER = 'S3'
ENABLED = TRUE
STORAGE_AWS_ROLE_ARN = 'arn:aws:iam::195969197650:role/tableau.role'
STORAGE_ALLOWED_LOCATIONS = ('s3://tableau4.project/')
COMMENT = 'Optional Comment';
```

---

## Data Loading using Snowflake

```sql
COPY INTO tableau_dataset
FROM @tableau_stage
FILE_FORMAT = (TYPE = CSV FIELD_DELIMITER = ',' SKIP_HEADER = 1)
ON_ERROR = 'CONTINUE';
```

---

## Data Transformation using SQL

### Monthly Usage Updates

```sql
UPDATE energy_consumption
SET monthly_usage_kwh = monthly_usage_kwh * 1.1
WHERE income_level = 'Low';

UPDATE energy_consumption
SET monthly_usage_kwh = monthly_usage_kwh * 1.2
WHERE income_level = 'Middle';

UPDATE energy_consumption
SET monthly_usage_kwh = monthly_usage_kwh * 1.3
WHERE income_level = 'High';
```

### Cost Savings Updates

```sql
UPDATE energy_consumption
SET cost_savings_usd = cost_savings_usd * 0.9
WHERE income_level = 'Low';

UPDATE energy_consumption
SET cost_savings_usd = cost_savings_usd * 0.8
WHERE income_level = 'Middle';

UPDATE energy_consumption
SET cost_savings_usd = cost_savings_usd * 0.7
WHERE income_level = 'High';
```

---

## Analytical SQL Queries

### Region-wise Energy Consumption

```sql
SELECT
    region,
    SUM(monthly_usage_kwh) AS total_usage
FROM energy_consumption
GROUP BY region
ORDER BY total_usage DESC;
```

### Energy Source Analysis

```sql
SELECT
    energy_source,
    AVG(cost_savings_usd) AS avg_savings
FROM energy_consumption
GROUP BY energy_source;
```

---

## Tableau Dashboard Features

The Tableau dashboard provides visual analysis for:

- KWH by Country
- KWH by Region
- KWH by Energy Source
- Cost Savings by Region
- Cost Savings by Energy Source

Project screenshots and dashboard previews are available in the `screenshots` folder.

---

## Key Insights

- Wind energy showed the highest energy consumption.
- Europe recorded high regional energy usage.
- Higher-income households consumed more electricity.
- Renewable energy adoption positively impacted cost savings.

---

## Project Structure

```text
dataset/
sql/
screenshots/
tableau/
documentation/
README.md
```

---

## Future Improvements

- Add real-time data integration
- Add KPI cards and filters in Tableau
- Implement advanced SQL analytics
- Integrate predictive analytics using Python

---

## Author

Rithvik Sukhavasi

M.Sc Data Science  
Data Analyst Portfolio Project
