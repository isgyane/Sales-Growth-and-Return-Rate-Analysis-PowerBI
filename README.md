# Time Intelligence Analysis of Sales Data in Power BI
Businesses require robust analytical tools to derive insights from their sales data. This project focuses on conducting a time intelligence analysis using Power BI, leveraging an extended sales dataset. The primary goal is to analyze sales trends over time, calculate year-on-year growth, and provide actionable insights for strategic decision-making.

## Data Overview
The dataset used for this analysis includes comprehensive sales records, encompassing various dimensions such as date, product, region, and sales figures. This extended dataset allows for in-depth analysis and reporting.

## Methodology
### 1. Creating a Dedicated Calendar Table
A dedicated calendar table is essential for time intelligence analysis. The following steps were undertaken:
Custom Calendar Period: The calendar table was designed to accommodate a custom fiscal year, allowing flexibility in reporting periods that differ from the standard calendar year.
Date Range: The table includes all necessary date attributes such as Year, Month, Quarter, Week, and Day, enabling detailed time-based analysis.

**DAX Code for Calendar Table:**

```
Calendar = 

    var min_date = MIN(Sales[Date]) // the minimum date
    var max_date = MAX(Sales[Date]) // the maximum date
  
    RETURN CALENDAR(
        DATE(YEAR(min_date), 1, 1), DATE(YEAR(max_date), 12, 31)
)
```

### 2. Year-on-Year Sales Growth Calculations
To assess the performance of sales over time, year-on-year growth calculations were performed. This involved:
**Calculating Year-To-Date Sales:**
```
YTD Sales = 
    IF(
        ISINSCOPE('Calendar'[Quarter]),
        TOTALYTD(SUM(Sales[Total_Sales]), 'Calendar'[Date])
    )
```


**Prior Year Sales Calculation**
```PriorYearSales = 

    CALCULATE(
        SUM(Sales[Total_Sales]),
        PARALLELPERIOD('Calendar'[Date], -12, MONTH)
    )
```

**Year-on-Year Growth:**
```
Sales YoY Growth = 
    VAR SalesPriorYear = CALCULATE(
            SUM(Sales[Total_Sales]),
            PARALLELPERIOD('Calendar'[Date], -12, MONTH)
        )

    RETURN
        DIVIDE(
            SUM(Sales[Total_Sales]) - SalesPriorYear,
            Sales[PriorYearSales]
        )
```
