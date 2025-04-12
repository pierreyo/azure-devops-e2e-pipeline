# Azure End-to-End Data Engineering Project with Azure DevOps


## Project Overview

This project focuses on building a complete Azure data engineering solution incorporating **Azure DevOps for Continuous Integration and Continuous Delivery (CI/CD)** . It covers building dynamic and parameterized pipelines in Azure Data Factory with data validation, performing big data transformations using PySpark in Azure Databricks, and leveraging Delta Live Tables for the Gold layer . The project starts from scratch, gradually increasing in complexity . The data source for this project is the **Paris 2024 Olympic Summer Games data set** .

## Key Technologies Used

*   **Azure DevOps:** Used for implementing CI/CD concepts in an Azure data engineering context . This includes using **Azure Repos** for version control, **Azure Pipelines** for building and releasing, and **Pull Requests** for code review and merging .
*   **Azure Data Factory (ADF):** Employed as the ETL/ELT tool for data ingestion. The project focuses on building **dynamic parameterized pipelines** and implementing data validation steps . It uses **linked services** to connect to data sources like HTTP (for GitHub API) and Azure Data Lake Storage Gen2 .
*   **Azure Databricks:** Utilized for big data transformations using **PySpark** . The project covers creating enriched entities and building Databricks workflows for dynamic data transformation . It also introduces **Delta Lake** and **Delta Live Tables** for building the Gold layer.
*   **Azure Data Lake Storage Gen2:** Serves as the data lake for storing data in different layers (Bronze/Raw) [2, 13].
*   **HTTP Connector:** Used in Azure Data Factory to ingest data from the GitHub API [2, 7].
*   **Delta Lake:** Employed within Azure Databricks for reliable data lakes and building the Gold layer using Delta Live Tables [1, 9, 11].
*   **Delta Live Tables (DLT):** A framework within Azure Databricks used to build and manage data pipelines for the Gold layer with features like expectations for data quality and change data capture (CDC) .

## Project Architecture

The project architecture involves the following key stages:

1.  **Data Source:** Data is sourced from **CSV files** related to the Paris 2024 Olympics, potentially downloaded from Kaggle or the provided GitHub repository . Azure Data Factory is used to pull data, demonstrating scenarios of fetching files directly from an API .
2.  **Bronze Layer (Raw):** Raw CSV files are ingested into Azure Data Lake Storage Gen2 using Azure Data Factory . The project demonstrates dynamic ingestion of multiple files using parameterized pipelines and a ForEach loop . It also covers handling potential data quality issues during ingestion.
3.  **Silver Layer (Transformed):** Data is read from the Bronze layer by Azure Databricks. Transformations are performed using PySpark to create Silver entities . The project showcases reading data in different formats (CSV and Parquet), applying transformations like filling null values and filtering, and saving the transformed data as Delta tables in Azure Data Lake Storage Gen2 . Dynamic data reading using Databricks Workflows is also demonstrated .
4.  **Gold Layer (Curated):** The Gold layer is built using **Delta Live Tables (DLT)** in Azure Databricks . DLT pipelines are created to define data transformations, apply data quality expectations, and potentially handle change data capture (CDC) for the athletes' data .
5.  **CI/CD with Azure DevOps:** The entire project lifecycle, from development to deployment, is managed using Azure DevOps . This includes:
    *   **Azure Repos:** For version control of ADF pipelines and Databricks notebooks . Feature branches are created for development, and changes are merged into the main branch using pull requests .
    *   **Azure Pipelines:** Implicitly used through the publish feature of ADF to move changes to the live mode . The concept of deploying changes to different environments (QA, Prod) using the ADF publish branch is also mentioned .
    *   **Pull Requests:** Used for merging feature branches into the main branch, allowing for code review and tracking changes .

## Setup Instructions

To set up and run this project, you will need an **Azure subscription** .

Here's a high-level overview of the setup process:

1.  **Create an Azure Account:** If you don't have one, follow the instructions in the video to create a free Azure account .
2.  **Create a Resource Group:** Create an Azure Resource Group to organize all your project resources .
3.  **Create Azure Data Lake Storage Gen2 Account:** Create a storage account configured for Data Lake Gen2 to store the data . Create containers (e.g., `bronze`, `silver`, `gold`, `source`).
4.  **Create Azure Data Factory:** Create an Azure Data Factory instance for orchestrating the data pipeline .
5.  **Configure Azure DevOps Organization and Project:** Create an Azure DevOps organization and a project .
6.  **Connect ADF to Azure DevOps Git Repository:** Configure your ADF instance to connect to the Azure DevOps Git repository for version control . Create a repository if you don't have one . Understand the concepts of the main branch, feature branches, and the ADF publish branch .
7.  **Create Linked Services in ADF:**
    *   Create an **HTTP linked service** to connect to the GitHub repository containing the CSV files (using the raw file URL) .
    *   Create an **Azure Data Lake Storage Gen2 linked service** to connect to your storage account .
8.  **Create Datasets in ADF:** Create parameterized datasets for the source (HTTP) and sink (Azure Data Lake Storage Gen2) .
9.  **Build Dynamic Pipelines in ADF:** Implement pipelines to ingest CSV files from the source to the Bronze layer using a Lookup activity to read metadata and a ForEach activity to iterate and copy data . Implement logic to handle specific files (like `Nox.csv`) with metadata validation.
10. **Create Azure Databricks Workspace:** Create an Azure Databricks workspace, preferably using the Trial (Premium) plan . Understand the concept of the managed resource group .
11. **Enable Unity Catalog in Databricks:** Follow the steps outlined in the video (and potentially the linked video) to enable the "Manage Account" button and create a Unity Metastore . Attach your Databricks workspace to the Unity Metastore . Create external locations and storage credentials in Unity Catalog to access the data in Azure Data Lake Storage Gen2 .
12. **Develop Databricks Notebooks:**
    *   Create notebooks to read data from the Bronze layer (Delta and CSV formats) .
    *   Perform data transformations in the Silver layer using PySpark . Save the transformed data as Delta tables in the Silver external location .
    *   Create Databricks Workflows to orchestrate the reading and writing of Silver entities .
    *   Create notebooks for the Gold layer using Delta Live Tables (DLT) . Define streaming tables, views, and apply data quality expectations using `@dlt.expect` . Explore the use of `applyChanges` API for CDC on the athletes' data .
