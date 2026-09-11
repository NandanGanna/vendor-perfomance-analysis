Vendor Performance Analysis

A data analytics project for evaluating vendor performance, purchasing
efficiency, sales performance, inventory turnover, profitability, and
vendor dependency.

The project combines Python, Pandas, SQLite, SQL, statistical
analysis, Jupyter Notebooks, and Power BI to transform raw vendor,
purchase, invoice, pricing, and sales data into an analysis-ready vendor
performance dataset and business insights.

Project Objectives

The project focuses on answering key business questions:

Which vendors and brands generate the highest sales?

Which vendors contribute the most to procurement spend?

How dependent is the business on its top vendors?

Which brands have low sales but high profit margins and may need
promotion or pricing changes?

Does purchasing in larger quantities reduce unit purchase prices?

Which vendors or products have low inventory turnover?

How much capital is tied up in unsold inventory?

Do top-performing and low-performing vendors have significantly
different profit margins?

Project Structure

Vendor Performance Analysis/
│
├── Vendor Performance Analysis.ipynb
├── Exploratory Data Analysis.ipynb
├── ingestion_db.py
├── get_vendor_summary.py
├── vendor_sales_summary.csv
├── vendor_performance.pbix
├── Vendor Performance Report.pdf
└── data/
    ├── *.csv
    └── ...

Main Files

File                                  Description

Exploratory Data Analysis.ipynb     Explores the source database
tables, relationships, data
quality, and required aggregations.

Vendor Performance Analysis.ipynb   Performs EDA, statistical analysis,
visualization, and business
analysis on the vendor summary
dataset.

ingestion_db.py                     Loads CSV files from the data/
directory into SQLite tables.

get_vendor_summary.py               Creates, cleans, and stores the
aggregated vendor_sales_summary
table.

vendor_sales_summary.csv            Exported analysis-ready vendor
performance dataset.

vendor_performance.pbix             Power BI dashboard/report for
interactive visualization.

Vendor Performance Report.pdf       Final analytical report.

Data Pipeline

The project follows an ETL-style workflow:

Raw CSV Files
     │
     ▼
ingestion_db.py
     │
     ▼
SQLite Database (inventory.db)
     │
     ├── purchases
     ├── purchase_prices
     ├── vendor_invoice
     ├── sales
     ├── begin_inventory
     └── end_inventory
     │
     ▼
get_vendor_summary.py
     │
     ├── Purchase Summary
     ├── Sales Summary
     └── Freight Summary
     │
     ▼
vendor_sales_summary
     │
     ▼
Data Cleaning & Feature Engineering
     │
     ├── GrossProfit
     ├── ProfitMargin
     ├── StockTurnover
     └── SalesToPurchaseRatio
     │
     ├───────────────┐
     ▼               ▼
Python Analysis   Power BI
     │               │
     └───────┬───────┘
             ▼
       Business Insights

Source Data

The analysis uses several related datasets.

purchases

Contains vendor purchase transactions, including:

Vendor number and name

Brand

Product description

Purchase price

Quantity purchased

Purchase dollars

Purchase order information

purchase_prices

Provides product-level pricing information such as:

Brand

Actual price

Volume

Vendor/product pricing information

vendor_invoice

Contains vendor invoice information and freight costs used to calculate
total freight expenditure by vendor.

sales

Contains sales transaction information, including:

Vendor

Brand

Sales quantity

Sales dollars

Sales price

Excise tax

Inventory Tables

begin_inventory and end_inventory contain inventory snapshots. They
were examined during EDA but are not central to the vendor performance
aggregation.

Database Ingestion

ingestion_db.py reads every CSV file in the data/ directory and
creates a SQLite table using the CSV filename as the table name.

Example:

python ingestion_db.py

This creates:

inventory.db

and populates the raw tables.

Vendor Summary Creation

get_vendor_summary.py creates the aggregated vendor performance
dataset.

The SQL transformation builds three intermediate summaries:

Freight Summary --- total freight cost by vendor.

Purchase Summary --- purchase quantities, purchase dollars,
vendor information, product information, and pricing.

Sales Summary --- sales quantities, sales dollars, sales price,
and excise tax by vendor and brand.

These summaries are joined into the final:

vendor_sales_summary

The resulting table is stored in SQLite and can also be exported as:

vendor_sales_summary.csv

Run:

python get_vendor_summary.py

Data Cleaning

The project performs several cleaning operations:

Converts Volume to a numeric data type.

Replaces missing values with 0.

Removes leading/trailing whitespace from VendorName.

Removes leading/trailing whitespace from Description.

Filters inconsistent records for downstream analysis, including
records with non-positive gross profit, non-positive profit margin,
or zero sales quantity.

Feature Engineering

The following metrics are derived from the aggregated data.

Gross Profit

GrossProfit = TotalSalesDollars - TotalPurchaseDollars

Measures the dollar profit generated from sales before other operating
costs.

Profit Margin

ProfitMargin = (GrossProfit / TotalSalesDollars) × 100

Measures profitability relative to sales revenue.

Stock Turnover

StockTurnover = TotalSalesQuantity / TotalPurchaseQuantity

Measures how quickly purchased inventory is converted into sales.

Sales-to-Purchase Ratio

SalesToPurchaseRatio = TotalSalesDollars / TotalPurchaseDollars

Measures sales revenue generated relative to procurement expenditure.

Exploratory Data Analysis

The notebooks examine:

Descriptive statistics

Missing values

Data types

Distributions

Outliers

Vendor and product frequencies

