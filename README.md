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
Cities by lowest sales
```DAX
Bot 3 Cities = 
CONCATENATEX(
    TOPN(
        3,
        SUMMARIZE(
            Total_Sales,
            Total_Sales[City],
            "Revenue",[Total Revenue By City]
        ),
        [Total Revenue],
        ASC
    ),Total_Sales[City],
    ", ",
    [Total Revenue],
    ASC
)
```
Months by lowest sales
```DAX
Bottom 3 Months = 
CONCATENATEX(
    TOPN(
        3,
        SUMMARIZE(
            Total_Sales,
            'Calendar'[Month Name],
            "Revenue",[Total Revenue By Month]
        ),
        [Total Revenue],
        ASC
    ),'Calendar'[Month Name],
    ", ",
    [Total Revenue],
    ASC
)
```
Top products ("Others" included)
```DAX
Rank Top = 
    if([Top Products Revenue] <> BLANK(),
    RANKX(TOPN(
                SELECTEDVALUE('Top N Selection'[Value]),
                ALLSELECTED('Products Table'),
                [Total Revenue],
    ),
    [Total Revenue],
    ,
    DESC,Dense)
    )
```
Top cities by sales
```DAX
Top 3 Cities = 
CONCATENATEX(
    TOPN(
        3,
        SUMMARIZE(
            Total_Sales,
            Total_Sales[City],
            "Revenue",[Total Revenue By City]
        ),
        [Total Revenue],
        DESC
    ),Total_Sales[City],
    ", ",
    [Total Revenue],
    Desc
)
```
Top products revenue
```DAX
Top Products Revenue = 
    var topNselected = SELECTEDVALUE('Top N Selection'[Value])

    var topProdtable = 
        topn(topNselected,ALLSELECTED('Products Table'),[Total Revenue])
    
    var topProdSales = 
    CALCULATE([Total Revenue],KEEPFILTERS(topProdtable))

    var otherSales = 
        CALCULATE([Total Revenue],ALLSELECTED('Products Table')
        ) - 
        calculate(
            [Total Revenue],topProdtable
        )
    var currentProd =
        SELECTEDVALUE('Products Table'[Product])
    return 
    if(currentProd <> "Others",
    topProdSales,
    othersales
    )
```
Total Orders
```DAX
Total Orders = COUNTROWS(Total_Sales)
```
Total Unique products 
```DAX
Total Products Sold = 
 DISTINCTCOUNT(Total_Sales[Product])
```
Total Revenue
```DAX
Total Revenue = 
    SUMX(Total_Sales,Total_Sales[Quantity_Ordered] * Total_Sales[Price_Each])
```
Latest Date
```DAX
latest Date = 
LASTDATE('Calendar'[Start of Month])
```
Latest MoM sales cahnge % 
```DAX
Latest Mom Sales change % = 
    VAR ld = [latest Date]
    var Current_month_sales = [Latest Month Total Revenue]
    var Previous_month_sales = CALCULATE([Total Revenue],'Calendar'[Start of Month] = EDATE(ld,-1))
    RETURN
    DIVIDE(Current_month_sales - Previous_month_sales, Previous_month_sales)
```
Latest Month total Revenue
```DAX
Latest Month Total Revenue = 
    VAR ld = [latest Date]
RETURN
    CALCULATE(
        [Total Revenue],
        'Calendar'[Start of Month] = ld
    )
```
MoM Sales change %
```DAX
Latest Month Total Revenue = 
    VAR ld = [latest Date]
RETURN
    CALCULATE(
        [Total Revenue],
        'Calendar'[Start of Month] = ld
    )
```
Total revenue by previous month
```DAX
Total Revenue Previous Month = 
    CALCULATE([Total Revenue],PREVIOUSMONTH('Calendar'[Date]))
```
Month name derived from the column month
```DAX
Month Name = FORMAT('Calendar'[Date],"MMM")
```
Top N selection for filtering products
```DAX
Top N Selection = {1,2,3,4,5}
```

### Results/Findings

- Cities by highest sales are the following: San Francisco, Los Angeles, New York
- Cities by lowest sales are the following: Austin, Portland, Seattle
- Months by highest sales are the following: December, October, April
- Months by lowest sales are the following: January, September, February
- Sales are grown from year start to end
