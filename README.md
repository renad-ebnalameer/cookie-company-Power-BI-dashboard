# Cookie Company Performance Dashboard (Power BI)

An interactive 4-page Power BI report that helps a cookie company's CEO understand **why profit fluctuates**, **which products to focus on in 2026**, and **which customers bring in the most income**.

> Final project for the Data Analysis with Power BI and Excel course on Courslia. The dataset is a sample dataset provided by the course.

![Executive Summary](screenshots/02_executive_summary.png)

## Business questions

1. Why does profit fluctuate from month to month?
2. Which products should the company focus on in 2026?
3. Which customers bring in the most income, and where are they located?

## Key findings

- **Profit follows sales, not costs.** Monthly revenue ranges from $212K to $435K, while profit margin stays between 57.6% and 58.2% every month.
- **Chocolate Chip is the engine:** 37% of total profit. The top 3 products deliver 73% of profit.
- **Volume is not profit.** Fortune Cookie sells 154K units, about the same as Oatmeal Raisin (155K), but earns 5.6x less profit ($77K vs $435K) at the lowest margin (50%).
- **Customer concentration:** the top customer (ACME Bites) is 27% of profit and the top 3 customers are 67%.
- **Cross-sell opportunity:** two smaller Seattle accounts have never ordered Sugar or White Chocolate Macadamia Nut.
- **Order size matters:** large orders (2,500+ units) are 17% of orders but 33% of profit.
- **Growth:** Sep-Dec 2020 revenue is 18.8% higher than the same months of 2019.

## Report pages

| Page | What it answers | Main visuals |
|---|---|---|
| **Home** | Cover and entry point | Title, Start button |
| **Executive Summary** | Why does profit fluctuate? | 5 KPI cards, monthly revenue and profit, margin trend, profit by quarter, key insights |
| **Product Performance** | Where should we focus? | Profit by product, margin vs volume (bubble chart), product table, key insights |
| **Customer Insights** | Who are our best customers? | Profit by customer, profit by state, state/city/customer table, key insights |

![Product Performance](screenshots/03_product_performance.png)
![Customer Insights](screenshots/04_customer_insights.png)

## Data model

- **Orders** (700 rows, Sep 2019 - Dec 2020) is the fact table.
- **Customers** (8 customers) connects to Orders by `Customer ID`.
- **Cookie Types** (6 products) connects to Orders by product name.
- **DatesTable** is a custom date table (marked as a date table) connected to Orders by date.

## Data preparation (Power Query)

- Standardised state names to upper case (e.g. `wa` to `WA`).
- Set identifier columns (`Customer ID`, `Order ID`, `Zip`) to Text, and revenue and cost to Fixed decimal number.
- Added columns: `Profit`, `Order Size` (Small / Medium / Large by units), `Location` (City, State) and `Profit per Cookie`.
- Removed columns that are not needed for the analysis.

## DAX measures (13)

| Measure | DAX |
|---|---|
| Total Revenue | `SUM ( Orders[Revenue] )` |
| Total Cost | `SUM ( Orders[Cost] )` |
| Total Profit | `[Total Revenue] - [Total Cost]` |
| Profit Margin % | `DIVIDE ( [Total Profit], [Total Revenue] )` |
| Total Units Sold | `SUM ( Orders[Units Sold] )` |
| Total Orders | `DISTINCTCOUNT ( Orders[Order ID] )` |
| Average Order Value | `DIVIDE ( [Total Revenue], [Total Orders] )` |
| Profit per Unit | `DIVIDE ( [Total Profit], [Total Units Sold] )` |
| Active Customers | `DISTINCTCOUNT ( Orders[Customer ID] )` |
| Revenue LY | `CALCULATE ( [Total Revenue], SAMEPERIODLASTYEAR ( DatesTable[Date] ) )` |
| Comparable Revenue | `SUMX ( VALUES ( DatesTable[Month Year] ), IF ( NOT ISBLANK ( [Revenue LY] ), [Total Revenue] ) )` |
| Revenue YoY % | `DIVIDE ( [Comparable Revenue] - [Revenue LY], [Revenue LY] )` |
| Top Customer Share | `VAR TopProfit = MAXX ( VALUES ( Customers[Customer Name] ), [Total Profit] ) RETURN DIVIDE ( TopProfit, [Total Profit] )` |

`Revenue YoY %` only compares months that exist in both years (Sep-Dec), because 2019 data starts in September.

## Design

- Consistent warm theme (brown, caramel, gold and cream) and layout across all pages.
- Page navigation sidebar, synced Year and Product slicers, and a "Clear all slicers" button.
- Written key insights on every analysis page.

## Notes and limitations

- Course sample data covering 16 months, not real company data.
- Margins are **gross margins** (revenue minus product cost). Salaries, rent and marketing are not included.
- The YoY comparison covers Sep-Dec only.
- "Implication for 2026" lines in the report are recommendations, not facts from the data.

## Tools

Power BI Desktop, Power Query, DAX. Source data in Excel.

## Author

Renad Ebn Alameer - https://www.linkedin.com/in/renad-ebn-alameer

Thanks to my instructor Mohamed Attia and Courslia for the course.
