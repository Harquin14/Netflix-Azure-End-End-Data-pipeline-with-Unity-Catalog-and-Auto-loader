# Netflix-Azure-End-End-Data-pipeline(Azure + Databricks + Unity Catalog)


This project demonstrates a full production-grade medallion architecture (Bronze → Silver → Gold) implemented using Azure Data Factory, Azure Data Lake Gen2, Databricks, Unity Catalog, and Auto Loader. 


The pipeline ingests raw data from GitHub, processes it incrementally using Databricks, applies business transformations, and outputs analytics-ready Delta tables for reporting.


<img width="1261" height="623" alt="netflix drawio" src="https://github.com/user-attachments/assets/d580ad5f-1fcb-4520-80e0-23a929c5e2f2" />


# Architecture Overview 


🔹 Technologies

- Azure Data Factory (ADF) – Orchestration & ingestion

- Azure Data Lake Storage (ADLS Gen2) – Bronze, Silver & Gold layers

- Databricks + Unity Catalog – ELT processing, governance, Delta Lake

- Databricks Auto Loader – Incremental file ingestion

- GitHub – Raw data source

- Tableau  – Reporting (Gold layer outputs)

🔹 Medallion Layout

- Bronze (Raw Layer)

- File Format: CSV

- Landing zone for raw, unmodified data

- Silver (Cleaned & Standardized)

- File Format: Delta

- Data cleansing, schema enforcement, transformations

- Gold (Business Layer)

- File Format: Delta

- Aggregations, business rules, KPI datasets for BI tools
