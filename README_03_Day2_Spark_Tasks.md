# Day 2 Spark Tasks  
## London Transport Data Engineering Project

Welcome to the **Spark task file for Day 2**.

In this file, you will build the **Spark version** of the London Transport Data Engineering Project.

On Day 1, you solved the business problem locally using:

- Python
- PostgreSQL
- ETL
- ELT

On **Day 2**, you will solve the **same business problem** again, but now using **Spark**.

That is important because this helps you understand how the same data engineering logic can move into a more scalable processing framework.

This task file is fully guided on purpose.

You are not expected to invent the Spark solution by yourself.  
You are expected to follow each step carefully, run the code, observe the result, and learn from the workflow.

---

# 1. Day 2 Spark objective

Your goal in this part is to build a working Spark pipeline that:

- reads the London transport raw files
- creates Spark DataFrames
- inspects schemas
- selects the important source files for the Day 2 reporting output
- cleans and standardizes important fields
- joins the datasets together
- performs aggregations
- writes output files

By the end of this file, you should have a working Spark pipeline and a set of reporting outputs.

---

# 2. Important note before starting

This project contains **10 raw source files**, but for the main Spark reporting workflow today, you will focus mainly on:

- `stations.csv`
- `lines.csv`
- `journeys.json`
- `boroughs.csv`
- `zones.csv`

The other files still belong to the project and are part of the full data environment, but today’s first Spark reporting layer will use the most important subset.

That is realistic.

Real engineering projects often begin with the core sources first and expand later.

---

# 3. What final Spark output are we building today?

Today, your Spark pipeline should help produce outputs that answer business questions such as:

- Which stations are busiest?
- Which lines carry the most passenger traffic?
- Which boroughs show the most activity?
- Which zones appear most often?
- Which lines show the highest average delays?

So today’s Spark work is not random technical experimentation.

It is still solving the same transport analytics business problem.

---

# 4. Spark flow for Day 2

Here is the Spark flow you are building:

```text
Raw source files → Spark DataFrames → Cleaning and selection → Joins → Aggregations → Output files
````

More specifically:

```text
stations.csv
lines.csv
journeys.json
boroughs.csv
zones.csv
      ↓
Spark DataFrames
      ↓
Spark cleaning and transformations
      ↓
Joined transport reporting DataFrame
      ↓
Aggregated reporting outputs
      ↓
data/output/
```

This is your Day 2 Spark workflow.

---

# 5. Step 1 - Create the Spark pipeline file

## Your task

Open or create:

```text
src/spark_pipeline.py
```

This file will contain the guided Spark work for Day 2.

---

# 6. Step 2 - Start with the imports

## Your task

At the top of `src/spark_pipeline.py`, add:

```python
from pathlib import Path
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, trim, initcap, when, sum as spark_sum, avg
```

## Why this matters

These imports prepare the main Spark tools you need:

* `SparkSession` to start Spark
* `col` for column operations
* `trim` for removing extra spaces
* `initcap` for cleaning text capitalization
* `when` for conditional logic
* aggregation functions like `sum` and `avg`

This is the beginning of your Spark workflow.

---

# 7. Step 3 - Define the project paths

## Your task

Still in `src/spark_pipeline.py`, add:

```python
PROJECT_ROOT = Path(__file__).resolve().parent.parent
RAW_DATA_FOLDER = PROJECT_ROOT / "data" / "raw"
OUTPUT_FOLDER = PROJECT_ROOT / "data" / "output"
```

## Why this matters

This gives your script clean project-aware file paths.

That is better than hardcoding confusing full system paths.

It is also more portable and more professional.

---

# 8. Step 4 - Start the Spark session

## Your task

Add this function:

```python
def create_spark_session():
    spark = (
        SparkSession.builder
        .appName("LondonTransportSparkProject")
        .getOrCreate()
    )
    return spark
