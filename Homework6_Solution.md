# 📊 Dataset

NYC Yellow Taxi dataset – **November 2025**

---

```

# 🧠 Homework Solutions

## Question 1 – Install Spark and PySpark

with guide in zoomcamp, spark successfully installed and verified.
then for check version with
```
uv run python -c "import pyspark; print(pyspark.__version__)"
```
Result:
Spark version: 4.1.1

```
**Explanation** : This command imports the PySpark library and prints its version number. The output confirms that Spark version 4.1.1 is successfully installed and accessible from the Python environment.

---

## Question 2 – Yellow November 2025

```
check size with
```bash
ls -lh yellow_2025_11_repartitioned
```
Average file size:
```
~25 MB
```

✅ **Answer:** 25MB

**Explanation** : Repartitioning distributes data across multiple files for parallel processing. The 25 MB average file size represents a balanced partition strategy—small enough for efficient processing but large enough to avoid excessive metadata overhead. Parquet format compresses data efficiently, resulting in these compact file sizes.

---

## Question 3 – Count Records

Number of taxi trips that **started on 15 November 2025**:
running on cmd:
```bash
uv run python count_taxi_trips_15_nov_2025.py
```
✅ **Answer:**
```
162,604
```
**Explanation** : This query demonstrates Spark's ability to filter large datasets efficiently. By applying a date filter on the pickup timestamp column, Spark scans only relevant partitions (if the data is partitioned by date) or uses predicate pushdown to minimize data reading, returning the exact count of trips that began on this specific date.

---

## Question 4 – Longest Trip

Longest trip duration in the dataset:
running on cmd:
```bash
uv run python longest_trips.py
```
✅ **Answer:**
```
90.6 hours
```
**Explanation** :  This calculation uses Spark's datetime functions to compute the time difference between pickup and dropoff timestamps. The result of 90.6 hours (approximately 3.8 days) represents an unusually long taxi trip, possibly indicating data anomalies, multi-day rentals, or trips outside typical city boundaries.

---

## Question 5 – Spark User Interface

Spark Web UI runs on:

✅ **Answer:**
```
http://localhost:4040
```
**Explanation** : When Spark applications run, they automatically start a web UI on port 4040 (or the next available port if 4040 is occupied). This interface provides real-time monitoring of jobs, stages, tasks, storage memory, environment variables, and executor performance—essential for debugging and optimizing Spark applications.

---

## Question 6 – Least Frequent Pickup Zone

Using Spark SQL join with taxi zone lookup.
running on cmd:
```bash
uv run python Least_frequent_pickup_zone.py
```
✅ **Answer:**

```
Governor's Island/Ellis Island/Liberty Island
```
**Explanation** :   This analysis combines two datasets using Spark SQL—trip records and zone metadata. By aggregating trips per pickup location and sorting ascending, we identify the least utilized zones. These islands have limited vehicle access (ferry-only), naturally resulting in very few taxi pickups, making them statistically the least frequent pickup locations.


---





