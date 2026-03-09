# 🛒 Maven Market Retail Analytics — Power BI Dashboard

## Project Overview

This project is an end-to-end retail analytics case study for Maven Market — a multi-national grocery chain operating across the USA, Mexico and Canada. The analysis covers two years of transactional data from 1997 to 1998 across 8 connected data tables. The final output is a 4 page interactive Power BI dashboard covering topline performance, product brand analysis, customer insights and store performance.

---

## Data Source

- **Source:** Maven Market — Internal Retail Dataset
- **Format:** 8 CSV Files
- **Period:** January 1997 to December 1998
- **Countries:** USA, Mexico, Canada

---

## Data Files

| File | Description | Key Columns |
|---|---|---|
| MavenMarket_Customers.csv | Customer demographics and membership details | customer_id, name, gender, income, membership, occupation |
| MavenMarket_Products.csv | Product catalog with pricing and cost details | product_id, brand, name, retail price, cost, weight |
| MavenMarket_Stores.csv | Store details including type, location and size | store_id, store_type, city, country, total_sqft |
| MavenMarket_Transactions_1997.csv | All transactions from 1997 | transaction_date, product_id, customer_id, store_id, quantity |
| MavenMarket_Transactions_1998.csv | All transactions from 1998 | transaction_date, product_id, customer_id, store_id, quantity |
| MavenMarket_Returns_1997-1998.csv | Product returns from 1997 to 1998 | return_date, product_id, store_id, quantity |
| MavenMarket_Calendar.csv | Date dimension table | date |
| MavenMarket_Regions.csv | Sales region and district details | region_id, sales_district, sales_region |

---

## Tools Used

| Tool | Purpose |
|---|---|
| Power BI | Dashboard development and visualization |
| Power Query | Data transformation and custom column creation |
| DAX | Calculated measures and KPIs |
| Data Modeling | Star and snowflake schema design |

---

## Project Workflow

### Part 1 — Connecting and Shaping the Data

**Customers Table**
- Promoted headers and confirmed data types — customer_id as whole number, customer_acct_num and customer_postal_code as text
- Created full_name column by merging first_name and last_name separated by a space
- Created birth_year column by extracting year from birthdate column formatted as text
- Created conditional column has_children — N if total_children equals 0, otherwise Y

**Products Table**
- Promoted headers and confirmed data types — product_id as whole number, product_sku as text, product_retail_price and product_cost as decimal numbers
- Verified 111 distinct product brands and 1,560 distinct product names
- Created discount_price column equal to 90% of original retail price rounded to 2 decimal places
- Replaced null values with zeros in recyclable and low_fat columns

**Stores Table**
- Promoted headers and confirmed data types — store_id and region_id as whole numbers
- Created full_address column by merging store_city, store_state and store_country separated by comma and space
- Created area_code column by extracting characters before the dash in store_phone field

**Regions Table**
- Promoted headers and confirmed region_id as whole number

**Calendar Table**
- Promoted headers and added the following date columns using date tools:
  - Start of Week starting Sunday
  - Name of Day
  - Start of Month
  - Name of Month
  - Quarter of Year
  - Year

**Return Data Table**
- Promoted headers and confirmed all ID columns and quantity as whole numbers

**Transaction Data Table**
- Combined MavenMarket_Transactions_1997 and MavenMarket_Transactions_1998 from a folder
- Removed Source.Name column after combining
- Confirmed all ID columns and quantity as whole numbers
- Data covers 1/1/1997 through 12/30/1998

---

### Part 2 — Creating the Data Model

**Relationships Created:**
- Transaction_Data connected to Customers via customer_id
- Transaction_Data connected to Products via product_id
- Transaction_Data connected to Stores via store_id
- Transaction_Data connected to Calendar via transaction_date as active and stock_date as inactive
- Return_Data connected to Products via product_id
- Return_Data connected to Calendar via return_date
- Return_Data connected to Stores via store_id
- Stores connected to Regions via region_id as snowflake schema

**Model Design:**
- All relationships follow one-to-many cardinality
- Primary keys on lookup table side and foreign keys on data table side
- All filters are one-way with no bidirectional filters
- Filter context flows downstream from lookup tables to data tables
- Data tables connected via shared lookup tables only

**Data Formatting:**
- All date fields formatted to M/d/yyyy format
- product_retail_price, product_cost and discount_price formatted as Currency
- customer_city categorized as City, customer_postal_code as Postal Code, customer_country as Country/Region
- store_city categorized as City, store_state as State or Province, store_country as Country/Region, full_address as Address

---

### Part 3 — DAX Measures

**Transaction Measures:**
```
Total Transactions = COUNTROWS(Transaction_Data)
Last Month Transactions = CALCULATE([Total Transactions], DATEADD('Calendar'[date], -1, MONTH))
```

**Revenue Measures:**
```
Total Revenue = SUMX(Transaction_Data, Transaction_Data[quantity] * RELATED(Products[product_retail_price]))
Last Month Revenue = CALCULATE([Total Revenue], DATEADD('Calendar'[date], -1, MONTH))
Revenue Target = [Last Month Revenue] * 1.05
```