```

## Why this matters

This function starts Spark and gives your project a clear application name.

That is the standard first step in a Spark project.

---

# 9. Step 5 - Read the raw source files with Spark

## Your task

Add this function below:

```python
def load_dataframes(spark):
    stations_df = (
        spark.read
        .option("header", True)
        .csv(str(RAW_DATA_FOLDER / "stations.csv"))
    )

    lines_df = (
        spark.read
        .option("header", True)
        .csv(str(RAW_DATA_FOLDER / "lines.csv"))
    )

    boroughs_df = (
        spark.read
        .option("header", True)
        .csv(str(RAW_DATA_FOLDER / "boroughs.csv"))
    )

    zones_df = (
        spark.read
        .option("header", True)
        .csv(str(RAW_DATA_FOLDER / "zones.csv"))
    )

    journeys_df = (
        spark.read
        .option("multiline", True)
        .json(str(RAW_DATA_FOLDER / "journeys.json"))
    )

    return stations_df, lines_df, boroughs_df, zones_df, journeys_df
```

## What this does

This function reads the Day 2 source files into Spark DataFrames.

## Why this matters

This is the Spark version of the extraction step.

On Day 1, Python extracted raw files into lists and dictionaries.

Today, Spark reads them into DataFrames.

---

# 10. Step 6 - Inspect the schemas

## Your task

Add this function:

```python
def inspect_dataframes(stations_df, lines_df, boroughs_df, zones_df, journeys_df):
    print("\n=== Stations Schema ===")
    stations_df.printSchema()

    print("\n=== Lines Schema ===")
    lines_df.printSchema()

    print("\n=== Boroughs Schema ===")
    boroughs_df.printSchema()

    print("\n=== Zones Schema ===")
    zones_df.printSchema()

    print("\n=== Journeys Schema ===")
    journeys_df.printSchema()
```

## Why this matters

One of the most important Spark habits is checking the schema before doing transformations.

This helps you understand:

* column names
* data types
* whether Spark interpreted the file as expected

A data engineer should not transform data blindly.

---

# 11. Step 7 - Inspect some sample rows

## Your task

Add another helper function:

```python
def preview_data(stations_df, lines_df, boroughs_df, zones_df, journeys_df):
    print("\n=== Stations Preview ===")
    stations_df.show(5, truncate=False)

    print("\n=== Lines Preview ===")
    lines_df.show(5, truncate=False)

    print("\n=== Boroughs Preview ===")
    boroughs_df.show(5, truncate=False)

    print("\n=== Zones Preview ===")
    zones_df.show(5, truncate=False)

    print("\n=== Journeys Preview ===")
    journeys_df.show(5, truncate=False)
```

## Why this matters

It is important to actually look at the data.

This helps you notice:

* extra spaces
* uppercase/lowercase problems
* missing values
* odd records

That is part of real data engineering work.

---

# 12. Step 8 - Clean the stations DataFrame

## Your task

Add this function:

```python
def clean_stations_df(stations_df):
    cleaned_df = (
        stations_df
        .filter(col("station_id").isNotNull() & (trim(col("station_id")) != ""))
        .dropDuplicates(["station_id"])
        .withColumn("station_id", trim(col("station_id")))
        .withColumn("station_name", initcap(trim(col("station_name"))))
        .withColumn("borough_id", trim(col("borough_id")))
        .withColumn("zone_id", trim(col("zone_id")))
        .withColumn("line_id", trim(col("line_id")))
        .withColumn("station_type", initcap(trim(col("station_type"))))
    )
    return cleaned_df
```

## What this does

This function:

* removes rows with missing station IDs
* removes duplicate station IDs
* trims extra spaces
* standardizes text formatting

## Why this matters

Stations are reference data, so they should be clean before joining.

---

# 13. Step 9 - Clean the lines DataFrame

## Your task

Add this function:

```python
def clean_lines_df(lines_df):
    cleaned_df = (
        lines_df
        .filter(col("line_id").isNotNull() & (trim(col("line_id")) != ""))
        .dropDuplicates(["line_id"])
        .withColumn("line_id", trim(col("line_id")))
        .withColumn("line_name", initcap(trim(col("line_name"))))
        .withColumn("transport_mode", initcap(trim(col("transport_mode"))))
        .withColumn("operator_id", trim(col("operator_id")))
        .withColumn("vehicle_type_id", trim(col("vehicle_type_id")))
    )
    return cleaned_df
