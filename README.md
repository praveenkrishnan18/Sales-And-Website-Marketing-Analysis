# Sales & Website Marketing Analytics Dashboard

An interactive two-page Power BI report built with the Maven Fuzzy Factory e-commerce dataset. The report combines sales and product performance with website traffic and marketing conversion analysis.

## Project objectives

- Monitor revenue, gross profit, orders, and average order value.
- Analyze sales trends over time and compare revenue across products.
- Understand website traffic by marketing source and device type.
- Compare sessions, converted sessions, and session conversion rates.
- Prepare source data, create a relational model, and define reusable DAX measures for reporting.

## Dashboard pages

### 1. Executive Sales Overview

<img width="1195" height="668" alt="image" src="https://github.com/user-attachments/assets/ba4c72c7-9491-44a1-9ef6-cf74c6a37222" />

Includes KPI cards for Total Revenue, Gross Profit, Total Orders, and Average Order Value, along with a monthly revenue trend and revenue by product.

### 2. Website & Marketing Analysis

<img width="1197" height="670" alt="image" src="https://github.com/user-attachments/assets/b604f599-1b1a-41a7-974c-6d6c2cb7de1a" />

Includes KPI cards for Total Sessions, Total Pageviews, Converted Sessions, and Session Conversion Rate. Visuals compare sessions by UTM source, conversion performance by source, and performance by device type. Source filters allow interactive exploration.

## Data sources

The project uses six CSV tables from the Maven Fuzzy Factory dataset:

| Source table | Grain / purpose |
|---|---|
| `orders` | One record per order |
| `order_items` | One record per order item |
| `order_item_refunds` | Refund transactions |
| `products` | Product lookup data |
| `website_sessions` | Website session and marketing attributes |
| `website_pageviews` | Individual website pageviews |

The source data was cleaned and typed in Power Query. Refunds were aggregated by `order_item_id` before being joined into the order-item fact query to avoid multiplying sales rows when an item has multiple refund records.

## Data model

The model separates sales, pageview, product, session, and date data. The central sales fact is at **one row per order item**; pageviews remain at one row per pageview. This keeps the different grains separate and reduces double-counting risk.

```text
DimProduct  (1) ───── (*) FactSales (*) ───── (1) DimSession
                                      
DimSession  (1) ───── (*) FactPageviews

DimDate     (1) ───── (*) FactSales
```

`DimDate` is used for date-based analysis. Use active, single-direction one-to-many relationships from dimensions to facts. For date analysis on pageviews, relate `DimDate[Date]` to `FactPageviews[pageview_date]` if that relationship is included in the final PBIX model.

## Main DAX measures

The report uses reusable measures including:

- **Total Revenue** — sum of item-level revenue.
- **Total COGS** — sum of item-level cost of goods sold.
- **Gross Profit** — Total Revenue minus Total COGS.
- **Profit Margin %** — Gross Profit divided by Total Revenue.
- **Total Orders** — distinct count of order IDs.
- **Total Items Sold** — count of order-item rows.
- **Total Refunds** — sum of aggregated refund amounts.
- **Net Revenue** — Total Revenue minus Total Refunds.
- **Average Order Value** — Total Revenue divided by distinct orders.
- **Refund Rate %** — Total Refunds divided by Total Revenue.
- **Total Sessions** — distinct count of website session IDs.
- **Total Pageviews** — distinct count of pageview IDs.
- **Converted Sessions** — distinct count of sessions associated with an order.
- **Session Conversion Rate %** — Converted Sessions divided by Total Sessions.
- **Pages per Session** — Total Pageviews divided by Total Sessions.

## Example observations from the report

The current report snapshot shows approximately:

- **$1.94M** total revenue.
- **$1.22M** gross profit.
- **472.9K** website sessions and a **6.83%** session conversion rate.
- `gsearch` as the largest traffic source by session volume.
- Desktop sessions converting at a higher rate than mobile sessions in the displayed results.
- **The Original Mr. Fuzzy** as the highest-revenue product in the displayed product comparison.

These are descriptive observations from this dataset and report snapshot, not claims about a real business's current performance.

## Tools and techniques

- Microsoft Power BI Desktop
- Power Query (data preparation and data type handling)
- DAX (KPI and analytical measures)
- Dimensional modeling / star-schema concepts
- Relational modeling across sales, products, sessions, and pageviews
- Interactive visuals, slicers, and cross-filtering

## How to explore

1. Open the `.pbix` file in Power BI Desktop.
2. Refresh the data if the source CSV locations have changed; update the Power Query source paths as needed.
3. Review the model relationships before interpreting date-based analysis.
4. Use the slicers on the Website & Marketing Analysis page to compare marketing sources and device types.
5. Select bars or table rows to cross-filter related visuals.

## Repository contents

```text
Sales-And-Website-Marketing-Analysis/
├── README.md
└── screenshots/
    ├── executive-sales-overview.png
    └── website-marketing-analysis.png
```

The Power BI `.pbix` file and original CSV dataset are not included in this package. Add them separately if you intend to publish them, and ensure you have permission to redistribute the source data.
