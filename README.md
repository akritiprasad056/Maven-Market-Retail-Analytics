# Maven Market Retail Analytics — Power BI Dashboard

## Project Overview

This project is an end-to-end Business Intelligence case study analyzing retail transaction data for Maven Market — a multinational grocery retail chain operating across the USA, Canada and Mexico. The final output is a 4 page interactive Power BI dashboard covering topline performance, product analytics, customer intelligence and store operations.

---

## Data Source

- **Source:** Maven Analytics
- **Link:** https://www.mavenanalytics.io
- **Records:** Multi-year retail transactions (1997–1998)
- **Format:** CSV

---

## Tools Used

| Tool | Purpose |
|---|---|
| Power BI | Dashboard development and visualization |
| Power Query | Data transformation and custom column creation |
| DAX | Calculated measures and KPIs |
| Excel | Data exploration and preparation |

---

## Project Workflow

### Step 1 — Data Collection
Downloaded retail data from Maven Analytics consisting of multiple CSV files covering customers, products, stores, transactions and returns across USA, Canada and Mexico.

### Step 2 — Data Cleaning
The raw data was cleaned and prepared for modeling:
- Removed duplicate records and null values
- Standardized column data types across all tables
- Merged and appended 1997 and 1998 transaction tables into a single fact table
- Created conditional columns for priority flags and customer age groups

### Step 3 — Power Query Transformations
Performed key transformations in Power Query after loading data into Power BI:

1. **Data Types** — Set appropriate data types for all columns across all tables. Numeric fields set as Whole Number or Decimal. Date fields set as Date type.

2. **Transaction Table Merge** — Appended 1997 and 1998 transaction tables into a unified fact table for complete historical analysis.

3. **Age Group Column** — Created using Conditional Column logic to group customers into meaningful age brackets.

4. **Priority Column** — Created using Conditional Column to flag high value customers based on membership card tier and income level.

5. **Full Name Column** — Combined first name and last name columns into a single display field for customer reporting.

### Step 4 — Data Modeling
Built a Star Schema in Power BI with one central fact table and supporting dimension tables:

- **Fact Table:** Transactions
- **Dimension Tables:** Customers, Products, Stores, Calendar, Regions, Returns
- Established one-to-many relationships between all tables
- Configured the Calendar table as the master date table for time intelligence functions

### Step 5 — DAX Measures
Created a dedicated Measures table in Power BI with the following DAX measures:
```
Total Revenue = SUMX(Transactions, Transactions[Quantity] * RELATED(Products[RetailPrice]))
```
```
Total Cost = SUMX(Transactions, Transactions[Quantity] * RELATED(Products[ProductCost]))
```
```
Total Profit = [Total Revenue] - [Total Cost]
```
```
Profit Margin % = DIVIDE([Total Profit], [Total Revenue], 0)
```
```
Total Transactions = COUNT(Transactions[TransactionDate])
```
```
Total Returns = COUNT(Returns[ReturnDate])
```
```
Return Rate = DIVIDE([Total Returns], [Total Transactions], 0)
```
```
Revenue YTD = TOTALYTD([Total Revenue], Calendar[Date])
```
```
Revenue vs Target = [Total Revenue] - [Monthly Target]
```
```
Weekend Transactions = CALCULATE([Total Transactions], Calendar[IsWeekend] = "Y")
```
```
Total Customers = DISTINCTCOUNT(Transactions[CustomerID])
```
```
Revenue per Sqft = DIVIDE([Total Revenue], RELATED(Stores[GrosssqFt]), 0)
```

### Step 6 — Dashboard Development
Built a 4 page interactive Power BI dashboard with slicers, KPI cards, bar charts, line charts, map visuals and matrix tables.

---

## Dashboard Pages

### Page 1 — Topline Performance
- KPI Cards — Total Revenue, Total Profit, Total Transactions, Return Rate
- Revenue vs Target gauge visual
- Monthly revenue trend line chart
- Revenue breakdown by country
- Slicers — Year, Country

### Page 2 — Product Analytics
- KPI Cards — Total Products, Top Product Revenue, Average Retail Price
- Top 10 products by revenue bar chart
- Product performance matrix with revenue, profit and return rate
- Revenue by product brand bar chart
- Slicers — Product Category, Brand

### Page 3 — Customer Intelligence
- KPI Cards — Total Customers, Average Revenue per Customer, Top Customer Revenue
- Customer segmentation by membership card tier donut chart
- Top 10 customers by revenue bar chart
- Revenue by income level and occupation bar charts
- Slicers — Membership Card, Country, Gender

### Page 4 — Store Operations
- KPI Cards — Total Stores, Top Store Revenue, Average Revenue per Store
- Store performance ranking bar chart
- Revenue per square foot by store
- New vs existing store comparison
- Slicers — Region, Country

---

## Key Insights

1. The USA contributes the highest share of total revenue among the three countries reflecting its larger store count and customer base
2. Hermanos brand leads product revenue — indicating strong brand loyalty among Maven Market customers
3. Return rates remain low across most product categories suggesting high product quality and customer satisfaction
4. Gold and Platinum membership card holders generate a disproportionately high share of total revenue confirming the value of loyalty programs
5. Weekend transactions account for a significant portion of weekly sales — highlighting the importance of weekend staffing and promotions
6. Revenue per square foot varies significantly across stores — presenting opportunities for operational optimization in lower performing locations

---

## Dataset Tables

| Table | Description |
|---|---|
| Transactions | All sales transaction records for 1997 and 1998 |
| Customers | Customer demographics and membership information |
| Products | Product catalog with cost and retail price |
| Stores | Store details including region, country and size |
| Calendar | Date table for time intelligence calculations |
| Returns | Product return records |
| Regions | Geographic hierarchy for regional analysis |

---

## Project Structure
```
Maven-Market-Retail-Analytics/
│
├── README.md
├── Data/
│   └── MavenMarket_Data.csv
├── Dashboard/
│   └── MavenMarket_Report.pbix
└── Screenshots/
    ├── Page1_ToplinePerformance.png
    ├── Page2_Product.png
    ├── Page3_Customer.png
    └── Page4_Store.png
```

---

## How to Use

1. Download the repository
2. Open MavenMarket_Report.pbix in Power BI Desktop
3. The dashboard will load with all 4 pages
4. Use the slicers on each page to filter and explore the data interactively

---

## Author

**Akriti Prasad**
Retail and Business Analytics
Connect on LinkedIn

---

## Acknowledgements

Data sourced from Maven Analytics
https://www.mavenanalytics.io