```

## Why this matters

The reporting result needs a clean line name and transport mode.

---

# 14. Step 10 - Clean the boroughs DataFrame

## Your task

Add this function:

```python
def clean_boroughs_df(boroughs_df):
    cleaned_df = (
        boroughs_df
        .filter(col("borough_id").isNotNull() & (trim(col("borough_id")) != ""))
        .dropDuplicates(["borough_id"])
        .withColumn("borough_id", trim(col("borough_id")))
        .withColumn("borough_name", initcap(trim(col("borough_name"))))
        .withColumn("region_group", initcap(trim(col("region_group"))))
    )
    return cleaned_df
```

## Why this matters

Borough data gives location context to the report.

---

# 15. Step 11 - Clean the zones DataFrame

## Your task

Add this function:

```python
def clean_zones_df(zones_df):
    cleaned_df = (
        zones_df
        .filter(col("zone_id").isNotNull() & (trim(col("zone_id")) != ""))
        .dropDuplicates(["zone_id"])
        .withColumn("zone_id", trim(col("zone_id")))
        .withColumn("zone_name", initcap(trim(col("zone_name"))))
        .withColumn("fare_group", initcap(trim(col("fare_group"))))
    )
    return cleaned_df
```

## Why this matters

Zone data helps enrich the final reporting view.

---

# 16. Step 12 - Clean the journeys DataFrame

## Your task

Add this function:

```python
def clean_journeys_df(journeys_df):
    cleaned_df = (
        journeys_df
        .filter(col("journey_id").isNotNull() & (trim(col("journey_id")) != ""))
        .filter(col("station_id").isNotNull() & (trim(col("station_id")) != ""))
        .filter(col("line_id").isNotNull() & (trim(col("line_id")) != ""))
        .filter(trim(col("passenger_count")).rlike("^[0-9]+$"))
        .filter(trim(col("delay_minutes")).rlike("^[0-9]+$"))
        .filter(trim(col("journey_date")).rlike("^[0-9]{4}-[0-9]{2}-[0-9]{2}$"))
        .withColumn("journey_id", trim(col("journey_id")))
        .withColumn("station_id", trim(col("station_id")))
        .withColumn("line_id", trim(col("line_id")))
        .withColumn("passenger_count", trim(col("passenger_count")).cast("int"))
        .withColumn("delay_minutes", trim(col("delay_minutes")).cast("int"))
        .withColumn("journey_date", trim(col("journey_date")).cast("date"))
        .withColumn("time_band", initcap(trim(col("time_band"))))
        .withColumn("entry_exit_flag", initcap(trim(col("entry_exit_flag"))))
    )
    return cleaned_df
```

## What this does

This function:

* keeps only rows with valid IDs
* keeps only rows with valid numeric passenger counts
* keeps only rows with valid numeric delay values
* keeps only correctly formatted dates
* casts values into proper data types
* cleans text columns

## Why this matters

Journeys are the main event data in today’s reporting workflow.

---

# 17. Step 13 - Join the cleaned DataFrames

## Your task

Add this function:

```python
def build_transport_report_df(stations_df, lines_df, boroughs_df, zones_df, journeys_df):
    report_df = (
        journeys_df.alias("j")
        .join(stations_df.alias("s"), col("j.station_id") == col("s.station_id"), "inner")
        .join(lines_df.alias("l"), col("j.line_id") == col("l.line_id"), "inner")
        .join(boroughs_df.alias("b"), col("s.borough_id") == col("b.borough_id"), "left")
        .join(zones_df.alias("z"), col("s.zone_id") == col("z.zone_id"), "left")
        .select(
            col("j.journey_id"),
            col("j.journey_date"),
            col("s.station_id"),
            col("s.station_name"),
            col("s.borough_id"),
            col("b.borough_name"),
            col("s.zone_id"),
            col("z.zone_name"),
            col("l.line_id"),
            col("l.line_name"),
            col("l.transport_mode"),
            col("j.passenger_count"),
            col("j.delay_minutes"),
            col("j.time_band"),
            col("j.entry_exit_flag")
        )
    )
    return report_df
