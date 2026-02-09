## Question 1:
What is count of records for the 2024 Yellow Taxi Data?
- 65,623
- 840,402
- **20,332,093**
- 85,431,289

**Query:**
```sql
SELECT COUNT(*) FROM `de-zoomcamp-2026-484615.nytaxi.yellow_tripdata_2024_homework`;
```
**Answer:** `20,332,093`

## Question 2:
Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables.
What is the **estimated amount** of data that will be read when this query is executed on the External Table and the Table?

- 18.82 MB for the External Table and 47.60 MB for the Materialized Table
- **0 MB for the External Table and 155.12 MB for the Materialized Table**
- 2.14 GB for the External Table and 0MB for the Materialized Table
- 0 MB for the External Table and 0MB for the Materialized Table

**Queries:**
```sql
-- External Table
SELECT COUNT(DISTINCT(PULocationID)) FROM `de-zoomcamp-2026-484615.nytaxi.external_yellow_tripdata_homework`;

-- Materialized Table
SELECT COUNT(DISTINCT(PULocationID)) FROM `de-zoomcamp-2026-484615.nytaxi.yellow_tripdata_2024_homework`;
```

**Answer:** `0 MB for the External Table and 155.12 MB for the Materialized Table`

**Explanation:** BigQuery shows an estimated 0 MB for External Tables because it does not scan external data during the query planning phase. The amount of data to be processed can only be determined at query execution time, when BigQuery reads the data directly from the external source, such as Google Cloud Storage or other external systems.
In contrast, for internal (materialized) tables, BigQuery can provide a more accurate data processing estimate because it relies on pre-calculated metadata, including table size and statistical information that is stored within BigQuery.


## Question 3:
Write a query to retrieve the PULocationID from the table (not the external table) in BigQuery. Now write a query to retrieve the PULocationID and DOLocationID on the same table. Why are the estimated number of Bytes different?

- **BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.**
- BigQuery duplicates data across multiple storage partitions, so selecting two columns instead of one requires scanning the table twice, doubling the estimated bytes processed.
- BigQuery automatically caches the first queried column, so adding a second column increases processing time but does not affect the estimated bytes scanned.
- When selecting multiple columns, BigQuery performs an implicit join operation between them, increasing the estimated bytes processed

**Queries:**
```sql
-- Scanning only PULocationID
SELECT PULocationID FROM `de-zoomcamp-2026-484615.nytaxi.yellow_tripdata_2024_homework`;

-- Scanning PULocationID and DOLocationID
SELECT PULocationID, DOLocationID FROM `de-zoomcamp-2026-484615.nytaxi.yellow_tripdata_2024_homework`;
```

**Answer:** BigQuery is a columnar database.

## Question 4:
How many records have a fare_amount of 0?
- 128,210
- 546,578
- 20,188,016
- **8,333**

**Query:**
```sql
SELECT COUNT(*) FROM `de-zoomcamp-2026-484615.nytaxi.yellow_tripdata_2024_homework`
WHERE fare_amount = 0;
```
**Answer:** `8,333`

## Question 5:
What is the best strategy to make an optimized table in Big Query if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID (Create a new table with this strategy)
- **Partition by tpep_dropoff_datetime and Cluster on VendorID**
- Cluster on by tpep_dropoff_datetime and Cluster on VendorID
- Cluster on tpep_dropoff_datetime Partition by VendorID
- Partition by tpep_dropoff_datetime and Partition by VendorID

**Strategy:**
Partitioning on the date column (`tpep_dropoff_datetime`) allows BigQuery to skip scanning data from unrequested days. Clustering on `VendorID` sorts the data within those partitions, speeding up ordering and filtering by that column.

**SQL:**
```sql
CREATE OR REPLACE TABLE `de-zoomcamp-2026-484615.nytaxi.yellow_tripdata_2024_partitioned_clustered_homework`
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID 
AS SELECT * FROM `de-zoomcamp-2026-484615.nytaxi.external_yellow_tripdata_homework`;
```
**Answer:** `Partition by tpep_dropoff_datetime and Cluster on VendorID`

## Question 6:
Write a query to retrieve the distinct VendorIDs between tpep_dropoff_datetime 2024-03-01 and 2024-03-15 (inclusive).
Use the materialized table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 5 and note the estimated bytes processed. What are these values?

Choose the answer which most closely matches.

- 12.47 MB for non-partitioned table and 326.42 MB for the partitioned table
- **310.24 MB for non-partitioned table and 26.84 MB for the partitioned table**
- 5.87 MB for non-partitioned table and 0 MB for the partitioned table
- 310.31 MB for non-partitioned table and 285.64 MB for the partitioned table

**Queries:**
```sql
-- Normal Table
SELECT DISTINCT(VendorID) FROM `de-zoomcamp-2026-484615.nytaxi.yellow_tripdata_2024_homework`
WHERE DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-15';

-- Partitioned & Clustered Table
SELECT DISTINCT(VendorID) FROM `de-zoomcamp-2026-484615.nytaxi.yellow_tripdata_2024_partitioned_clustered_homework`
WHERE DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-15';
```

**Answer:** `310.24 MB for non-partitioned table and 26.84 MB for the partitioned table`

## Question 7: 
Where is the data stored in the External Table you created?
- Big Query
- Container Registry
- **GCP Bucket**
- Big Table

**Answer:** `GCP Bucket`

**Reasoning:** External tables reference data that is stored outside of BigQuery’s managed internal storage. 
In this case, the underlying data resides in Google Cloud Storage (GCS), meaning BigQuery does not physically store or materialize the data. 
Instead, it accesses the files in GCS on demand at query execution time, allowing users to query external data without loading it into BigQuery while still using standard SQL.

## Question 8:
It is best practice in Big Query to always cluster your data:
- True
- **False**

**Answer:** `False`

**Reasoning:** Clustering is most effective for large tables where filtering is frequent. For very small tables (typically under 50 MB), clustering might not provide noticeable benefits and can add complexity.


## (Bonus: Not worth points) Question 9:
Write a `SELECT count(*)` query FROM the materialized table you created. How many bytes does it estimate will be read? Why?

**Query:**
```sql
SELECT COUNT(*) FROM `de-zoomcamp-2026-484615.nytaxi.yellow_tripdata_2024_homework`;
```

**Estimate:** `0 bytes`

**Reasoning:** 
BigQuery stores the total row count of native (materialized) tables as part of its table metadata. 
When executing a simple COUNT(*) query without any filters or conditions, BigQuery retrieves this value directly from the metadata, rather than scanning the underlying data columns. 
As a result, the query does not require reading any table data and therefore reports 0 bytes processed, making the operation extremely fast and cost-efficient.


