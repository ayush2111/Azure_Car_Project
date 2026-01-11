# 🚗 Car Sales Azure ETL Pipeline

## 📌 Project Overview
This project demonstrates an **end-to-end Azure ETL data pipeline** built to process **car sales data** using Azure-native services.

The pipeline follows **real-world data engineering best practices**, including:
- Incremental data ingestion
- Medallion architecture (Silver → Gold)
- Slowly Changing Dimensions (**SCD Type 1**)
- Star schema data modeling
- Scalable and production-ready design

---

## 🏗️ Architecture Overview

### Azure Services Used
- **Azure Data Factory (ADF)** – Data ingestion & orchestration  
- **Azure Data Lake Storage Gen2 (ADLS)** – Data lake storage  
- **Azure Databricks** – Data transformation & modeling  
- **Azure SQL / Synapse** – Analytics layer  

---

## 📥 Data Ingestion – Azure Data Factory

Azure Data Factory is used to ingest data **incrementally** from the source system into ADLS.

### Incremental Load Design
- Watermark-based incremental logic
- Lookup activities to fetch:
  - Last successful load timestamp
  - Current load timestamp
- Copy activity loads **only new and updated records**
- Stored procedure updates watermark after successful load

### ADF Incremental Pipeline

![ADF Incremental Pipeline](https://github.com/ayush2111/Azure_Car_Project/blob/main/Screenshot%202026-01-11%20104851.png)

✔ Prevents full reloads  
✔ Optimized for large datasets  
✔ Production-ready approach  

---

## 🥈 Silver Layer – Databricks

The **Silver layer** contains cleaned and standardized data generated using Azure Databricks.

### Silver Layer Responsibilities
- Data cleansing
- Schema enforcement
- Type casting
- Null handling
- Deduplication

The Silver layer acts as a **single source of truth** for downstream transformations.

---

## 🥇 Gold Layer – Databricks (Fact & Dimensions)

From the Silver layer, data is transformed into **Gold tables** following a **star schema** design.

### Silver → Gold Transformation Flow

![Silver to Gold Flow](https://github.com/ayush2111/Azure_Car_Project/blob/main/Screenshot%202026-01-10%20191031.png)

### Gold Tables
- `gold_dim_branch`
- `gold_dim_date`
- `gold_dim_dealer`
- `gold_dim_model`
- `gold_fact_sales`

---

## 🔁 Slowly Changing Dimensions (SCD Type 1)

This project implements **SCD Type 1** for all dimension tables.

### SCD Type 1 Characteristics
- Overwrites old dimension values
- No historical tracking
- Maintains only the **latest state** of dimension attributes

### Implementation Approach
- Business keys used to identify existing records
- Existing dimension records are updated in place
- New records are inserted
- Implemented using Databricks transformation logic

✔ Simple and efficient  
✔ Common in operational analytics  
✔ Optimized for performance  

---

## 🧱 Star Schema Design

The Gold layer follows a **star schema** optimized for analytical queries and BI reporting.

![Star Schema Design](https://github.com/ayush2111/Azure_Car_Project/blob/main/Screenshot%202026-01-10%20185218.png)

### Tables

#### Fact Table
- **fact_sales**
  - Revenue
  - Units_Sold
  - Revenue_Per_Unit
  - Foreign keys to all dimension tables

#### Dimension Tables
- **dim_branch**
- **dim_dealer**
- **dim_model**
- **dim_date**

This design enables:
- Fast aggregations
- Simple joins
- Easy integration with BI tools like Power BI

---

## 📊 Data Layers Summary

| Layer | Description |
|------|------------|
| **Bronze** | Raw data ingested incrementally from source systems using Azure Data Factory |
| **Silver** | Cleaned, standardized, and validated data |
| **Gold** | Analytics-ready fact & dimension tables modeled using a star schema |

---

## 🚀 Key Highlights

- Azure Data Factory incremental pipelines
- Databricks Silver → Gold transformations
- SCD Type 1 dimension management
- Star schema data modeling
- Enterprise-style architecture