```

## Why this matters

This is the central reporting DataFrame of Day 2.

It combines:

* journey activity
* station details
* borough context
* zone context
* line information

This is the Spark version of the reporting logic from Day 1.

---

# 18. Step 14 - Build reporting aggregations

## Your task

Now add functions for useful reporting outputs.

### Top stations by passenger count

```python
def build_top_stations_df(report_df):
    return (
        report_df
        .groupBy("station_name")
        .agg(spark_sum("passenger_count").alias("total_passengers"))
        .orderBy(col("total_passengers").desc())
    )
```

### Average delay by line

```python
def build_line_delay_df(report_df):
    return (
        report_df
        .groupBy("line_name")
        .agg(avg("delay_minutes").alias("avg_delay_minutes"))
        .orderBy(col("avg_delay_minutes").desc())
    )
```

### Passenger activity by borough

```python
def build_borough_passengers_df(report_df):
    return (
        report_df
        .groupBy("borough_name")
        .agg(spark_sum("passenger_count").alias("total_passengers"))
        .orderBy(col("total_passengers").desc())
    )
```

## Why this matters

Spark is not only for reading and joining data.

It is also powerful for producing aggregated reporting outputs.

These are the kinds of summaries that a business team would actually care about.

---

# 19. Step 15 - Write output files

## Your task

Add this helper function:

```python
def write_outputs(report_df, top_stations_df, line_delay_df, borough_passengers_df):
    OUTPUT_FOLDER.mkdir(parents=True, exist_ok=True)

    report_df.coalesce(1).write.mode("overwrite").option("header", True).csv(str(OUTPUT_FOLDER / "transport_report"))
    top_stations_df.coalesce(1).write.mode("overwrite").option("header", True).csv(str(OUTPUT_FOLDER / "top_stations"))
    line_delay_df.coalesce(1).write.mode("overwrite").option("header", True).csv(str(OUTPUT_FOLDER / "line_delay"))
    borough_passengers_df.coalesce(1).write.mode("overwrite").option("header", True).csv(str(OUTPUT_FOLDER / "borough_passengers"))
```

## What this does

This function writes the DataFrames to output folders as CSV.

## Why this matters

A data pipeline should produce outputs, not only intermediate transformations.

This step makes your Day 2 Spark work feel more complete and professional.

---

# 20. Step 16 - Create the main pipeline flow

## Your task

Now add the main function:

```python
def main():
    spark = create_spark_session()

    stations_df, lines_df, boroughs_df, zones_df, journeys_df = load_dataframes(spark)

    inspect_dataframes(stations_df, lines_df, boroughs_df, zones_df, journeys_df)
    preview_data(stations_df, lines_df, boroughs_df, zones_df, journeys_df)

    stations_clean_df = clean_stations_df(stations_df)
    lines_clean_df = clean_lines_df(lines_df)
    boroughs_clean_df = clean_boroughs_df(boroughs_df)
    zones_clean_df = clean_zones_df(zones_df)
    journeys_clean_df = clean_journeys_df(journeys_df)

    report_df = build_transport_report_df(
        stations_clean_df,
        lines_clean_df,
        boroughs_clean_df,
        zones_clean_df,
        journeys_clean_df
    )

    top_stations_df = build_top_stations_df(report_df)
    line_delay_df = build_line_delay_df(report_df)
    borough_passengers_df = build_borough_passengers_df(report_df)

    print("\n=== Final Transport Report Preview ===")
    report_df.show(10, truncate=False)

    print("\n=== Top Stations Preview ===")
    top_stations_df.show(10, truncate=False)

    print("\n=== Line Delay Preview ===")
    line_delay_df.show(10, truncate=False)

    print("\n=== Borough Passengers Preview ===")
    borough_passengers_df.show(10, truncate=False)

    write_outputs(report_df, top_stations_df, line_delay_df, borough_passengers_df)

    spark.stop()

    print("\nSpark pipeline completed successfully.")
