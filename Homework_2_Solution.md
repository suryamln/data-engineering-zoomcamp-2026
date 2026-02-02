# Question 1: Extract Task File Size

### Question
Within the execution for `Yellow` Taxi data for the year `2020` and month `12`: what is the uncompressed file size (i.e. the output file `yellow_tripdata_2020-12.csv` of the `extract` task)?
- **128.3 MiB**
- 134.5 MiB
- 364.7 MiB
- 692.6 MiB

### Solution
To answer this question, the flow `04_postgres_taxi.yaml` was executed with the following parameters:
- `taxi`: yellow
- `year`: 2020
- `month`: 12

After the execution completed successfully, the file size was verified in the **Outputs** tab of the `extract` task in the Kestra UI.

### Answer
The uncompressed file size is **128.3 MiB**.


# Question 2: Rendered Variable Value

### Question
What is the rendered value of the variable `file` when the inputs `taxi` is set to `green`, `year` is set to `2020`, and `month` is set to `04` during execution?
- `{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv`
- **`green_tripdata_2020-04.csv`**
- `green_tripdata_04_2020.csv`
- `green_tripdata_2020.csv`

### Solution
```yaml
variables:
  file: "{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv" # this one
  staging_table: "public.{{inputs.taxi}}_tripdata_staging"
  table: "public.{{inputs.taxi}}_tripdata"
  data: "{{outputs.extract.outputFiles[inputs.taxi ~ '_tripdata_' ~ inputs.year ~ '-' ~ inputs.month ~ '.csv']}}"
```
Based on the given variable configuration (taxi=green, year=2020, month=04), the rendered value from the template {{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv is:

This was determined by analyzing the `variables` section of the Kestra flow and observing the rendered output during an execution with the following inputs:
The variable `file` is defined as:
```yaml
variables:
  file: "{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv"
```
- `{{inputs.taxi}} renders as green`
- `{{inputs.year}} renders as 2020`
- `{{inputs.month}} renders as 04`
- `taxi`: green
- `year`: 2020
- `month`: 04

Substituting the inputs results in: `green_tripdata_2020-04.csv`.
The naming follows the template’s pattern, using underscores (_) and hyphens (-) exactly as structured

### Answer
The rendered value is **`green_tripdata_2020-04.csv`**.

# Question 3: Yellow Taxi 2020 Row Count

### Question
How many rows are there for the `Yellow` Taxi data for all CSV files in the year 2020?
- 13,537,299
- **24,648,499**
- 18,324,219
- 29,430,127

### Solution
1. Executed a **Backfill** for the `Yellow` taxi dataset for the entire year of 2020 using the `05_postgres_taxi_scheduled.yaml` flow.
2. After all 12 months were loaded into the Postgres database, the following query flow was used to count the total rows:

```yaml
id: cek_data_hasil_3
namespace: zoomcamp

tasks:
  - id: counts_row_yellow_2020
    type: io.kestra.plugin.jdbc.postgresql.Queries
    sql: |
      SELECT COUNT(*) as total 
      FROM public.yellow_tripdata 
      WHERE filename LIKE 'yellow_tripdata_2020%';
    fetchType: FETCH_ONE

  - id: log_hasil
    type: io.kestra.plugin.core.log.Log
    message: "count raw Yellow Taxi 2020 is: {{ outputs.count_row_yellow_2020.row.total }}"

pluginDefaults:
  - type: io.kestra.plugin.jdbc.postgresql
    values:
      url: jdbc:postgresql://postgres:5432/kestra
      username: kestra
      password: k3str4
```

### Answer
The total number of rows for Yellow Taxi data in 2020 is **24,648,499**.

# Question 4: Green Taxi 2020 Row Count

### Question
How many rows are there for the `Green` Taxi data for all CSV files in the year 2020?
- 5,327,301
- 936,199
- **1,734,051**
- 1,342,034

### Solution
1. Executed a **Backfill** for the `Green` taxi dataset for the entire year of 2020 using the `05_postgres_taxi_scheduled.yaml` flow.
2. Used the following kestra flow to query the database and retrieve the count:

```yaml
id: cek_data_hasil
namespace: zoomcamp

tasks:
  - id: counts_row_green_2020
    type: io.kestra.plugin.jdbc.postgresql.Queries
    sql: |
      SELECT COUNT(*) as total 
      FROM public.green_tripdata 
      WHERE filename LIKE 'green_tripdata_2020%';
    fetchType: FETCH_ONE

  - id: log_hasil
    type: io.kestra.plugin.core.log.Log
    message: "count raw Green Taxi 2020 adalah: {{ outputs.count_raw_green_2020.row.total }}"

pluginDefaults:
  - type: io.kestra.plugin.jdbc.postgresql
    values:
      url: jdbc:postgresql://postgres:5432/kestra
      username: kestra
      password: k3str4
```

### Answer
The total number of rows for Green Taxi data in 2020 is **1,734,051**.

# Question 5: Yellow Taxi March 2021 Row Count

### Question
How many rows are there for the `Yellow` Taxi data for the March 2021 CSV file?
- 1,428,092
- 706,911
- **1,925,152**
- 2,561,031

### Solution
1. Executed the flow manually for Yellow Taxi, Year 2021, and Month 03.
2. Used the following query flow to check the row count in the database:

```yaml
id: cek_data_hasil_nomor_5
namespace: zoomcamp

tasks:
  - id: count_row_yellow_maret_2021
    type: io.kestra.plugin.jdbc.postgresql.Queries
    sql: |
      SELECT COUNT(*) as total 
      FROM public.yellow_tripdata 
      WHERE filename LIKE 'yellow_tripdata_2021_03%';
    fetchType: FETCH_ONE

  - id: log_hasil
    type: io.kestra.plugin.core.log.Log
    message: "Total baris Yellow Taxi Maret 2021 adalah: {{ outputs.count_raw_yellow_maret_2021.row.total }}"

pluginDefaults:
  - type: io.kestra.plugin.jdbc.postgresql
    values:
      url: jdbc:postgresql://postgres:5432/kestra
      username: kestra
      password: k3str4
```

### Answer
The total number of rows for Yellow Taxi in March 2021 is **1,925,152**.

# Question 6
How would you configure the timezone to New York in a Schedule trigger? (1 point) <br>
a. Add a timezone property set to EST in the Schedule trigger configuration <br>
b. Add a timezone property set to America/New_York in the Schedule trigger configuration <br>
c. Add a timezone property set to UTC-5 in the Schedule trigger configuration <br>
d. Add a location property set to New_York in the Schedule trigger configuration <br>

# Answer :
To answer this, i just look at the documentation link here: 
https://kestra.io/docs/workflow-components/triggers/schedule-trigger

The page give the example as below:

```yaml
triggers:
  - id: daily
    type: io.kestra.plugin.core.trigger.Schedule
    cron: "@daily"
    timezone: America/New_York
```
so, it is clear that <b> the answer is (b) Add a timezone property set to America/New_York in the Schedule trigger configuration </b>









