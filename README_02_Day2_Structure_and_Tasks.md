# Day 2 Structure and Tasks  
## London Transport Data Engineering Project

Welcome to **Day 2** of the London Transport Data Engineering Project.

This document explains:

- the structure of the Day 2 repository
- the technical setup path for Windows and macOS students
- what you must complete today
- how the Spark stage is organized
- what your deliverables are by the end of Day 2

Please read this file carefully before starting the Spark task file.

---

# 1. Day 2 goal

The goal of **Day 2** is to complete the **Spark stage** of the London Transport Data Engineering Project.

That means by the end of today, you should have:

- a working Day 2 Spark repository
- the correct folder structure
- access to the raw data files
- Spark working in your environment
- a guided Spark pipeline completed
- Spark transformations and joins completed
- reporting outputs generated
- checkpoint answers written
- your progress pushed to GitHub step by step

This is a full project day, and it should feel like a real junior data engineering task.

---

# 2. Day 2 project scope

For **Day 2 only**, your work is focused on **Spark processing**.

You are **not** working on:

- PostgreSQL ETL logic from Day 1
- PostgreSQL ELT logic from Day 1
- AWS S3 integration yet
- Databricks yet
- dashboards
- orchestration tools

Today is about taking the same London transport business problem and solving it using **Spark**.

That is the full focus of Day 2.

---

# 3. What Day 2 is trying to teach you

Day 2 is here to help you understand that the same business problem can be solved using a different data engineering tool.

On Day 1, you solved the problem locally with:

- Python
- PostgreSQL
- ETL
- ELT

On Day 2, you solve the same problem with:

- Spark
- Spark DataFrames
- Spark joins
- Spark aggregations
- Spark output generation

This is important because real data engineering work is not only about one tool.

It is about understanding the data problem, then applying the right framework to solve it.

---

# 4. Raw data for Day 2

For Day 2, you will work with the same London transport raw data environment.

The full raw source layer contains at least these files:

