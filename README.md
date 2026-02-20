# ADS507_Finalproject

MetroPT3 Incremental ETL Pipeline
🌟 Highlights
    Incremental ETL pipeline using MySQL + Docker
    Watermark-based loading for efficient updates
    Full schema reset and reproducible pipeline execution
    Monitoring dashboard for runtime visibility
    Uses SQLAlchemy for clean Python with SQL integration

ℹ️ Overview
This project implements a production style incremental ETL pipeline using MetroPT3 compressor data. The pipeline loads structured CSV data into a MySQL database, tracks ingestion progress using a watermark control table, and supports repeatable execution with monitoring capabilities.

The goal of this project is to simulate real world data engineering practices including:
    Schema initialization
    Controlled incremental ingestion
    Data consistency management
    Pipeline observability
The system is designed to run against a Dockerized MySQL instance for reproducibility.

🧱 Architecture
Pipeline Components:
1.Database Initialization
  Drops and recreates all pipeline tables for a clean slate.

2.Watermark Control Table (etl_control)
  Tracks last processed record to enable incremental loads.

3.Incremental Loader
  Reads new CSV data
  Inserts only unseen records
  Updates watermark

4.Monitoring Dashboard
  Displays:
    Current watermark
    Record counts
    Pipeline state

⬇️ Installation
Requirements:
    Python 3.9+
    MySQL (Docker recommended)
    SQLAlchemy
    Pandas
    PyMySQL

Install dependencies:
    !pip install pandas sqlalchemy pymysql

Start MySQL via Docker (example):
    docker run --name mysql-etl -e MYSQL_ROOT_PASSWORD=password -p 3306:3306 -d mysql:latest

🚀 Usage
Run the notebook cells sequentially:
1.Connect to MySQL
2.Reset schema
3.Initialize watermark
4.Run incremental ETL
5.View monitoring dashboard

The pipeline will:
    Insert only new data
    Avoid duplicate processing
    Maintain ingestion state

👤 Author
Caly Nguyen
Graduate student focused on data science data engineering, and machine learning systems. Initially, I thought SQL could not create "real" programs. This project showed me that SQL is more than writing queries. SQL can support reliable pipelines when combined with good design.