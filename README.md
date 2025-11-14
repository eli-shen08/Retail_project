# ⭐ Azure Retail Insight Lakehouse

#### End-to-End Azure Data Engineering Project with Medallion Architecture
![cover](pic/retail_proj.png)

## 📖 Project Overview
Azure Retail Insight Lakehouse (ARIL) is a fully automated Azure Data Engineering solution designed to ingest, validate, transform, model, and visualize retail data coming from multiple sources.

This project demonstrates real-world, enterprise-grade practices using the **Medallion Architecture (Bronze → Silver → Gold)** along with workflow **Automation, Email Alerts, Transformations, and Analytics**.

#### Full Pipeline in ADF
![Full Pipe](pic/full_pipe.png)

## ✨ Key Features
🔹 1. Multi-Source Data Ingestion

  - Extract retail data from:
    - Azure SQL Database
    - GitHub raw files
  - Loaded into ADLS Bronze layer
  - Parallel ingestion for improved performance

  ## ✨ Data Creation
  First I have created a Schema & Multiple Tables inside Azure SQL Database, the code can be found [db.sql](db.sql). <br/>
  After that I have upload [customers.json](customers.json) file in a github repsitory so that I can fetch it using an API request.
  
  First I have used a **LookUp Activity** and to get the Tables Names under the **Retail** Schema.
  ![LookUp](pic/lookup_query.png)
  
  ### ✨ Extract retail Data
  After that I have used a **ForEach** activity so I can ingest the data from the tables parallely.<br/>
  Inside the **ForEach** I have two **Copy Activity** one for SQL Database and another to copy from Github, <br/>
  ![Inside](pic/inside_ForEach.png)
  
  Inside the first Copy this is the query to get the data from all the tables. <br/>
  ![Copy1](pic/Copy1.png)
  
  #### ✨ Logic App Email Confirmation
  After that i am send a **Confirmation Email** via a **LogicApp** for the completion of the bronze layer. I have actually created 2 more apps for silver and gold layers as well. <br/>
  ![pic/emailapp.png](pic/emailapp.png)
  Logic App Design is same for each one just parameters are different.<br/>
  ![logicdesign.png](pic/logicdesign.png)<br/>
  
  **Now we have our data in out bronze layer.**

## ✨ Data Transformation (DataBricks)
  🔹 2. Automated Workflow  
        - On successful Bronze load → Logic App sends confirmation email<br/>
        - Silver Databricks notebook triggers automatically<br/>
        - It processes the data and saves the data in **Delta** format.<br/>
        - On success → second confirmation email<br/>
        Check out the notebook --> [bronzeToSilver.ipynb](bronzeToSilver.ipynb)

   **Now we have our data in out Silver layer.**<br/>
        - Next Gold Databricks notebook triggers automatically<br/>
        - It creates **Dimesion, Fact, Aggregrate tables**.
        - Also I have implemented star schema.
   ![Star Schema](pic/starschema.png)

 **This entire workflow was done through Azure Data Factory**


 ## ✨ Data Analytics and Visualization
   🔹3. Downloaded the aggregrate file as csv and visualized data.
   ![graphs.png](pic/graphs.png)



