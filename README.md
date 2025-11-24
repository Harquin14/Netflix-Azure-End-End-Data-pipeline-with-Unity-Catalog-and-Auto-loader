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



  🔄 Pipeline Flow
  
1. Azure Resources Created

- I deployed all required cloud components:

- Resource Group

- Azure Data Lake Gen2

- Azure Data Factory

- Databricks Workspace (Unity Catalog enabled)

- Access Connector for Databricks


# 📥 2. ADF Pipeline for Data Ingestion (GitHub → ADLS Bronze)


### ✔️ Copy Activity

- Pulls CSV files directly from a public GitHub repo using a HTTPS linked service.

- ADLS Gen2 is the sink linked service.

### ✔️ Pipeline Parameters

- Parameter	Purpose
  
- folder_name	Bronze folder path
 
- file_name	Name of the file to pull from GitHub

  
### ✔️ ForEach Activity

- Used because multiple files must be copied:

- Iterates through each file reference

- Executes the copy activity dynamically
 
### ✔️ Validation Activity

Before loading:

- Checks if expected marker file exists in ADLS

- Only proceeds when validation passes

### ✔️ Web Activity + Set Variable

- Web activity sends a GET request to GitHub REST API

- Extracts metadata of files (filenames, updated timestamps)

- "Set Variable" stores the JSON response → used for iteration
- 

# ⚡ 3. Databricks Processing (Bronze → Silver → Gold)


## 3.1 Auto Loader Pipeline (Bronze Loader)

- A Databricks notebook that:

- Listens to the raw_data directory in ADLS

- Ingests files incrementally using Auto Loader

- Writes clean Bronze tables registered in Unity Catalog

- Output Format: CSV → Delta (Bronze)
  

## 3.2 Parameterized Notebook (Silver Loader)

## Purpose:

- Process multiple datasets dynamically

- Enforce schema, cast datatypes, drop nulls, clean bad rows

- This notebook is triggered from Databricks Jobs with parameters such as:

      table_name
      input_path
      output_path


- Output Format: Delta (Silver)

  

## 3.3 Scheduled Weekly Transformations


### Using Databricks Jobs, I scheduled:

- Data cleaning & transformations every Sunday

- Ensures Silver data remains clean and up-to-date

  

## 3.4 Silver → Gold Processing


### A parameterized Gold notebook:

- Runs business logic

- Performs aggregations

- Builds KPI datasets for analytics

- Output Format: Delta (Gold)
- 

# 📊 4. Reporting Layer

### Gold tables are consumed in:

- Tableau


- These datasets are optimized for:

- Dashboards

- KPIs

- Real-time insights
