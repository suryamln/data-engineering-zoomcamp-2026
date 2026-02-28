# Module 5 Homework: Data Platforms with Bruin

---

## Homework Solutions Guide

### Question 1. Understanding Bruin's Project Layout

**Goal:** Grasp the essential files needed to structure a project in Bruin.

**Correct Choice:** `.bruin.yml` and `pipeline.yml` (assets can be anywhere)

**Explanation**  Bruin offers a lot of flexibility in project design. The .bruin.yml file is mandatory as it marks the project's root directory and holds environment settings (like connection details for DuckDB). The pipeline.yml file is equally important because it defines the pipeline's overall structure. You are free to organize your SQL and Python asset files in any folder structure you like, as long as each file's frontmatter correctly associates it with the pipeline.

---

### Question 2. Choosing the Right Materialization Method

**Goal:** Pick the appropriate incremental loading technique for datasets organized by time.

```yaml
# Example asset frontmatter
materialization:
  type: incremental
  strategy: time_interval
  time_column: pickup_datetime
```

**Correct Choice:** `time_interval - incremental based on a time column`

**Explaination** For datasets like monthly taxi records, the time_interval strategy is ideal. It allows the pipeline to safely replace data for a specific time window (e.g., reprocessing only the records for January 2020) without wiping out the entire table or creating duplicate entries.

---

### Question 3. Modifying Variables from the Command Line

**Goal:** Learn the correct syntax to temporarily change a pipeline variable during execution.

```bash
# Overriding the default array variable during execution
bruin run --var 'taxi_types=["yellow"]'

```

**Correct Choice:** `bruin run --var 'taxi_types=["yellow"]'`

**Explanation** Because the taxi_types variable is originally defined as an array in the pipeline's configuration file, any command-line override using the --var flag must be formatted as a valid JSON-style array string.

---

### Question 4. Executing an Asset and Its Dependents

**Goal:** Run a specific task and automatically trigger all tasks that rely on its output.

```bash
# Running an asset and everything that depends on it
bruin run ingestion/trips.py --downstream

```

**Correct Choice:** `bruin run ingestion/trips.py --downstream`

**Explanation** After updating the main data ingestion script (trips.py), you need to refresh all subsequent transformation steps that use this data. The --downstream flag in Bruin ensures this chain reaction, rebuilding the ingestion task and every asset that depends on it.

---

### Question 5. Enforcing Data Quality Rules

**Goal:** Add a validation rule directly into an asset's definition to ensure data integrity.

```yaml
# Adding a quality check in the asset frontmatter
columns:
  - name: pickup_datetime
    type: timestamp
    checks:
      - name: not_null
```

**Correct Choice:** `name: not_null`

**Explanation?** Bruin treats data quality as a core feature. Adding a not_null check on the pickup_datetime column means the pipeline will flag an error or halt if any records are missing this essential timestamp, preventing incomplete data from affecting downstream processes.


---

### Question 6. Visualizing Data Lineage

**Goal:** Use Bruin's built-in tool to see the dependency graph of your pipeline.

```bash
# Generating the dependency graph
bruin lineage

```

**Correct Choice:** `bruin lineage`

**Explanation** The bruin lineage command is a key tool for understanding and troubleshooting your pipeline. It generates a visual graph showing how different assets are interconnected based on their defined references.


---

### Question 7. Running a Pipeline from Scratch

**Goal:** Execute the entire pipeline as if on a fresh, empty database..

```bash
# Running the pipeline from scratch
bruin run --full-refresh

```

**Correct Choice:** `--full-refresh`

**Explanation** When you are setting up a new DuckDB instance or have made significant structural changes, you need to bypass any incremental logic. The --full-refresh flag instructs Bruin to drop existing tables and views and rebuild the entire pipeline from the ground up.

---
