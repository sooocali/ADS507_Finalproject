# ADS507 Final Project – Design Document

**GitHub Repository:** https://github.com/sooocali/ADS507_Finalproject.git

**Author:** Caly Nguyen  
**Course:** ADS507  
**Last Updated:** February 2026

---

## 1. Project Overview

This project implements a containerized, incremental ETL pipeline that ingests structured CSV sensor data into MySQL. The pipeline is designed to:
- Load data incrementally
- Track ingestion state using a watermark table
- Support monitoring through run logs
- Produce an analytics ready final table for downstream querying and modeling

The system uses:
- Docker (MySQL 8.0)
- Python (Pandas + SQLAlchemy)
- Jupyter Notebook
- MySQL schema design (raw → staging → final)

---

## 2. Source Dataset

### Dataset
**MetroPT-3 Compressor Dataset**

### Where it came from
UCI Machine Learning Repository:  
https://archive.ics.uci.edu/dataset/791/metropt+3+dataset

### Why I chose it
I chose this dataset because it simulates a realistic telemetry/sensor stream:
- It is structured (CSV) and suitable for relational ingestion
- It includes time based records, which is ideal for incremental loading
- It reflects real world monitoring and predictive maintenance use cases
- It allows demonstrating raw/staging/final layers and pipeline observability

---

## 3. Pipeline Output

### Output artifact(s)
The main output of the pipeline is an analytics ready table

The output contains:
- Cleaned and typed fields
- Deduplicated records
- Only newly ingested rows since the last run

### Why the output is useful
The output is useful because it can be directly used for:
- Exploratory analysis 
- Trend monitoring of compressor behavior
- Anomaly detection
- Predictive maintenance modeling
- Dashboarding and reporting

---

## 4. System Architecture

### Architecture diagram 

![Architecture Diagram](images/Architecture_diaragram.png)

**Architecture summary:**
1. CSV source data is read by the Python ETL process.
2. Data is loaded into a raw table to preserve the original structure.
3. Transformations/cleaning occur in staging.
4. Cleaned data is loaded to the final table.
5. Watermark/control table stores “last processed” state.
6. Run logs capture pipeline execution metadata.

---

## 5. Database Schema

### Final schema diagram (required)

![Final Schema Diagram](images/compressor_ERR.png)

### Tables and purpose

- **raw_metro**
  - Stores ingested data as close to source format as possible.
  - Purpose: reproducibility + auditability.

- **stg_metro**
  - Stores cleaned/transformed records.
  - Purpose: standardize types, fix nulls, parse timestamps, rename fields, etc.

- **metro_data**
  - Stores analytics ready records.
  - Purpose: optimized querying and downstream analytics/ML use.

- **etl_control**
  - Stores ingestion state per dataset (example: last timestamp, last row id).
  - Purpose: incremental loads and deduplication.

- **etl_run_log**
  - Stores metadata about each ETL run.
  - Purpose: monitoring and observability.

---

## 6. Gaps, Risks, and Limitations

### Scaling
**Current state:**
- Runs on a single MySQL Docker container
- Works well for small to moderate datasets

**Scaling concerns as data grows:**
- Larger datasets will increase load and query times
- Indexing/partitioning may be required
- Notebook execution is manual

**Potential improvements:**
- Add indexes on key fields
- Partition tables by date/time
- Replace notebook runs with orchestration
- Move database to a managed service if needed

### Security
**Current state:**
- Credentials may be stored in docker compose or notebook
- MySQL exposed via a host port for local development

**Security gaps:**
- No secrets manager
- No strict least privilege roles by default

**Potential improvements:**
- Use `.env` for secrets
- Create limited DB roles

### Extensibility
**Current state:**
- Modular raw → staging → final design makes it easy to add new datasets
- Watermark table pattern supports multiple pipelines

**Potential improvements:**
- Add data quality checks (row counts, null thresholds, schema checks)
- Add automated unit/integration tests for ETL logic

---

## 7. Conclusion

This pipeline demonstrates production style ETL fundamentals:
- Incremental ingestion using watermark tracking
- Clear separation of raw, staging, and final layers
- Monitoring using run logs
- Containerized deployment for reproducibility

The system is a strong foundation for data engineering workflows and can be improved with the addition of security deatures, and scaling techniques.