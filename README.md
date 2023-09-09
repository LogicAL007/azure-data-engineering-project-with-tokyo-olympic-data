# Azure Data Engineering: Tokyo Olympics Gender Participation & Medals Analysis

## Introduction

In this multifaceted project, we harness the power of Azure's data services to unveil not only the gender dynamics during the Tokyo Olympics but also the medals secured by each participating nation. Starting from raw data hosted on GitHub, we meticulously process this information using Azure's suite of tools and ultimately visualize our findings through Power BI.

## Data Source

Our primary dataset is the Tokyo Olympics dataset available on [kaggle](https://www.kaggle.com/datasets/arjunprasadsarkhel/2021-olympics-in-tokyo) . This data provides a detailed snapshot of every participant, inclusive of coaches, medals, Teams and gender.

## Objectives

1. Determine the average number of male versus female participants in the Tokyo Olympics.
2. Collate and analyze the number of medals (gold, silver, and bronze) acquired by each country during the event.
   
## Prerequisites

Before delving into the detailed workflow, it's imperative to set up access for Azure Databricks to interact with our Azure Blob Storage container (where our raw data is stored). Here's how you can achieve this:

### Service Principal & Permissions Setup:

1. **Azure Active Directory (Azure AD)**:
   
   - Navigate to the Azure portal.
   - Create a new service principal within Azure AD. Remember to note the 'Application ID', 'Authentication Key', and 'Tenant ID' once created.

2. **Assign Role in Azure Blob Storage**:

   - Go to the storage account containing the 'raw data' blob container.
   - Under 'Access control (IAM)', assign the 'Storage Blob Data Contributor' role to the newly created service principal. This provides Databricks the required permissions to read from and write to the container.

3. **Azure Databricks Configuration**:

   - Launch the Databricks workspace.
   - Under 'Workspace', create a new library.
   - Using the Databricks Utilities (`dbutils`), configure the following:

     ```python
     spark.conf.set(
       "fs.azure",
       "true")
     spark.conf.set(
       "fs.azure.account.key.<YOUR-STORAGE-ACCOUNT-NAME>.blob.core.windows.net",
       "<YOUR-ACCESS-KEY>")
     ```

   - Remember to replace `<YOUR-STORAGE-ACCOUNT-NAME>` with your actual Azure Blob Storage account name and `<YOUR-ACCESS-KEY>` with the access key or connection string for authentication.

Now that Azure Databricks can securely and seamlessly communicate with the Azure Blob Storage container, let's proceed with our data engineering workflow.

## Detailed Workflow


### 1. Data Ingestion with Azure Data Factory:

- **Pipeline Creation**: Begin by creating a robust pipeline in Azure Data Factory.
  
- **Data Activity Setup**: Implement the `Copy Data` activity. Ensure that the connector fetches data directly from the provided GitHub URL, ensuring data integrity.
  
- **Staging Area**: Designate the `raw_data` folder as the primary repository for the freshly ingested data, facilitating easier subsequent processing.

<img width="1280" alt="Screenshot 2023-09-06 183315" src="https://github.com/LogicAL007/azure-data-engineering-project-with-tokyo-olympic-data/assets/122959675/48ee044e-ffa3-4059-82b8-a5a95d6b3009">

### 2. Transformation & Preparation with Azure Databricks:

- **Workspace Initialization**: If you haven't already, set up a new Databricks workspace. This isolated environment is pivotal for scalable data processing.
  
- **Notebook Integration**: Import essential notebooks from `databricks_notebooks` into your workspace. These scripts are tailored to refine and transform the raw dataset.
  
- **Execution**: Systematically run the notebooks, ensuring each transformation step successfully cleans, structures, and preps the data for the next phase.

<img width="1280" alt="Screenshot 2023-09-06 201541" src="https://github.com/LogicAL007/azure-data-engineering-project-with-tokyo-olympic-data/assets/122959675/7a5f2474-f473-4b60-83cc-dd2ad54fd0a8">


### 3. Data Analysis using Azure Synapse Analytics:

- **Synapse Studio Setup**: Navigate to and configure your Azure Synapse Studio. Become well-acquainted with the environment, focusing particularly on the data ingestion and querying aspects then create a workspace for the task to be performed using the same resource group as your storage account.
<img width="1280" alt="Screenshot 2023-09-07 124757" src="https://github.com/LogicAL007/azure-data-engineering-project-with-tokyo-olympic-data/assets/122959675/9d447abb-d36b-4c1a-b36c-6b443e88f4fb">
- **lake database** : create and lake database and add all the transformed data as tables.
<img width="1280" alt="Screenshot 2023-09-07 204243" src="https://github.com/LogicAL007/azure-data-engineering-project-with-tokyo-olympic-data/assets/122959675/3825b927-9da8-404e-b311-5ef2803e8596">
  
- **Script writing**: create a SQL scripts from on synapse analytics and write your queries. These quieries are tailored to provide insights about the gender distribution and medal count for each country.
  
- **Comprehensive Analysis**: Execute SQL scripts to not only derive the male-to-female participant ratio but also consolidate medals by country. Ensure your results are formatted suitably for visualization.
<img width="1280" alt="Screenshot 2023-09-07 205758" src="https://github.com/LogicAL007/azure-data-engineering-project-with-tokyo-olympic-data/assets/122959675/68bba5f7-faf3-4516-968c-9e6866ccd221">

### 4. Visualization in Power BI:

- **Power BI Setup**: Open Power BI Desktop and configure any necessary plugins or connectors.
  
- **Data Connection**: Seamlessly link Power BI to your processed dataset in Azure Synapse Analytics. Test the connection thoroughly to ensure real-time data sync.
  
- **Visualization**: Utilize pre-built templates in `powerbi_reports` or create intuitive custom visualizations that best represent the data insights.
  
- **Publication**: Once satisfied, publish the finalized report to Power BI Service. Here, you can set up scheduled refreshes, share the report, and collaborate with stakeholders.


## Extended Analysis Suggestions

1. **Medal Distribution by Sport**: Delve into each sport to see which countries dominated or underperformed. This can provide insights into specific sports strengths or interests by country.

2. **Gender & Medal Correlation**: Investigate if countries with a more balanced gender participation had better medal tallies or vice versa.

3. **Performance Over Previous Olympics**: Alongside gender trends, consider exploring a nation's performance trajectory over the past few Olympics, potentially revealing patterns of growth or decline.

