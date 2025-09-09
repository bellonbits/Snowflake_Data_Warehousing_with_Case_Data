# Beginner Lab: Snowflake Data Warehousing with Case Data

## Objectives

* Create a warehouse, database, and schema in Snowflake.
* Load a simple **health case dataset**.
* Run **basic queries** for analysis.

---

## Step 1: Create Warehouse, Database, and Schema

```sql
-- Create a small warehouse
CREATE OR REPLACE WAREHOUSE my_wh
  WITH WAREHOUSE_SIZE = 'XSMALL'
  AUTO_SUSPEND = 60
  AUTO_RESUME = TRUE;

-- Create a database
CREATE OR REPLACE DATABASE health_db;

-- Create a schema
CREATE OR REPLACE SCHEMA health_schema;
```

---

## Step 2: Create the Table

```sql
CREATE OR REPLACE TABLE health_schema.cases (
    Case_ID STRING,
    Date_Onset DATE,
    County STRING,
    Sex STRING,
    Age INT,
    Occupation STRING,
    Case_Status STRING,
    Clinical_Presentation STRING,
    Lab_Diagnosis STRING,
    Travel_History STRING,
    Travel_Destination STRING
);
```

---

## Step 3: Load Data

1. Instructor gives students a CSV file (e.g., `cases.csv`).
2. Students upload it to Snowflake **via Snowsight** (no CLI needed).

Example COPY command:

```sql
-- Create a file format for CSV
CREATE OR REPLACE FILE FORMAT csv_format
  TYPE = 'CSV'
  SKIP_HEADER = 1;

-- Create a stage
CREATE OR REPLACE STAGE my_stage
  FILE_FORMAT = csv_format;

-- Upload file (done via Snowsight UI)

-- Load data into the table
COPY INTO health_schema.cases
FROM @my_stage/cases.csv;
```

---

## Step 4: Run Simple Queries

```sql
-- View all rows
SELECT * FROM health_schema.cases LIMIT 10;

-- Count total cases
SELECT COUNT(*) AS total_cases FROM health_schema.cases;

-- Count cases by county
SELECT County, COUNT(*) AS cases_in_county
FROM health_schema.cases
GROUP BY County
ORDER BY cases_in_county DESC;

-- Count cases by Sex
SELECT Sex, COUNT(*) AS total
FROM health_schema.cases
GROUP BY Sex;

-- Average age of cases
SELECT AVG(Age) AS avg_age FROM health_schema.cases;

-- Travel history analysis
SELECT Travel_Destination, COUNT(*) AS travelers
FROM health_schema.cases
WHERE Travel_History = 'Yes'
GROUP BY Travel_Destination
ORDER BY travelers DESC;
```

---

## Step 5: Student Task

Ask students to:

1. Find which **County** has the highest number of cases.
2. Find the **most common occupation** among cases.
3. Find the **average age by Sex**.
4. Identify how many **PCR Positive cases** exist.

---

## Deliverables

Each student submits:

* Screenshots of their **table data**.
* Results of their queries (top 2–3 findings).
* A short reflection: *“What insight did I discover from the data?”*

---
