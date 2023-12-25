
# Tokyo Olympics Data Insights: Harnessing IBM Cognos for Dynamic Data Visualization and Microsoft Azure for Robust Data Storage and Pipelines
## Description - 
#### Embarking on the "Tokyo Olympics Data Insights" project, we seamlessly integrate technologies. Through Microsoft Azure, we establish a secure data repository on GitHub and deploy Azure Data Factory for streamlined data pipelines. Azure Databricks processes raw data, while Azure Synapse Analytics enhances analytical capabilities. App Registrations ensure secure data access, and our Spark-powered transformations yield dynamic insights. The transformed data is stored and organized in Azure Data Lake Storage. We leverage IBM Cognos for dynamic visualization, crafting interactive reports that unlock the full potential of Tokyo Olympics data. This project not only optimizes data storage and processing but also delivers compelling visual narratives for enhanced decision-making.
## Key Feautres
### Data Lake Storage Gen 2
* Scalable and secure foundation for storing vast volumes of Olympic data.
* Ensures data integrity and accessibility, setting the stage for comprehensive analytics.
### Azure Databricks
* Advanced analytics and machine learning capabilities with Apache Spark.
* Used in Data exploration, Data processing and building ML Models.
### Azure Data Factory
- Orchestrates a seamless data pipeline for streamlined information flow.
- Ensures comprehensive analytics through efficient data processing.
### Azure Synapse Tools
- Empowers seamless exploration, analysis, and visualization at unprecedented speed and scale.
- Expands the boundaries of data comprehension.
### IBM Cognos Analytics
- Takes center stage in transforming raw data into dynamic visualizations.
- Crafts vivid charts that bring the Tokyo Olympics data to life.

# Step To Create The Project
Step 1: Create a repository on GitHub to store the data for implementing the real-life scenario.

Step 2: Create a storage account. In the storage account, create containers, and within each container, create two folders: one for raw data and another for transformed data.
![App Screenshot](https://resumezcaler.blob.core.windows.net/sahilresume/SS1.png)
Step 3: Create an Azure Data Factory. In the Data Factory, design a pipeline to link data from GitHub to the raw data folder in Azure Data Lake Storage within the specified storage account.
![App Screenshot](https://resumezcaler.blob.core.windows.net/sahilresume/SS2.png)
Step 4: Set up Azure Databricks, ensuring the region is Southeast Asia.

Step 5: Configure a compute resource in Azure Databricks to perform operations on the raw data.
![App Screenshot](https://resumezcaler.blob.core.windows.net/sahilresume/SS3.png)
Step 6: Mount the data in Azure Data Lake Storage to Azure Databricks. Provide authentication in the backend to access it, using the credentials obtained from an App Registration.

Step 7: Create an App Registration and save the Client ID and Tenant ID. Generate a key for this app and save its value.

Step 8: In Azure Databricks, use a Python notebook and the provided configuration code to connect Azure Data Lake Storage with Databricks.

                 configs = {
                   "fs.azure.account.auth.type": "OAuth",
                   "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
                   "fs.azure.account.oauth2.client.id": "YOUR_CLIENT_ID",
                   "fs.azure.account.oauth2.client.secret": 'YOUR_CLIENT_SECRET',
                   "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/YOUR_TENANT_ID/oauth2/token"
                 }
                 
                 dbutils.fs.mount(
                   source="abfss://tokyoolympicdata@tokyolympic.dfs.core.windows.net",
                   mount_point="/mnt/tokyoolympic",
                   extra_configs=configs
                 )
Step 9: Add access management for authorization. Go to the container housing raw and transformed data, and in Access Control settings (IAM), add a role assignment. Assign the Storage Blob Data Contributor role to the App Registration (e.g., app01).

Step 10: Transform the data using Spark commands in the Databricks command line.

Step 11: Transform the data as needed. For example:
                
                 top_gold_medal_countries = medals.orderBy("Gold", ascending=False).select("Team/NOC","Gold").show()
                 
                 average_entries_by_gender = entriesgender.withColumn(
                   'Avg_Female', entriesgender['Female'] / entriesgender['Total']
                 ).withColumn(
                   'Avg_Male', entriesgender['Male'] / entriesgender['Total']
                 )
                 
                 average_entries_by_gender.show()
Step 12: After transforming the data, rewrite and paste it into the transformed data folder in the container.

     athletes.repartition(1).write.mode("overwrite").option("header", 'true').csv("/mnt/tokyoolympic/transformed-data/athletes")
 Repeat for other dataframes (coaches, entriesgender, medals, teams)
![App Screenshot](https://resumezcaler.blob.core.windows.net/sahilresume/SS4.png)
Step 13: Create Azure Synapse Analytics in the Southeast Asia region using the same resource group and storage account.

Step 14: In Synapse Analytics, create tables in the Lake Database.

Step 15: Attach these tables to the given data in Azure Data Lake Storage. Browse the files in the "input Folder" section, validate, and publish the data.

Step 16: Create SQL queries for the tables:


                      SELECT Country, COUNT(*) AS TotalAthletes
                      FROM Athletes
                      GROUP BY Country
                      ORDER BY TotalAthletes DESC;
                      
                      SELECT Country,
                        SUM(Gold) total_gold,
                        SUM(Silver) total_silver,
                        SUM(Bronze) total_bronze
                      FROM Medals
                      GROUP BY Country
                      ORDER BY total_gold DESC;
                      
                      SELECT Discipline,
                        AVG(Female) avg_female,
                        AVG(Male) avg_male
                      FROM EntriesGender
                      GROUP BY Discipline;

![App Screenshot](https://resumezcaler.blob.core.windows.net/sahilresume/SS5.png)
Step 17: Visualize the data.

By following these steps, you can efficiently set up a data processing pipeline, perform transformations, and analyze the data using Azure services.







## Data Vizualisation with IBM Cognos

![App Screenshot](https://resumezcaler.blob.core.windows.net/sahilresume/viz1.png)
![App Screenshot](https://resumezcaler.blob.core.windows.net/sahilresume/viz2.png)
![App Screenshot](https://resumezcaler.blob.core.windows.net/sahilresume/viz3.png)