Correlations between numerical variables

Purchase and sales relationships

Notable EDA Observations

Some products have negative gross profit, indicating sales below
purchase cost.

Some products have zero sales despite having purchase quantities,
indicating potentially slow-moving or obsolete inventory.

Freight cost has substantial variation across vendors.

Stock turnover varies widely across products.

Purchase price has weak correlation with total sales dollars and
gross profit in the analyzed dataset.

Purchase quantity and sales quantity show a very strong positive
correlation.

Faster inventory turnover does not necessarily imply higher
profitability.

Business Analysis

Vendor and Brand Performance

The analysis identifies high-performing vendors and brands based on
sales performance and profitability.

It also identifies products with:

Lower sales performance

Higher profit margins

These products may be candidates for:

Promotional campaigns

Pricing adjustments

Improved distribution

Increased visibility

Procurement Concentration

The project evaluates the contribution of top vendors to total
procurement spending.

The analysis indicates that the remaining vendors contribute
approximately 34.31% of procurement, highlighting potential
concentration among the largest suppliers.

High vendor concentration can create supply-chain risk, making supplier
diversification worth evaluating.

Bulk Purchasing

The analysis compares purchase volume with unit pricing.

Large-order purchases achieved an observed unit price of approximately
$10.78, with substantially lower unit costs than smaller orders.

This indicates that bulk purchasing can create meaningful procurement
savings when inventory can be managed efficiently.

Inventory Turnover

Low inventory turnover is used to identify slow-moving products/vendors.

Potential consequences include:

Higher storage costs

Capital tied up in inventory

Insurance and handling costs

Depreciation or obsolescence risk

Capital Locked in Inventory

The project evaluates inventory that has been purchased but not
converted into sales, helping identify vendors contributing
disproportionately to tied-up capital.

Statistical Analysis

The project compares profit margins between top-performing and
low-performing vendors.

A two-sample Welch's t-test is used to test:

H₀: There is no significant difference in mean profit margins
    between top-performing and low-performing vendors.

H₁: The mean profit margins are significantly different.

The analysis found a statistically significant difference between the
two groups.

The reported 95% confidence intervals were approximately:

Vendor Group               95% Confidence Interval

Top-performing vendors            30.74% -- 31.61%
Low-performing vendors            40.48% -- 42.62%

The results indicate that lower-performing vendors can have higher
profit margins despite generating less sales revenue.

Key Business Takeaways

High sales volume does not automatically mean high
profitability.

Some lower-performing vendors maintain substantially higher
margins, suggesting opportunities to improve sales through
pricing, promotion, or distribution.

Bulk procurement can materially reduce unit purchase costs, but
the benefit depends on efficient inventory management.

Slow-moving inventory ties up working capital and increases
carrying costs.

Vendor concentration should be monitored because heavy
dependence on a small group of suppliers can increase procurement
risk.

Inventory turnover and profitability are not directly
interchangeable metrics; high turnover alone does not guarantee
better margins.

Pre-aggregating vendor data improves reporting efficiency by
avoiding repeated joins and aggregations over large transaction
tables.

Technologies Used

Python

Pandas

NumPy

SQLite

SQL

SQLAlchemy

Matplotlib

Seaborn

SciPy

Jupyter Notebook

Power BI

Python Dependencies

Install the core dependencies with:

pip install pandas numpy matplotlib seaborn scipy sqlalchemy jupyter

Running the Project

1. Clone or download the project

Place the project files in a single working directory.

2. Create the data directory

Ensure the raw CSV files are available under:

data/

3. Install dependencies

pip install pandas numpy matplotlib seaborn scipy sqlalchemy jupyter

4. Ingest the raw CSV files

python ingestion_db.py

This creates/populates:

inventory.db

5. Generate the vendor summary

python get_vendor_summary.py

This creates:

vendor_sales_summary

inside inventory.db.

6. Run the notebooks

Start Jupyter:

jupyter notebook

Recommended order:

1. Exploratory Data Analysis.ipynb
2. Vendor Performance Analysis.ipynb

7. Open the Power BI report

Open:

vendor_performance.pbix

in Microsoft Power BI Desktop.

Logging

Both Python ETL scripts use logging:

logs/ingestion_db.log
logs/get_vendor_summary.log

The logs capture ingestion progress, processing information, and
completion status.

Output

The main analytical output is:

vendor_sales_summary

with metrics covering:

Vendor identity

Brand/product information

Purchase price

Actual price

Volume

Purchase quantity

Purchase dollars

Sales quantity

Sales dollars

Sales price

Excise tax

Freight cost

Gross profit

Profit margin

Stock turnover

Sales-to-purchase ratio

Limitations and Considerations

Profit calculations are based on sales dollars minus purchase
dollars and do not represent fully loaded net profit.

Freight is aggregated at vendor level rather than allocated to
individual products.

Some ratios can become undefined or infinite when sales or purchase
dollars/quantities are zero.

Inventory turnover above 1 can occur because sales may include
inventory purchased in earlier periods.

Statistical significance does not by itself establish a causal
relationship between vendor performance and profit margin.

The dataset represents the available transaction period and should
be refreshed for ongoing operational use.

Project Outcome

The project produces a reusable analytical pipeline that converts raw
procurement and sales data into a consolidated vendor-performance
dataset, statistical analysis, and Power BI reporting layer.

It can support decisions around:

Vendor selection

Supplier diversification

Procurement strategy

Bulk purchasing

Product pricing

Promotional targeting

Inventory optimization

Working-capital management

Vendor profitability analysis
