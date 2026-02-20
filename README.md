# ADS507 Final Project  
## MetroPT3 Incremental ETL Pipeline

---

## 🌟 Highlights
- Incremental ETL pipeline using **MySQL + Docker**
- Watermark-based loading for efficient updates
- Full schema reset and reproducible pipeline execution
- Monitoring dashboard for runtime visibility
- Uses SQLAlchemy for clean Python + SQL integration

---

## ℹ️ Overview
This project implements a production-style incremental ETL pipeline using MetroPT3 compressor data. The pipeline loads structured CSV data into a MySQL database, tracks ingestion progress using a watermark control table, and supports repeatable execution with monitoring capabilities.

The goal of this project is to simulate real-world data engineering practices including:
- Schema initialization  
- Controlled incremental ingestion  
- Data consistency management  
- Pipeline observability  

The system is designed to run against a Dockerized MySQL instance for reproducibility.

---

## 🧱 Architecture

### Pipeline Components

1. **Database Initialization**  
   Drops and recreates all pipeline tables for a clean slate.

2. **Watermark Control Table (`etl_control`)**  
   Tracks the last processed record to enable incremental loads.

3. **Incremental Loader**
   - Reads new CSV data  
   - Inserts only unseen records  
   - Updates watermark  

4. **Monitoring Dashboard**
   Displays:
   - Current watermark  
   - Record counts  
   - Pipeline state  

---

## ⬇️ Installation

### Requirements
- Python 3.9+
- MySQL (Docker recommended)
- SQLAlchemy
- pandas
- PyMySQL

### Install dependencies
```bash
pip install pandas sqlalchemy pymysql

docker run --name mysql-etl \
  -e MYSQL_ROOT_PASSWORD=password \
  -p 3306:3306 \
  -d mysql:latest

## 📊 Data Source

The MetroPT3 compressor dataset was obtained from:  
https://archive.ics.uci.edu/dataset/791/metropt3+dataset

The dataset contains structured CSV sensor data representing compressor performance metrics. It was used to simulate a real-world incremental ingestion workflow.

---

🚀 Usage

Run the notebook cells sequentially:

Connect to MySQL

Reset schema

Initialize watermark

Run incremental ETL

View monitoring dashboard

The pipeline will:

Insert only new data

Avoid duplicate processing

Maintain ingestion state

👤 Author

Caly Nguyen
Graduate student focused on data science, data engineering, and machine learning systems.
Initially, I thought SQL could not create “real” programs. This project showed me that SQL is more than writing queries — it can support reliable pipelines when combined with thoughtful system design.