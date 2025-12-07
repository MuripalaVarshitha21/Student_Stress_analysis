# Student_Stress_analysis
Student Stress Analytics project using Databricks. Includes data ingestion, cleaning, feature engineering, PySpark notebook analysis, SQL insights, and an interactive dashboard to visualize stress patterns, risk scores, academic pressure, wellbeing, and behavioral factors. 



# 📘 *Student Stress Analytics – Databricks Final Exam Project 

This repository contains an end-to-end *Student Stress Analysis* project completed using Databricks.
It includes data ingestion, cleaning, feature engineering, PySpark notebook analysis, SQL analytics, and a fully interactive dashboard.
Built for the module *Unlocking the Power of Big Data (MADSC102)*.

---

# 📁 *Repository Contents (Flat Main-Branch Structure)*

All files are stored *directly on the main branch* with no subfolders:

| File Name                               | Description                                                                    |
| --------------------------------------- | ------------------------------------------------------------------------------ |
| *Student Stress Factors.csv*          | Original dataset containing student stress indicators                          |
| *Students_stress_factors.ipynb*       | PySpark notebook for data cleaning, feature engineering, and notebook analysis |
| *SQL_Quries.ipynb*                    | SQL notebook containing all analytical queries used for dashboard visuals      |
| *Student_stress_analysis.lvdash.json* | Exported interactive Databricks dashboard configuration                        |
| *README.md*                           | Documentation explaining the entire project                                    |


---

# 📊 *Dataset Description*

Student Stress Factors.csv contains self-reported student wellbeing metrics including:

* *sleep_quality*
* *headaches_per_week*
* *academic_performance*
* *study_load*
* *extracurricular_freq*
* *stress_level*
* Optional: review text / notes

These features help analyze how lifestyle, academics, and health contribute to overall *student stress*.

---

# 🧵 *1. Data Ingestion (Requirement 4.1)*

The dataset was uploaded into Databricks via:

**Create Table → Upload File → Create in Catalog (workspace.students_stress).**

Then loaded in the notebook:

python
spark.sql("USE CATALOG workspace")
spark.sql("USE SCHEMA students_stress")
df_raw = spark.table("student_stress_factors_raw")


This establishes the raw data source for cleaning.

---

# 🧼 *2. Data Cleaning & Feature Engineering (Requirement 4.2)*

Performed in *Students_stress_factors.ipynb*.

### ✔ Cleaning actions

* Renamed long/emoji column names
* Casted numeric fields (int/double)
* Removed rows with key missing values
* Standardized categorical fields

### ✔ Engineered features

* *wellbeing_score*
* *academic_stress_index*
* *risk_score* (stress risk prediction metric)
* Text-based features such as review length (if available)

These added attributes allow richer and more accurate analysis.

---

# 💾 *3. Delta Table Storage (Requirement 4.3)*

Cleaned data was saved as a Delta table:

python
df.write.format("delta")
  .mode("overwrite")
  .saveAsTable("workspace.students_stress.student_stress_clean")


A Delta table enables fast SQL queries and dashboard integration.

---

# 📘 *4. Notebook Analysis (Requirement 4.5)*

Inside the PySpark notebook *Students_stress_factors.ipynb*, the analysis included:

* Summary statistics
* Grouped comparisons (sleep vs stress, study load vs stress)
* Notebook-level charts built using display()
* A primary visual showing stress trends across key factors

This satisfies the requirement for at least one *notebook-based visualization*.

---

# 🧮 *5. SQL Analysis (Requirement 4.4)*

Queries stored inside *SQL_Quries.ipynb* and also referenced in the dashboard export.

### Example SQL insights used:

#### ✔ Stress vs Extracurricular Activity

sql
SELECT extracurricular_freq, COUNT(*) AS num_students,
       ROUND(AVG(stress_level), 2) AS avg_stress_level
FROM student_stress_clean
GROUP BY extracurricular_freq;


#### ✔ Stress Risk Pattern (Scatter Plot)

sql
SELECT stress_level,
       ROUND(AVG(wellbeing_score), 2) AS avg_wellbeing_score,
       ROUND(AVG(academic_stress_index), 2) AS avg_academic_stress_index,
       ROUND(AVG(risk_score), 2) AS avg_risk_score
FROM student_stress_clean
GROUP BY stress_level;


#### ✔ Stress Funnel (Study Load → Sleep → Stress)

sql
SELECT 'Total Students' AS stage, COUNT(*) AS total
FROM student_stress_clean
UNION ALL
SELECT 'High Study Load', COUNT(*) FROM student_stress_clean WHERE study_load >= 4
UNION ALL
SELECT 'Poor Sleep', COUNT(*) FROM student_stress_clean WHERE sleep_quality <= 2
UNION ALL
SELECT 'High Stress', COUNT(*) FROM student_stress_clean WHERE stress_level >= 4;


#### ✔ KPI Metrics

sql
SELECT COUNT(*) AS total_students,
       ROUND(AVG(stress_level),2) AS avg_stress_level,
       ROUND(MAX(risk_score),2) AS max_risk_score
FROM student_stress_clean;


These satisfy the *“at least 2 SQL queries”* requirement.

---

# 📊 *6. Dashboard (Requirement 4.6)*

Dashboard export: *Student_stress_analysis.lvdash.json*

### Visuals included:

* *Pie Chart* → Stress by Extracurricular Frequency
* *Scatter Plot* → Wellbeing vs Academic Pressure (Risk Gradient)
* *Funnel Chart* → Stress Progression Pipeline
* *KPI Tiles* → Total Students, Average Stress, Max Risk
* *Interactive Filters*:

  * stress_level
  * sleep_quality
  * study_load
  * extracurricular_freq

These filters make the dashboard dynamic and professor-ready.

---

# 🏁 *7. Summary*

This project demonstrates full proficiency with Databricks:

* ✔ Data ingestion
* ✔ Cleaning & feature engineering
* ✔ Delta storage
* ✔ PySpark notebook analysis
* ✔ SQL analytics
* ✔ Fully interactive dashboard

