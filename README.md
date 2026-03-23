# Batch and Streaming ETL Pipeline for Music Insights

A production-style **Medallion Architecture** project built on **Azure**, **Databricks**, **Azure Data Factory (ADF)**, and **PySpark** to process music data in both batch and streaming modes.

This project demonstrates how raw music data is ingested into the **Bronze** layer, transformed into a clean and trusted **Silver** layer, and then curated into a business-ready **Gold** layer for analytics and insights.

---

## Project Highlights

* **Incremental batch ingestion** using Azure Data Factory
* **Real-time / near real-time streaming** using Spark Structured Streaming and Auto Loader
* **Medallion Architecture** with **Bronze → Silver → Gold** layers
* **Delta Lake / Delta Live Tables** style processing for reliable transformations
* **SCD Type 2** logic for historical tracking
* **Databricks Asset Bundles** used to deploy reusable jobs and workflows for Silver and Gold layers
* **Environment-aware deployment** for **dev / stage / prod**

---

## Architecture Overview

```text
Source Systems
   ↓
Azure Data Factory / Streaming Ingestion
   ↓
Bronze Layer (Raw Landing Zone)
   ↓
Databricks (Cleaning, Enrichment, Business Rules)
   ↓
Silver Layer (Trusted / Standardized Data)
   ↓
Databricks (Aggregations, KPIs, Insights)
   ↓
Gold Layer (Analytics-Ready Data)
   ↓
Downstream Reporting / BI / Analysis
```

---

## Tech Stack

* **Azure Data Lake Storage Gen2**
* **Azure Data Factory**
* **Azure Databricks**
* **PySpark**
* **Spark Structured Streaming**
* **Auto Loader**
* **Delta Lake / Delta Live Tables**
* **Databricks Asset Bundles**
* **SQL Server / Azure SQL Database**

---

## How the Project Works

### 1) Ingestion Layer

Data is brought into Azure using:

* **ADF pipelines** for scheduled or incremental batch loads
* **Spark Structured Streaming** for streaming data ingestion
* **Auto Loader** for scalable file ingestion

### 2) Bronze Layer

The Bronze layer stores raw, source-aligned data with minimal transformation.

**Purpose:**

* Keep original data intact
* Support replay and auditing
* Capture CDC / incremental changes

**Stored in:**

* `bronze/`
* `cdc/`
* `logs/`

### 3) Silver Layer

The Silver layer is where the main data cleaning and transformation happens.

**What happens here:**

* Deduplication
* Schema standardization
* Null handling
* Data type casting
* Incremental merge logic
* Historical tracking using **SCD Type 2**

**Implementation:**

* Built using **Databricks notebooks / jobs**
* Deployed through **Databricks Asset Bundles**

### 4) Gold Layer

The Gold layer contains business-ready tables for reporting and analytics.

**What happens here:**

* Aggregations
* KPI calculations
* Final curated datasets
* Domain-specific insights

**Implementation:**

* Built using **Databricks**
* Deployed through **Databricks Asset Bundles**

---

## Project Hierarchy

### Azure Resource Hierarchy

```text
Resource Group
├── Azure Data Factory
├── Azure Databricks Workspace
├── Storage Account (ADLS Gen2)
├── Azure SQL Server
├── Azure SQL Database
└── API Connection / Linked Services
```

### Storage Hierarchy

```text
Storage Account
├── bronze
│   ├── DimArtist
│   ├── DimArtist_cdc
│   ├── DimDate
│   ├── DimDate_cdc
│   ├── DimTrack
│   ├── DimTrack_cdc
│   ├── DimUser
│   ├── DimUser_cdc
│   ├── FactStream
│   ├── FactStream_cdc
│   └── cdc
├── silver
│   ├── DimArtist
│   ├── DimDate
│   ├── DimTrack
│   ├── DimUser
│   └── FactStream
├── gold
│   └── curated analytics tables / aggregates
├── databricksmetastore
└── $logs
```

### ADF Pipeline Hierarchy

```text
Azure Data Factory
├── Incremental_Ingestion
└── looping_Incremental
```

### Databricks Deployment Hierarchy

```text
Databricks Asset Bundle
├── resources
├── src / notebooks
├── jobs
├── parameters
└── targets
    ├── dev
    ├── stage
    └── prod
```

---

## Pipeline Flow

### Incremental Ingestion Pipeline

This pipeline handles incremental loads from the source and moves data into the Bronze layer.

### Looping / Orchestration Pipeline

This pipeline orchestrates repeated processing logic using **ForEach** and conditional activities.

### Databricks Processing

Databricks is used for:

* Transforming Bronze → Silver
* Creating Silver → Gold
* Running reusable jobs via Asset Bundles
* Managing environment-specific deployment

---

## Why Databricks for Silver and Gold?

Databricks is used for the Silver and Gold layers because it is well suited for:

* Complex data transformations
* Scalable Spark processing
* Streaming + batch unified processing
* Reusable notebooks and jobs
* Deployment automation with **Databricks Asset Bundles**

This keeps the architecture clean:

* **ADF** = orchestration and ingestion
* **Databricks** = transformation and analytics preparation
* **ADLS** = storage and layer separation

---

## Key Features

* Incremental processing
* Streaming support
* Medallion architecture
* Historical data tracking
* Reusable deployment structure
* Clean separation of storage layers
* Production-style workflow design

---

## Example Use Case

This project can be used to analyze music-related events such as:

* User activity
* Track plays
* Artist trends
* Time-based streaming patterns
* Fact and dimension modeling for analytics

---

## Future Improvements

* Add automated testing for Databricks jobs
* Add monitoring and alerting dashboards
* Add CI/CD pipeline for bundle deployment
* Add Power BI dashboard on top of Gold tables
* Add data quality checks and expectations

---

## Contact

Created by **Harshit Sharma**

---
