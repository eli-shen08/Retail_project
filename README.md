# ⭐ Azure Retail Insight Lakehouse

#### End-to-End Azure Data Engineering Project with Medallion Architecture
![cover](pic/retail_proj.png)

## 📖 Project Overview
Azure Retail Insight Lakehouse (ARIL) is a fully automated Azure Data Engineering solution designed to ingest, validate, transform, model, and visualize retail data coming from multiple sources.

This project demonstrates real-world, enterprise-grade practices using the **Medallion Architecture (Bronze → Silver → Gold)** along with workflow **Automation, Email Alerts, Transformations, and Analytics**.

#### Full Pipeline in ADF
![Full Pipe](pic/full_pipe.png)

✨ Key Features
🔹 1. Multi-Source Data Ingestion

  - Extract retail data from:
    - Azure SQL Database
    - GitHub raw files
  - Loaded into ADLS Bronze layer
  - Parallel ingestion for improved performance

## Data Creation
First I have created a Schema & Multiple Tables inside Azure SQL Database, the code can be found [db.sql](db.sql). <br/>
After that I have upload [customers.json](customers.json) file in a github repsitory so that I can fetch it using an API request.

First I have used a **LookUp Activity** and to get the Tables Names under the **Retail** Schema. 

