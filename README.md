# Databricks Medallion Architecture & API Ingestion Project

## 📌 Overview

This project demonstrates end-to-end data engineering workflows using **Databricks (Unity Catalog + Volumes)**.
It includes:

1. **Medallion Architecture (Bronze → Silver → Gold) using CSV files**
2. **API Data Ingestion → Transformation → Delta Table**

All implementations follow **best practices** such as schema enforcement, reusable utilities, and Delta Lake usage.

---

# 🧩 Assignment 1: Medallion Architecture (CSV Processing)

## 📂 Project Structure

```
Workspace/
│
├── source_to_bronze/
│     ├── utils
│     └── employee_source_to_bronze
│
├── bronze_to_silver/
│     └── employee_bronze_to_silver
│
├── silver_to_gold/
      └── employee_silver_to_gold
```

---

## 📁 Volume Path Used

```
/Volumes/assignment/default/employee_vol/
```

---

## 📥 Input Datasets

* employee.csv
* department.csv
* country.csv

Stored at:

```
/Volumes/assignment/default/employee_vol/input/
```

---

## 🥉 Bronze Layer

### Actions Performed:

* Read CSV files as DataFrames
* Store raw data in volume

### Output:

```
/source_to_bronze/employee
/source_to_bronze/department
/source_to_bronze/country
```

---

## 🥈 Silver Layer

### Transformations:

* Read data with **custom schema**
* Convert column names:

  * CamelCase → snake_case
* Add column:

  * `load_date`
* Store as Delta Table

### Table Details:

* Database: `employee_info`
* Table: `dim_employee`

### Output Path:

```
/silver/employee_info/dim_employee
```

---

## 🥇 Gold Layer

### Business Transformations:

1. Total salary per department (descending)
2. Employee count per department per country
3. Department name with country name
4. Average age per department

### Additional Column:

* `at_load_date`

### Output Tables:

```
/gold/employee/fact_employee_salary
/gold/employee/fact_employee_count
/gold/employee/fact_employee_dept_country
/gold/employee/fact_employee_avg_age
```

---

## 🔧 Utilities Used

* Column conversion (camel → snake)
* Load date addition

---

# 🌐 Assignment 2: API Data Ingestion

## 📡 API Used

```
https://reqres.in/api/users?page=2
```

Header:

```
x-api-key: reqres_f84e4451a9e645e78c89a38b40af1425
```

---

## ⚙️ Processing Steps

### 1. API Extraction

* Loop through pages dynamically
* Stop when API returns empty data

### 2. Data Cleaning

* Dropped fields:

  * page
  * per_page
  * total
  * total_pages
  * support

### 3. Schema Enforcement

* Applied custom schema to DataFrame

### 4. Flattening

* Extracted nested `data` block

### 5. Derived Columns

* `site_address` from email (e.g., reqres.in)
* `load_date` (current date)

---

## 💾 Output

### Database:

```
site_info
```

### Table:

```
person_info
```

### Storage Path:

```
/Volumes/assignment/default/employee_vol/site_info/person_info
```

### Format:

* Delta
* Mode: Overwrite

---

# 🏗️ Architecture Summary

```
        ┌──────────────┐
        │   Raw Data   │
        └──────┬───────┘
               │
        ┌──────▼───────┐
        │   Bronze     │  (Raw CSV)
        └──────┬───────┘
               │
        ┌──────▼───────┐
        │   Silver     │  (Cleaned + Delta)
        └──────┬───────┘
               │
        ┌──────▼───────┐
        │    Gold      │  (Business Aggregates)
        └──────────────┘
```

---

# 🚀 Key Features

* ✅ Unity Catalog Volume usage
* ✅ Schema enforcement
* ✅ Reusable utility functions
* ✅ Delta Lake storage
* ✅ API pagination handling
* ✅ Data standardization (snake_case)
* ✅ Layered architecture (Medallion)

---

# 🔥 Interview Highlights

* Why Medallion Architecture?
  → Improves data quality and maintainability

* Why Delta?
  → ACID transactions, versioning, performance

* Why custom schema?
  → Avoids inference issues, ensures consistency

* Why loop API?
  → Handles dynamic pagination

* Why snake_case?
  → Industry standard naming convention

---

# 📌 Conclusion

This project demonstrates:

* End-to-end ETL pipeline design
* Real-time API ingestion
* Data transformation and optimization
* Production-ready data engineering practices

---

# 👨‍💻 Author

**Name:** Mohan
**Course:** B.E. Computer Science Engineering
**Focus:** Databricks | Data Engineering | Full Stack Development

---