**Cost and Profit Measures:**
```
Total Cost = SUMX(Transaction_Data, Transaction_Data[quantity] * RELATED(Products[product_cost]))
Total Profit = [Total Revenue] - [Total Cost]
Last Month Profit = CALCULATE([Total Profit], DATEADD('Calendar'[date], -1, MONTH))
Profit Margin = DIVIDE([Total Profit], [Total Revenue], 0)
```

**Returns Measures:**
```
Total Returns = COUNTROWS(Return_Data)
Last Month Returns = CALCULATE([Total Returns], DATEADD('Calendar'[date], -1, MONTH))
Return Rate = DIVIDE([Total Returns], [Total Transactions], 0)
```

**Additional Measures:**
```
Total Quantity Sold = SUM(Transaction_Data[quantity])
Weekend Transactions = CALCULATE([Total Transactions], 'Calendar'[Day Name] IN {"Saturday","Sunday"})
```

---

### Part 4 — Dashboard Development

**Page 1 — Topline Performance**
- Maven Market logo added to top left
- Matrix visual showing Top 30 Product Brands by Total Transactions, Total Profit, Profit Margin and Return Rate with conditional formatting — data bars on Total Transactions, White to Green scale on Profit Margin and White to Red scale on Return Rate
- 3 KPI Cards — Current Month Transactions, Current Month Profit and Current Month Returns each compared against last month targets
- Map visual showing Total Transactions by store city with country slicer
- Treemap showing Total Transactions by store country with drill down to state and city level
- Column chart showing Weekly Revenue Trending filtered to 1998
- Gauge chart showing Total Revenue vs Revenue Target for latest month
- Bookmarks added for key insights including Portland 1000 Sales milestone

**Page 2 — Product Brand Detail**
- Selected Product Brand card showing currently filtered brand
- 3 Gauge charts — Total Revenue vs Target, Total Profit vs Target and Total Transactions vs Target
- Weekly Profit Trend area chart from Jan 1997 to Dec 1998
- Weekly Return Trend area chart from Jan 1997 to Dec 1998

**Page 3 — Customer Detail**
- Customer by Membership bar chart — Bronze, Normal, Golden and Silver tiers
- Top 100 Customers table showing Customer ID, Name, Transactions, Profit and Total Revenue
- Top 1 Customer highlight card showing highest transaction customer
- Transactions by Gender pie charts
- Transactions by Occupation pie chart
- Year filter buttons for 1997 and 1998

**Page 4 — Store Performance**
- Store Performance by Type area chart — Supermarket leads at $0.47M followed by Deluxe Supermarket, Gourmet Supermarket, Mid-Size Grocery and Small Grocery
- Total Stores by Country map showing USA, Canada and Mexico locations
- Country filter buttons for Canada, Mexico and USA
- Top Store Type card showing Supermarket as highest performer with 81K quantity sold and $170.4K revenue

---

## Key Insights

1. USA dominates transactions with 181K — significantly ahead of Mexico at 73K and Canada at 16K
2. Current Month Transactions reached 18,325 exceeding the target of 17,339 by 5.69%
3. Current Month Profit of $71.68K exceeded the target of $67.87K by 5.61%
4. Supermarket is the top performing store type with 81K quantity sold and $170.4K revenue
5. Bronze membership tier has the highest customer count at 5,700 followed by Normal at 2,400
6. Top customer Dawn Laner made 301 transactions generating $2,235 in revenue
7. Overall Profit Margin stands at 59.93% across the top 30 product brands
8. Portland store hit 1,000 sales milestone in December confirming strong regional performance

---

## Data Model Structure

```
Regions
   └── Stores
          ├── Transaction_Data ──── Customers
          │         └──────────────── Products
          └── Return_Data
                    ├──────────────── Products
                    └── Calendar (shared with Transaction_Data)
```

---

## Project Structure

```
Maven-Market-Retail-Analytics/
│
├── README.md
├── Data/
│   ├── MavenMarket_Customers.csv
│   ├── MavenMarket_Products.csv
│   ├── MavenMarket_Stores.csv
│   ├── MavenMarket_Regions.csv
│   ├── MavenMarket_Calendar.csv
│   ├── MavenMarket_Returns_1997-1998.csv
│   ├── MavenMarket_Transactions_1997.csv
│   └── MavenMarket_Transactions_1998.csv
├── Dashboard/
│   └── Maven_Market_Report.pbix
└── Screenshots/
    ├── Page1_Topline_Performance.png
    ├── Page2_Product_Brand_Detail.png
    ├── Page3_Customer_Detail.png
    └── Page4_Store_Performance.png
```

---

## How to Use

1. Download the repository
2. Place both transaction CSV files in a folder named MavenMarket Transactions
3. Open Maven_Market_Report.pbix in Power BI Desktop
4. Update the folder path for the Transaction_Data table if needed
5. The dashboard will load with all 4 pages
6. Use the slicers and filter buttons to explore the data interactively

---

## Author

**Akriti Prasad**
Data Analytics | Power BI | Healthcare and Pharma Analytics
Connect on LinkedIn
