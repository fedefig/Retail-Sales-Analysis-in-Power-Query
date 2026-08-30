# Retail-Sales-Analysis-in-Power-Query
Consolidation and cleaning of transactional data from two yearly tables (2010 and 2011) to ensure accurate KPI calculations in the dashboard.

Customer Purchasing Behavior Analysis Using Online Retail II (Power Query + Power Pivot)

Dataset Information
Additional Information
This Online Retail II data set contains all the transactions occurring for a UK-based and registered, non-store online retail for 2010 and 2011.The company mainly sells unique all-occasion giftware, many customers of the company are wholesalers.
Source: https://archive.ics.uci.edu/dataset/502/online+retail+ii

1. Dataset Overview (Online Retail II)
The dataset contains:
•	Transactions (InvoiceNo, StockCode, Description, Quantity, UnitPrice)
•	Customers (CustomerID, Country)
•	Dates (InvoiceDate)
•	Returns/Cancellations (Invoices starting with “C”)
Pain points:
•	It’s messy (missing IDs, negative quantities, duplicates)
•	It’s large (500k+ rows)

2. Objectives:
-	Power Query cleaning script
-	Power Pivot data model 
-	DAX measures
-	Excel dashboard with relevant KPIs
  

⚙️ Power Query Data Preparation (M Code)
Consolidation and cleaning of transactional data from two yearly tables (Year 2009 2010 and Year 2010 2011) to ensure accurate KPI calculations in the dashboard.

🧩 Key Steps
•	Combine datasets: Merges both yearly tables into one unified source.
•	Filter integrity, it removes:
    o	Null invoices
    o	Cancellations (invoices starting with “C”)
    o	Negative or zero quantities and prices
    o	Missing customer IDs
•	Text normalization: Trims spaces in text fields (Description, Country, StockCode) for consistent joins and grouping.
•	Sales calculation: Adds a computed column Sales = Quantity × Price.
•	Date enrichment: Extracts the Year from Invoice Date for time based analysis.
•	Currency formatting: Converts the Sales column to currency type for accurate aggregation.
•	Final filter: Excludes 2009 data to focus on the 2010–2011 comparison, as 2009 data is incomplete

💡 Insight

This transformation ensures that only valid, positive, customer linked transactions feed into the Excel dashboard, preventing distortions in KPIs such as total sales, average order value, and customer counts.

📊 Sales Performance Dashboard 
Key Insights
This dashboard provides a clear year over year comparison of sales metrics between 2010 and 2011, highlighting trends in revenue, customer engagement, and product performance.

🔍 Main Observations
•	Stable KPIs: Both years show identical headline metrics — €8.7M in sales, 4,231 customers, and 18,325 invoices — suggesting consistent business volume.
•	Slight Decline: A  4.4% year over year drop in total sales indicates minor contraction despite steady customer counts.
•	Seasonality: Monthly charts reveal strong peaks in Q4, typical of end of year demand cycles.
•	Customer Growth: The customer base fluctuates but shows resilience, with notable spikes in months 10–12.
•	Product Mix Evolution:
    o	   2010 top sellers: classic items like T shirts, mugs, and stationery.
    o	2011 top sellers: more premium or diversified products such as handbags, caps, and leather goods, reflecting a shift toward higher value merchandise.
•	Data Filters: Interactive slicers for year, month, quarter, and semester enable granular exploration of trends.

💡 Interpretation
Overall, the dashboard demonstrates a mature, steady sales structure with evolving product strategy and seasonal performance patterns.


Appendix:
DAX MEASURES

⭐ 1. Total Sales
This is the core measure of your model.
DAX
Total Sales :=
SUM ( FactSales[Sales] )
Why it matters
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



