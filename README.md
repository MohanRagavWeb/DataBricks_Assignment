# Databricks Data Engineering Assignment

## 1. Introduction

This project demonstrates end-to-end data processing using Databricks with Unity Catalog volumes. It covers two main use cases:

1. Implementation of Medallion Architecture (Bronze, Silver, Gold) using CSV datasets
2. API data ingestion, transformation, and storage using Delta Lake

The project follows standard data engineering practices such as schema enforcement, modular design, and layered data processing.

---

## 2. Environment Details

* Platform: Databricks (Unity Catalog enabled)
* Storage: Volumes
* Processing Engine: Apache Spark
* Data Format: CSV (input), Delta (output)

Base storage path used:

```
/Volumes/assignment/default/employee_vol/
```

---

## 3. Assignment 1: Medallion Architecture

### 3.1 Folder Structure

The notebooks are organized into three layers:

```
source_to_bronze/
    utils
    employee_source_to_bronze

bronze_to_silver/
    employee_bronze_to_silver

silver_to_gold/
    employee_silver_to_gold
```

---

### 3.2 Input Data

The following datasets are used:

* employee.csv
* department.csv
* country.csv

Location:

```
/Volumes/assignment/default/employee_vol/input/
```

---

### 3.3 Bronze Layer

In this layer, raw data is ingested without transformation.

Steps performed:

* Read CSV files into DataFrames
* Store data in raw format

Output location:

```
/source_to_bronze/employee
/source_to_bronze/department
/source_to_bronze/country
```

---

### 3.4 Silver Layer

This layer focuses on data cleaning and standardization.

Steps performed:

* Read data using a predefined schema
* Convert column names from CamelCase to snake_case
* Add a load_date column
* Store data as Delta table

Database and table:

* Database: employee_info
* Table: dim_employee

Storage location:

```
/silver/employee_info/dim_employee
```

---

### 3.5 Gold Layer

This layer contains business-level transformations.

Transformations:

1. Total salary per department in descending order
2. Employee count per department and country
3. Department names with corresponding country names
4. Average age of employees per department

Additional processing:

* Add at_load_date column

Output location:

```
/gold/employee/
```

Tables created:

* fact_employee_salary
* fact_employee_count
* fact_employee_dept_country
* fact_employee_avg_age

---

### 3.6 Utility Functions

A common utility notebook is used to avoid code duplication.

Functions include:

* Column name conversion (CamelCase to snake_case)
* Adding load_date column

---

## 4. Assignment 2: API Data Ingestion

### 4.1 API Details

API endpoint:

```
https://reqres.in/api/users?page=2
```

Header used:

```
x-api-key: reqres_f84e4451a9e645e78c89a38b40af1425
```

---

### 4.2 Data Extraction

* Data is fetched dynamically using pagination
* The process continues until no more records are returned

---

### 4.3 Data Cleaning and Transformation

Steps performed:

* Removed unnecessary fields:

  * page
  * per_page
  * total
  * total_pages
  * support

* Extracted the nested data array

* Applied a custom schema

* Derived a new column:

  * site_address (from email domain)

* Added load_date column

---

### 4.4 Output Storage

Database:

```
site_info
```

Table:

```
person_info
```

Storage path:

```
/Volumes/assignment/default/employee_vol/site_info/person_info
```

Format:

* Delta

Write mode:

* Overwrite

---

## 5. Architecture Overview

```
Raw Data → Bronze → Silver → Gold
```

* Bronze: Raw ingestion
* Silver: Cleaned and structured data
* Gold: Aggregated and business-ready data

---

## 6. Key Design Considerations

* Use of Unity Catalog volumes for governed storage
* Schema enforcement for data consistency
* Modular notebook design for reusability
* Delta Lake for reliable storage and performance
* Separation of concerns using Medallion Architecture

---

## 7. Conclusion

This project demonstrates a structured approach to building scalable data pipelines in Databricks. It covers both batch data processing and API-based ingestion while maintaining clean architecture and best practices.

---