```

And add this at the bottom:

```python
if __name__ == "__main__":
    main()
```

## Why this matters

This creates a clean full Spark pipeline from beginning to end.

That is exactly what a well-structured project should have.

---

# 21. Step 17 - Run the Spark pipeline

## Your task

From the project root, run:

```bash
python src/spark_pipeline.py
```

or if needed in your environment:

```bash
python3 src/spark_pipeline.py
```

## What you should expect

If everything is correct, you should see:

* schemas printed
* sample previews
* report previews
* final success message

Something like:

```text
Spark pipeline completed successfully.
```

If you get errors, check:

* whether Spark is available
* whether Java is installed
* whether the raw files are in the correct folder
* whether you are running from the project root

---

# 22. Step 18 - Check the output folder

## Your task

After the pipeline finishes, inspect:

```text
data/output/
```

You should find output folders such as:

* `transport_report`
* `top_stations`
* `line_delay`
* `borough_passengers`

## Why this matters

This confirms that your Spark pipeline produced usable reporting outputs.

---

# 23. Step 19 - Add project notes

## Your task

Open:

```text
docs/project_notes.md
```

and write short notes about:

* which files were used in the Spark workflow
* what transformations were performed
* what joins were performed
* what output files were created
* what felt similar to Day 1
* what felt different from Day 1

## Why this matters

Real engineers document their work.

This helps you explain your project later and makes your repository stronger.

---

# 24. Step 20 - Commit and push your Day 2 progress

## Your task

After completing the Spark work, push your progress.

Example:

```bash
git add .
git commit -m "Complete Day 2 Spark pipeline"
git push
```

## Why this matters

Your public GitHub repository is part of your professional project portfolio.

Keep it updated and organized.

---

# 25. What makes this Spark version realistic

This Spark stage reflects real data engineering habits such as:

* reading raw files into DataFrames
* inspecting schemas
* cleaning and standardizing values
* joining multiple datasets
* performing aggregations
* generating output data for reporting

This is why this stage matters.

It is guided, but it still reflects real engineering workflow.

---

# 26. Compare Day 2 with Day 1

## Your task

Pause and compare the approaches.

### Day 1

You solved the problem with:

* local Python
* PostgreSQL
* ETL
* ELT

### Day 2

You solved the same problem with:

* Spark
* Spark DataFrames
* Spark joins
* Spark aggregations
* output files

## Why this matters

This comparison is one of the most important learning goals of Day 2.

You are not just learning Spark syntax.

You are learning how a data engineering solution evolves across tools.

---

# 27. What you should understand after finishing

By the end of this Spark part, you should understand that:

* Spark can read raw structured data into DataFrames
* Spark can clean and transform data in a scalable style
* Spark can join multiple datasets together
* Spark can perform aggregations for reporting
* the business logic from Day 1 can be reused in a Spark workflow

That is the main learning goal of Day 2.

---

# 28. Final Day 2 reminder

At the end of Day 2, you should now have:

* a public Day 2 GitHub repository
* the full Day 2 structure
* the raw data files
* a working Spark script
* reporting output files
* checkpoint answers
* project notes
* multiple commits showing your progress

That is a strong and serious Day 2 achievement.

---

# 29. Final message

Treat this Spark stage like real junior data engineering work.

The goal is not only to make Spark run.

The goal is to understand how the same London transport business problem can be solved using a Spark-based workflow.

That is an important and realistic step in becoming a stronger data engineer.

Now continue to the checkpoint file when you finish:

* [README 04 - Day 2 Checkpoint Answers](./README_04_Day2_Checkpoint_Answers.md)


