# End-to-End Power BI & SQL Data Engineering Project

## Project Overview
This project demonstrates a complete Business Intelligence lifecycle, simulating a real-world scenario where data is migrated, transformed, and reported on across multiple environments.

The workflow moves from a **Test Environment (SQL Server)** to a **Production Environment (MySQL)**, integrating with **Power BI** for analytics and reporting.

**Key Features:**
- **Data Engineering**: Data cleaning, transformation using SQL (Joins, Updates, Filtering).
- **Environments**: Managing Test (SQL Server) vs. Production (MySQL) databases.
- **Power BI**: Data modeling, DAX measures, and dynamic visualization.
- **Migration**: Converting T-SQL logic to MySQL logic for production deployment.

## Repository Structure

```
├── data/
│   ├── raw/                # Raw CSV data files (Inventory, Products)
│   └── reference/          # Reference documents (DAX Formulae)
├── sql/
│   ├── sql_server/         # T-SQL scripts for Test Environment
│   │   ├── 1_test_env_transformation.sql
│   │   └── 2_prod_env_cleaning.sql
│   └── mysql/              # MySQL scripts for Production Environment
│       ├── 1_prod_env_setup_cleaning.sql
│       └── 2_prod_env_transformation.sql
├── docs/
│   ├── reports/            # Final Output PDF Reports
│   └── screenshots/        # Report Visualizations
└── README.md
```

## Workflow

### 1. Test Environment (SQL Server)
The project starts by setting up the data infrastructure in **Microsoft SQL Server**.
- **Data Import**: Raw inventory and product data is imported.
- **Transformation**: `sql/sql_server/` scripts handle data cleaning and joining tables (LEFT JOINs) to create a reporting-ready dataset (e.g., `new_table`).
- **Reporting**: Power BI connects to this test data to build the initial model and measures.

### 2. Production Environment (MySQL)
Once verified, the solution is migrated to **MySQL**, simulating a production backend.
- **Setup**: `sql/mysql/` scripts replicate the logic, adapted for MySQL syntax.
- **Data Migration**: Data is cleaned and structured for the production schema.

### 3. Power BI Integration
- The Power BI report is updated to switch from the SQL Server source to the MySQL Production source using Power Query.
- The final report includes verified DAX measures and visualizations.

## Prerequisites
- **Microsoft SQL Server** & SSMS
- **MySQL Server** & MySQL Workbench
- **Power BI Desktop**

## Getting Started
1.  **Setup Databases**: Run the scripts in `sql/sql_server/` to set up your Test DB.
2.  **Run Transformations**: Execute the transformation logic to prepare tables.
3.  **Power BI**: Connect Power BI to the SQL Server database for initial development.
4.  **Migrate**: Run `sql/mysql/` scripts to set up Prod DB.

## Key DAX Measures
The report utilizes the following DAX formulae to derive key insights from the `Demand/Availability Data` table:

| Measure | Formula | Description |
| :--- | :--- | :--- |
| **Total Availability** | `SUM('Demand/Availability Data'[availability])` | Total units available in inventory. |
| **Total Demand** | `SUM('Demand/Availability Data'[demand])` | Total units requested by orders. |
| **Total Days** | `DISTINCTCOUNT('Demand/Availability Data'[Order_Date_DD_MM_YYYY])` | Number of distinct operating days in the dataset. |
| **Avg Availability** | `DIVIDE([Total Availability], [Total Number of Days])` | Average daily inventory available. |
| **Avg Demand** | `DIVIDE([Total Demand], [Total Number of Days])` | Average daily order demand. |
| **Total Profit** | `SUMX(FILTER('Demand/Availability Data', [LOSS/PROFIT] > 0), [LOSS/PROFIT] * [unit_price])` | Revenue generated from met demand. |
| **Total Loss** | `SUMX(FILTER('Demand/Availability Data', [LOSS/PROFIT] < 0), [LOSS/PROFIT] * [unit_price]) * -1` | Potential revenue lost due to stockouts. |

## Report Preview

**[📄 Download Final PDF Report](docs/reports/Prod_Report_Output.pdf)**

### Analysis Dashboards
![Report Page 1](docs/screenshots/Report_Page_1.png)
![Report Page 2](docs/screenshots/Report_Page_2.png)


