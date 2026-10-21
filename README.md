
# SQL Data Warehouse Project

## Project Overview

This project demonstrates the design and implementation of a SQL Server Data Warehouse using an ETL (Extract, Transform, Load) pipeline and the Medallion Architecture.

The data warehouse integrates data from CRM and ERP source systems, processes it through Bronze, Silver, and Gold layers, and transforms raw data into clean, structured, and analytics-ready datasets.

The final Gold layer follows a star schema design, providing a unified view of customers, products, and sales to support data analysis and reporting.

## Architecture

The project follows a three-layer architecture:

**Source Systems → Bronze → Silver → Gold → Data Analytics**

### Bronze Layer — Raw Data Ingestion

The Bronze layer stores raw data loaded from CSV files originating from CRM and ERP systems.

Key operations:

- Created six raw tables to store source data.
- Loaded CSV files using SQL Server's `BULK INSERT` statement.
- Used `TRUNCATE TABLE` before loading new data.
- Implemented the `bronze.load_bronze` stored procedure to orchestrate data loading.
- Added execution messages, load duration tracking, and error handling.

### Silver Layer — Data Cleansing and Transformation

The Silver layer cleans, standardizes, and transforms raw data to improve data quality and consistency.

Key transformations:

- Removed leading and trailing whitespace from text fields.
- Standardized categorical values, including gender, marital status, product lines, and countries.
- Removed duplicate customer records by retaining the most recent record for each customer ID.
- Validated date fields and converted integer-based dates into SQL date types.
- Handled invalid, missing, or inconsistent values.
- Corrected sales amounts and prices using quantity and price information.
- Standardized customer identifiers across source systems.
- Transformed product keys and derived category identifiers.
- Added data warehouse load timestamps to track record processing.

The `silver.load_silver` stored procedure manages data transformation and loading from Bronze into Silver.

### Gold Layer — Business-Ready Data

The Gold layer contains three SQL views designed for analytical consumption. These views combine and enrich data from the Silver layer.

| View | Description |
|---|---|
| `gold.dim_customers` | Customer information, including demographics, country, marital status, and birthdate. |
| `gold.dim_products` | Product information, including category, subcategory, cost, product line, and current product records. |
| `gold.fact_sales` | Sales transactions, including order dates, shipping dates, product and customer keys, sales amounts, quantities, and prices. |

The Gold layer follows a star schema design, with customer and product dimensions connected to the sales fact view.

Surrogate keys are generated for customers and products using `ROW_NUMBER()`.

## Data Sources

The project uses six CSV files from two source systems.

### CRM

- `cust_info.csv` — Customer information.
- `prd_info.csv` — Product information.
- `sales_details.csv` — Sales transaction details.

### ERP

- `CUST_AZ12.csv` — Customer birthdates and gender.
- `LOC_A101.csv` — Customer location and country.
- `PX_CAT_G1V2.csv` — Product categories, subcategories, and maintenance information.

## Technologies Used

- **Microsoft SQL Server** — Database engine and data warehouse implementation.
- **T-SQL** — Data definition, transformation, and ETL logic.
- **Stored Procedures** — Reusable ETL execution and orchestration.
- **BULK INSERT** — Loading data from CSV files.
- **SQL Views** — Creating analytics-ready datasets.
- **Data Quality Testing** — Validating data consistency and transformation results.

## ETL Workflow

The pipeline consists of the following steps:

1. **Create database schemas and tables:** Define the Bronze and Silver table structures.
2. **Load Bronze:** Execute `bronze.load_bronze` to ingest the source CSV files.
3. **Transform Silver:** Execute `silver.load_silver` to cleanse and standardize the data.
4. **Create Gold views:** Define the customer dimension, product dimension, and sales fact views.
5. **Run data quality tests:** Validate transformed records and analytical relationships.

Example procedure execution:

```sql
EXEC bronze.load_bronze;
EXEC silver.load_silver;
```

Run the Gold view creation script after the Bronze and Silver layers have been created and populated.

## Data Quality and Testing

Data quality checks were performed throughout the project to verify the accuracy and consistency of the transformed data.

Examples of validation and cleansing logic include:

- Identifying and filtering records with missing customer identifiers.
- Removing duplicate customer records.
- Validating date formats and handling invalid dates.
- Standardizing categorical values across CRM and ERP sources.
- Checking consistency between sales amounts, quantities, and prices.
- Validating customer and product relationships used in the Gold layer.

These checks help ensure that the resulting datasets are more reliable and suitable for downstream analytical use.

## Getting Started

### Prerequisites

- Microsoft SQL Server.
- SQL Server Management Studio (SSMS) or another compatible SQL client.
- Access to the project's source CSV files.
- Appropriate permissions to create schemas, tables, views, and stored procedures, and to execute bulk imports.

### Setup Instructions

1. Clone or download this repository.
2. Create or select the target SQL Server database.
3. Create the required `bronze`, `silver`, and `gold` schemas.
4. Execute the SQL scripts in the appropriate order to create the tables and stored procedures.
5. Update the CSV file paths in the Bronze loading procedure to match your local environment.
6. Execute the Bronze and Silver loading procedures.
7. Run the Gold view creation script.
8. Execute the data quality tests and query the Gold views.

**Important:** The loading procedures truncate existing layer tables before reloading data, and the DDL scripts drop and recreate tables. Back up any data you need to preserve before running them.

## Project Goals

This project demonstrates practical skills in:

- Designing a layered data warehouse architecture.
- Implementing ETL workflows using SQL Server and T-SQL.
- Integrating data from multiple source systems.
- Cleaning, standardizing, and validating data.
- Developing stored procedures for repeatable data loading.
- Modeling analytical data using dimensions and fact views.
- Preparing structured datasets for data analysis and reporting.
