#  											**RETAIL BUSINESS PROJECT**





**Description:**

* We have to build an end to end data pipeline for retail client.
* We have data coming from multiple data sources such as Azure SQL DB and API in different format.
* We have to bring the data into data lake (ADLS).



**Data Creation:**

* First we will create a **SQL Database.**
* login **-  newadmin,** pass **- Rahul@\*08 |** Compute+Storage - use basic | CONNECTIVITY METHOD - public
* Query available in sql file.
* The customers.json data is in a GitHub repo adf-dev inside data folder.



**ADLS Creation:**

* Create a ADLS hierarchial storage account.
* Create container retail.
* Create 3 sub directories bronze, silver, gold.



1. Whenever we have multiple data source and we have to bring data into **ADLS** we use **ETL** operation so we use **ADF**

 	- First we will use ADF to get the data from DB and API into bronze layer.

 	- Inside ADF we first use LookUp to get all the table names inside a schema.



-- Use this Query in Lookup activity

SELECT

    TABLE\_SCHEMA AS SchemaName,

    TABLE\_NAME AS TableName

FROM

    INFORMATION\_SCHEMA.TABLES

WHERE

    TABLE\_TYPE = 'BASE TABLE' and TABLE\_SCHEMA = 'retail'

ORDER BY

    SchemaName, TableName;



 	- Then we use For each activity.

 	- Inside For each we use copy data activity and we use dynamic query.

 	- @concat('SELECT \* FROM ',item().SchemaName,'.',item().TableName)

 	- For sink create 2 dataset parameter tablename , schemaname and them set them to @item().Schemaname, @item.TableName

 	- After that set file path in data the way we want it \[@concat('bronze/',dataset().schemaname,'/',dataset().tablename)],\[@concat(dataset().tablename,'.parquet')]

 	So now we have out pipeline to copy into bronze

&nbsp;	- use another copy activity to get data from GitHub, linked service type will be http.

 

 1.1 **Now we want email notification on success of the pipline**

 	- create logic app trigger **when http request is received, Post** method and generate schema like

 	{

    		"to" : "dadas",

    		"subject" : "dasdas",

 		"msgBody" : "dasdsa"

 	}

 	- Add **web** to the end of bronze pipeline

 	- it will create the schema automatically, then save and copy the generated URL. Put it inside the web hook activity URL

 	- create **Pipeline Parameters for email - to, subjectforsuccess, msgforsuccess, subjectforfailure, msgforfailure.**

 	- Inside body use same style and select the parameter

{

    "to" : "@{pipeline().parameters.to}",

    "subject" : "@{pipeline().parameters.subjectForSuccess}",

    "msgBody" : "@{pipeline().parameters.messageForSuccess}"

}



 	- In head add Name - Content-Type, value - application/json

 	- Add a parse json action between

 	- content choose body from previous

 	- use sample payload to generate schema and give the same schema as 1.1



 	- Then add send email in logic app.



2\. **Next Databricks.**

&nbsp;	- Create workspace

&nbsp;	- Create Cluster personal compute, star cluster

&nbsp;	- Create notbook, Mount ADLS using,

dbutils.fs.mount(

&nbsp; source = "wasbs://containername@storagename.blob.core.windows.net",

&nbsp; mount\_point = "/mnt/retail\_project",

&nbsp; extra\_configs = {"fs.azure.account.key.storagename.blob.core.windows.net":"secret access key"})



&nbsp;	- To get databricks access token click on account icon then settings then developers then generate new token access token 

 

