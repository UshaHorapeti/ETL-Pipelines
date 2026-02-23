# ETL Pipeline

## Overview

This repository contains a production-grade ETL pipeline designed to automate extraction, transformation, validation, and loading of Demand Planning data into SQL Server.

The system integrates:

- Power BI Semantic Models (ADOMD.NET + DAX)
- Python (Pandas)
- SQL Server (pyodbc)

The automation reduced manual reporting time from 5–6 hours to ~30 minutes and eliminated repetitive analyst intervention.

---

## Architecture

    Power BI Semantic Model
            │
            ▼
    PowerShell (ADOMD.NET + DAX Execution)
            │
            ▼
    Raw Extract Files (CSV / Excel)
            │
            ▼
    Python Transformation Layer (Pandas)
            │
            ▼
    Standardized Output Files
            │
            ▼
    SQL Upload Layer (Chunked Insert via pyodbc)
            │
            ▼
    Post-SQL Validation

The pipeline follows a modular, decoupled ETL design to improve:

- Observability  
- Re-runnability  
- Debugging  
- Failure isolation  
- Production stability  

---

## Extract Layer

- Uses ADOMD.NET to execute DAX queries against Power BI Semantic Models.
- PowerShell scripts establish session-based connections to Analysis Services.
- Query results are streamed row-by-row and exported to CSV.
- Fail-fast behavior prevents downstream execution if extraction fails.

Why ADOMD.NET?

- Ensures consistency with business logic defined in Power BI.
- Enables DAX execution against tabular models.
- Avoids duplicating calculation logic in SQL.

---

## Transformation Layer (Python / Pandas)

The transformation engine is configuration-driven using structured job definitions.

### Responsibilities

- Schema standardization  
- Mapping enrichment  
- Snapshot normalization  
- Attribute date conversion  
- Business unit derivation  
- Null and numeric cleansing  

### Standard Output Schema

    ['Source',
     'Snapshot',
     'Material',
     'Sales Organization',
     'Country',
     'Attribute',
     'Value',
     'BU']

### Design Features

- Defensive copying of raw datasets  
- Mapping validation before merge  
- Deterministic file selection (latest by filename date)  
- Controlled column renaming  
- Left joins to preserve source rows  
- Explicit null normalization  

---

## Load Layer (SQL Server)

SqlUpload_Actuals.py and SqlUpload_Forecast.py:

- Consolidate multiple transformed feeds  
- Enforce expected schema  
- Normalize data types  
- Convert NaN → SQL NULL  
- Insert data using configurable chunk sizes (default: 5000 rows)  
- Append-mode ingestion  

### Why Chunking?

- Reduces lock contention  
- Improves memory stability  
- Prevents long-running transactions  
- Increases reliability for large datasets  

---

## Validation Layer

Post-SQL validation ensures:

- Row count consistency  
- Schema compliance  
- Data integrity checks  
- Controlled reconciliation between source and database  

---

## Design Principles

- Separation of concerns  
- Fail-fast behavior  
- Schema enforcement  
- Deterministic file handling  
- Defensive data cleaning  
- Configurable and scalable job orchestration  

---

## Performance Impact

- Reduced manual processing time by ~90%  
- Eliminated repetitive analyst effort  
- Standardized multi-feed ingestion  
- Improved traceability and consistency  

---

## Tech Stack

- Python (Pandas, NumPy)  
- ADOMD.NET (PowerShell)  
- SQL Server (pyodbc)  
- Configuration-driven transformation engine  
- Chunked ingestion strategy  

---

## Future Enhancements

- Transactional staging tables with swap strategy  
- Upsert/merge-based ingestion  
- Structured logging framework  
- Vectorized optimization for row-wise transformations  
- Orchestration integration (scheduler/Airflow)  

---

## Author

Built and maintained as part of enterprise Demand Planning automation initiatives.
The transformation engine is configuration-driven using structured job definitions.

### Responsibilities

- Schema standardization  
- Mapping enrichment  
- Snapshot normalization  
- Attribute date conversion  
- Business unit derivation  
- Null and numeric cleansing  

### Standard Output Schema

    ['Source',
     'Snapshot',
     'Material',
     'Sales Organization',
     'Country',
     'Attribute',
     'Value',
     'BU']

### Design Features

- Defensive copying of raw datasets  
- Mapping validation before merge  
- Deterministic file selection (latest by filename date)  
- Controlled column renaming  
- Left joins to preserve source rows  
- Explicit null normalization  

---

## Load Layer (SQL Server)

SqlUpload_Actuals.py and SqlUpload_Forecast.py:

- Consolidate multiple transformed feeds  
- Enforce expected schema  
- Normalize data types  
- Convert NaN → SQL NULL  
- Insert data using configurable chunk sizes (default: 5000 rows)  
- Append-mode ingestion  

### Why Chunking?

- Reduces lock contention  
- Improves memory stability  
- Prevents long-running transactions  
- Increases reliability for large datasets  

---

## Validation Layer

Post-SQL validation ensures:

- Row count consistency  
- Schema compliance  
- Data integrity checks  
- Controlled reconciliation between source and database  

---

## Design Principles

- Separation of concerns  
- Fail-fast behavior  
- Schema enforcement  
- Deterministic file handling  
- Defensive data cleaning  
- Configurable and scalable job orchestration  

---

## Performance Impact

- Reduced manual processing time by ~90%  
- Eliminated repetitive analyst effort  
- Standardized multi-feed ingestion  
- Improved traceability and consistency  

---

## Tech Stack

- Python (Pandas, NumPy)  
- ADOMD.NET (PowerShell)  
- SQL Server (pyodbc)  
- Configuration-driven transformation engine  
- Chunked ingestion strategy  

---

## Future Enhancements

- Transactional staging tables with swap strategy  
- Upsert/merge-based ingestion  
- Structured logging framework  
- Vectorized optimization for row-wise transformations  
- Orchestration integration (scheduler/Airflow)  

---

## Author

Built and maintained as part of enterprise Demand Planning automation initiatives.
