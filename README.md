🛍️ Retail Sales Analysis in Power Query & Power Pivot
Customer Purchasing Behavior Analysis Using Online Retail II

Consolidation and cleaning of transactional data from two yearly tables (2010 and 2011) to ensure accurate KPI calculations in the dashboard.

📁 Dataset and Dashboard Download  
The full Excel dataset and the Query dashboard are available here: [Download Dataset](https://www.kaggle.com/datasets/fedefigueiras/retail-sales-analysis-project)


📦 Dataset Information
Online Retail II contains all transactions for a UK‑based online gift retailer during 2010–2011.
The company sells unique all‑occasion giftware, with many customers being wholesalers.

Source: https://archive.ics.uci.edu/dataset/502/online+retail+ii

📁 Dataset Overview
The dataset includes:

Transactions: InvoiceNo, StockCode, Description, Quantity, UnitPrice

Customers: CustomerID, Country

Dates: InvoiceDate

Returns/Cancellations: Invoices starting with “C”

⚠️ Pain Points:

Missing customer IDs

Negative quantities and prices

Duplicates

500k+ rows of raw, unclean data

🎯 Project Objectives
Build a Power Query cleaning pipeline

Create a Power Pivot data model

Develop DAX measures

Design an Excel dashboard with relevant KPIs

⚙️ Power Query Data Preparation (M Code):
Consolidation and cleaning of transactional data from two yearly tables (2009–2010 and 2010–2011) to ensure accurate KPI calculations in the dashboard.

🧩 Key Steps
Combine datasets: Merge both yearly tables into one unified source.

Filter integrity to remove:

Null invoices

Cancellations (invoices starting with “C”)

Negative or zero quantities

Negative or zero prices

Missing customer IDs

Text normalization: Trim spaces in Description, Country, StockCode.

Sales calculation: Add Sales = Quantity × Price.

Date enrichment: Extract the Year from InvoiceDate.

Currency formatting: Convert Sales to currency type.

Final filter: Exclude 2009 because the data is incomplete.

💡 Insight:
This transformation ensures that only valid, positive, customer‑linked transactions feed into the Excel dashboard, preventing distortions in KPIs such as total sales, average order value, and customer counts.

📊 Sales Performance Dashboard:
A clear year‑over‑year comparison of sales metrics between 2010 and 2011, highlighting trends in revenue, customer engagement, and product performance.

![image alt](https://github.com/fedefig/Retail-Sales-Analysis-in-Power-Query/blob/main/Sales%20dashboard.png?raw=true)


🔍 Main Observations:

Stable KPIs: Both years show identical headline metrics — €8.7M in sales, 4,231 customers, and 18,325 invoices — indicating consistent business volume.

Slight Decline: A 4.4% YoY drop in total sales suggests minor contraction despite stable customer counts.

Seasonality: Strong peaks in Q4, typical of end‑of‑year demand cycles.

Customer Growth: Customer counts fluctuate but show resilience, with notable spikes in months 10–12.

Product Mix Evolution:

2010 top sellers: T‑shirts, mugs, stationery

2011 top sellers: handbags, caps, leather goods — indicating a shift toward higher‑value merchandise

Data Filters: Interactive slicers for year, month, quarter, and semester enable granular exploration.

💡 Interpretation:
The dashboard demonstrates a mature, steady sales structure with evolving product strategy and clear seasonal patterns.
It highlights how data cleaning, modeling, and visualization can produce actionable business insights.







Appendix:
DAX MEASURES

⭐ 1. Total Sales
This is the core measure of your model.
DAX
Total Sales :=
SUM ( FactSales[Sales] )
Why it matters:
Every other KPI (YoY, MoM, Top Products, Country Analysis) depends on this measure.
⭐ 2. Total Quantity
Useful for operational insights.
DAX
Total Quantity :=
SUM ( FactSales[Quantity] )
⭐ 3. Total Customers
This uses DISTINCTCOUNT, because customers repeat across invoices.
DAX
Total Customers :=
DISTINCTCOUNT ( FactSales[Customer ID] )
⭐ 4. Total Invoices
Same logic — invoices repeat across rows.
DAX
Total Invoices :=
DISTINCTCOUNT ( FactSales[Invoice] )
⭐ 5. Average Order Value (AOV)
Classic e commerce metric.
DAX
Average Order Value :=
DIVIDE ( [Total Sales], [Total Invoices] )
⭐ 6. YoY Sales (Year over Year)
Now that you have DimDate, you can use time intelligence.
DAX
YoY Sales :=
VAR CurrentYear = [Total Sales]
VAR LastYear = CALCULATE ( [Total Sales], SAMEPERIODLASTYEAR ( DimDate[Date] ) )
RETURN
CurrentYear - LastYear
⭐ 7. YoY %
Percentage growth.
DAX
YoY % :=
DIVIDE ( [YoY Sales], CALCULATE ( [Total Sales], SAMEPERIODLASTYEAR ( DimDate[Date] ) ) )
⭐ 8. MoM Sales (Month over Month)
DAX
MoM Sales :=
VAR CurrentMonth = [Total Sales]
VAR LastMonth = CALCULATE ( [Total Sales], DATEADD ( DimDate[Date], -1, MONTH ) )
RETURN
CurrentMonth - LastMonth
⭐ 9. MoM %
DAX
MoM % :=
DIVIDE ( [MoM Sales], CALCULATE ( [Total Sales], DATEADD ( DimDate[Date], -1, MONTH ) ) )
⭐ 10. Top Product (dynamic ranking)
DAX
Product Rank :=
RANKX (
    ALL ( DimProduct[StockCode] ),
    [Total Sales],
    ,
    DESC
)



