# Sales_Analysis
Insights about product sales for year 2019


## Table of Contents

- [Project Overview](#project-overview)
- [Tools](#tools)
- [Data Preparation](#data-preparation)
- [Explaratory Data Analysis](#explaratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results/Findings](#resultsfindings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)

### Project Overview

![image](https://github.com/user-attachments/assets/7d04fda8-5f19-47fa-af3a-937239f19353)

### Tools

- Data Visualization - PowerBI [Download here](https://www.microsoft.com/en-us/download/details.aspx?id=58494)
- Data Input - SQL Server Management Studio 20 [Download here](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16#download-ssms)

### Data Preparation


The initial data took several tasks to elaborate
1) Data Loading and inspection
2) Handling Missing Values
3) Data formating
4) Create a Calendar Table
5) Format month table
6) Includes custom measure written in DAX
7) Build relationships between tables


### Explaratory Data Analysis 


EDA involved in the following report to answer key questions, such as:

- Top products sold
- Total products sold
- Total revenue
- % of sales between months
- top months, cities, products by sales
- Top 5 products

### Data Analysis Expressions

Data Analysis featured following DAX:

Average order value
```DAX
Average Order Value = 
 [Total Revenue] / [Total Orders]
```

