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
{{inputs.taxi}} renders as green
{{inputs.year}} renders as 2020
{{inputs.month}} renders as 04
- `taxi`: green
- `year`: 2020
- `month`: 04
The variable `file` is defined as:
```yaml
variables:
  file: "{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv"
```
Substituting the inputs results in: `green_tripdata_2020-04.csv`.
The naming follows the template’s pattern, using underscores (_) and hyphens (-) exactly as structured

### Answer
The rendered value is **`green_tripdata_2020-04.csv`**.