```text
data/raw/
├── stations.csv
├── lines.csv
├── journeys.json
├── vehicle_types.csv
├── operators.csv
├── zones.csv
├── disruptions.json
├── fares.csv
├── boroughs.csv
└── schedules.xml
````

However, for the main Spark reporting pipeline today, you will focus mainly on:

* `stations.csv`
* `lines.csv`
* `journeys.json`
* `boroughs.csv`
* `zones.csv`

The other files still belong to the project and may be used later for enrichment or future stages.

That is realistic.
Real projects often begin with the most relevant subset of sources first.

---

# 5. Day 2 repository structure

Your Day 2 Spark repository should look like this:

```text
london-transport-spark-project/
│
├── README.md
├── README_02_Day2_Structure_and_Tasks.md
├── README_03_Day2_Spark_Tasks.md
├── README_04_Day2_Checkpoint_Answers.md
│
├── data/
│   ├── raw/
│   │   ├── stations.csv
│   │   ├── lines.csv
│   │   ├── journeys.json
│   │   ├── vehicle_types.csv
│   │   ├── operators.csv
│   │   ├── zones.csv
│   │   ├── disruptions.json
│   │   ├── fares.csv
│   │   ├── boroughs.csv
│   │   └── schedules.xml
│   └── output/
│
├── src/
│   ├── spark_pipeline.py
│   └── utils.py
│
├── docs/
│   ├── project_notes.md
│   └── checkpoint_answers.md
│
├── logs/
│   └── spark.log
│
└── requirements.txt
```

This structure is simple, but still professional.

It helps separate:

* raw data
* Spark code
* documentation
* outputs
* logs

That is a good engineering habit.

---

# 6. Why this structure matters

A good Spark project should not be a random collection of files.

Even when the work is guided, your repository should still look organized and professional.

This structure helps make the project:

* easier to read
* easier to run
* easier to explain
* easier to show later in interviews

This is important because the value of this project is not only the code.
It is also how clearly and professionally it is presented.

---

# 7. Environment setup path for students

Because students use different operating systems, the beginning of the setup is slightly different.

## Windows students

Windows students should begin from:

* **Git Bash**
* then enter **WSL**
* then continue the Spark work inside WSL

## macOS students

macOS students should begin from:

* **Terminal**
* then continue directly into the project

After those environment-specific starting steps, everyone should continue with the **same Spark workflow**.

That means the technical beginning is different, but the main Day 2 tasks are shared by everyone.

---

# 8. Windows students - how to begin

If you are a Windows student, start like this:

## Step 1

Open **Git Bash**

## Step 2

Enter WSL

```bash
wsl
```

## Step 3

Move to the correct folder where your Day 2 repository is stored

Example:

```bash
cd /mnt/c/Users/YourName/path/to/london-transport-spark-project
```

## Step 4

Check that you are in the correct project folder

```bash
pwd
ls
```

You should see your README files, `src`, `data`, and `docs`.

---

# 9. macOS students - how to begin

If you are a macOS student, start like this:

## Step 1

Open **Terminal**

## Step 2

Move into your Day 2 project folder

Example:

```bash
cd /path/to/london-transport-spark-project
```

## Step 3

Confirm you are in the correct location

```bash
pwd
ls
```

You should see your README files, `src`, `data`, and `docs`.

---

# 10. Common setup for everyone after that

From this point onward, all students should continue with the same workflow.

## Step 1

Check Python version

```bash
python --version
```

or

```bash
python3 --version
```

## Step 2

Check Java version

```bash
java -version
```

Spark needs Java, so this is important.

## Step 3

Check whether PySpark is available

```bash
pyspark
```

If PySpark opens, that is a good sign.

If not, follow the instructions from class or your setup guidance.

## Step 4

Exit PySpark after checking

```python
exit()
```

or press:

```text
Ctrl + D
```

---

# 11. Suggested Day 2 task list

Here is what you must complete today.

## Task 1 — Read the Day 2 main README

Read `README.md` and understand:

* what Stage 2 is
* how it connects to Day 1
* why Spark matters

## Task 2 — Confirm your environment

Make sure you can:

* open the correct terminal
* move into the correct project folder
* check Python and Java
* start PySpark

## Task 3 — Confirm the project structure

Make sure the Day 2 repository has the correct folders and files.

## Task 4 — Confirm the raw data files

Make sure the raw files are inside:

```text
data/raw/
```

## Task 5 — Complete the Spark tasks

Follow the guided Spark steps in:

* [README 03 - Day 2 Spark Tasks](./README_03_Day2_Spark_Tasks.md)

## Task 6 — Write project notes

Add notes to:

```text
docs/project_notes.md
```

You should write short notes about:

* which files were used in the Spark pipeline
* what transformations were done
* what outputs were created
* what you noticed about Spark compared to Day 1

## Task 7 — Answer checkpoint questions

Complete:

```text
docs/checkpoint_answers.md
```

## Task 8 — Commit and push progress

Push your progress regularly to your GitHub repository.

---

# 12. Suggested Day 2 commit milestones

To help you stay organized, here are some good commit points for today.

## Commit 1

After repository setup and environment check

Example:

```bash
git commit -m "Set up Day 2 Spark repository and environment"
```

## Commit 2

After confirming the raw data files

```bash
git commit -m "Add Day 2 raw data files"
```

## Commit 3

After starting Spark extraction and setup

```bash
git commit -m "Start Spark data loading tasks"
```

## Commit 4

After finishing Spark transformations and joins

```bash
git commit -m "Complete Spark transformations and joins"
```

## Commit 5

After finishing outputs, notes, and checkpoint answers

```bash
git commit -m "Complete Day 2 Spark deliverables"
```

These are examples, but they show how to work in a structured way.

---

# 13. What students should focus on technically today

The most important technical ideas of Day 2 are:

* reading raw data with Spark
* understanding Spark DataFrames
* inspecting schemas
* selecting columns
* cleaning values
* joining datasets
* aggregating results
* writing outputs

This is not meant to be random Spark experimentation.

It is a guided workflow, so the main goal is to understand each step clearly.

---

# 14. What students should observe while working

As you work today, think about these questions:

* How is Spark reading the raw data differently from Day 1?
* What feels similar to Day 1?
* What feels different from Day 1?
* Where is the business logic still the same?
* Why would a team choose Spark instead of only local scripts?

These are important reflections because they connect tool usage to real engineering thinking.

---

# 15. Day 2 deliverables

By the end of Day 2, your repository should contain:

* your own public GitHub Day 2 repository
* all 4 Day 2 README files
* the correct project structure
* the raw data files
* a working Spark script
* reporting outputs
* project notes
* checkpoint answers
* multiple commits showing step-by-step progress

If these are missing, your Day 2 submission is incomplete.

---

# 16. Why this day is important professionally

Day 2 helps you move from local beginner-level pipeline logic into a more modern data engineering tool.

That is important because many real data engineering roles involve Spark directly or indirectly.

So even in this beginner-friendly project, you are building experience that sounds stronger and more realistic when you talk about your work later.

A project that includes:

* raw data
* ETL / ELT
* PostgreSQL
* Spark
* structured GitHub documentation

already sounds much more like a real data engineering project than a small classroom exercise.

---

# 17. Required checkpoint file

For Day 2, you must also complete the checkpoint answers file.

You will find or create it here:

```text
docs/checkpoint_answers.md
```

This file is mandatory.

It helps confirm that you understood:

* the purpose of the Spark stage
* how it connects to the full project
* the main technical ideas from today

---

# 18. Important reminder

This project is **guided**.

You are not expected to invent everything from scratch.

You are expected to:

* follow the steps carefully
* run the provided logic
* understand what each step is doing
* write notes clearly
* keep your GitHub repository organized

That is the right way to learn from this kind of project.

---

# 19. Final message before starting Spark tasks

Day 2 is an important step in the full-stack journey of this project.

Yesterday you solved the problem locally.
Today you will solve the same problem with Spark.

That is exactly how a real engineering project grows:
the business case stays stable, and the technology evolves.

Now continue to the next file:

## Next step

* [README 03 - Day 2 Spark Tasks](./README_03_Day2_Spark_Tasks.md)



