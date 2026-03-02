# Module — dlt Workshop: Build Your Own Pipeline

## Overview

This repository contains my solutions for the DLT Workshop from the DataTalksClub Zoomcamp.
Throughout this module

The exercises focused on querying the Yellow Taxi dataset loaded through the `taxi_pipeline`.

---

## Technologies Used

- **dlt**: Used for data ingestion, schema creation, and managing incremental loads.
- **DuckDB / BigQuery**  Served as the destination data warehouse.
- **dlt Dashboard**: Enabled exploration of pipeline runs and loaded tables.
- **dlt MCP Server**: To ask questions about the pipeline programmatically.
- **Marimo Notebook**: For running SQL queries and building visualizations.

---

## Homework Solutions

### Question 1. What is the start date and end date of the dataset?

**Objective:** Identify the earliest and latest timestamps in the loaded Yellow Taxi data.

Using tools such as the **dlt Dashboard**, the **MCP agent**, or a **Marimo SQL query** like:

```sql
SELECT
    MIN(tpep_pickup_datetime) AS start_date,
    MAX(tpep_dropoff_datetime) AS end_date
FROM trips;
```

> **Answer:** `2009-01-01 to 2009-01-31`

**Explanation:**
The workshop pipeline is specifically configured to load the Yellow Taxi dataset for January 2009, which results in this exact date range.

---

### Question 2. What proportion of trips are paid with credit card?

**Objective:** Calculate the share of credit card payments in the loaded data.

Example query used in the notebook:

```sql
SELECT
    payment_type,
    COUNT(*) AS trips,
    COUNT(*) * 1.0 / SUM(COUNT(*)) OVER() AS proportion
FROM trips
GROUP BY payment_type;
```

> **Answer:** `36.66%`

**Explanation:**
The analysis of the loaded data reveals that credit card payments account for approximately one-third of all trips in this dataset.

---

### Question 3. What is the total amount of money generated in tips?

**Objective:** Sum all tip amounts for the dataset.

Sample query:

```sql
SELECT SUM(tip_amount) AS total_tips
FROM trips;
```

> **Answer:** `$8,063.41`

**Explanation:**
The aggregated value of tips across all trips from January 2009 totals roughly 8,063 USD.

---

## Data Investigation Notes

Throughout the workshop, I experimented with:

1. **dlt Dashboard**
   - Used the command dlt pipeline taxi_pipeline show to inspect table schemas and verify row counts.

2. **MCP Server**
   - Asked questions like _“How many trips used credit cards?”_ and received instant answers.

3. **Marimo Notebook**
   - Queried the data warehouse directly and created small visualizations to examine distributions, like trip distances and payment type frequencies.

---

## Principal Lessons

- **Unified Tooling:** dlt provides a cohesive framework that merges data intake, schema development, and oversight.
- **Adaptable Examination:**The availability of three distinct channels for data review (Dashboard, MCP Agent, and Notebook) made verification considerably easier.
- **Pipeline Simplicity:** The workshop clearly showed how minimal coding effort is necessary to construct dependable, repeatable data intake workflows
- **Straight Forward Findings:** Once the pipeline was properly configured, simple SQL queries proved adequate for retrieving the information needed to address all assignment questions.