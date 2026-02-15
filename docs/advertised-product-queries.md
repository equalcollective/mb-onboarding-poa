# Advertised Product Report Queries

> Documentation for queries in the "Advertised Product report queries FOR campaign analysis" collection (ID: 111)

---

## Report Type Overview

The Advertised Product Report shows **how each product (ASIN/SKU) you are advertising performs** in your Sponsored Products campaigns. This report answers questions like:

- Which of my advertised products are driving the most sales?
- Which products have high spend but low returns?
- How is each product performing week-over-week or month-over-month?

**Key Difference from Other Reports:**
- **Targeting Report** shows performance by *what you targeted* (keywords, ASINs, categories)
- **Advertised Product Report** shows performance by *which of your products* received clicks and sales

---

## Summary Table

### Temporal Configurations Used

| Temporal Model | Description | Use Case |
|----------------|-------------|----------|
| Date Range | Custom start/end date period | "How did this ASIN perform last month?" |
| Week-over-Week (WOW) | Sunday-Saturday weeks | "Compare this week vs last week" |
| Month-over-Month (MOM) | Calendar months (YYYY-MM) | "Monthly trends for each product" |

### Granularity Levels Used

| Granularity | Description | Unique Row Key |
|-------------|-------------|----------------|
| ASIN Level | Performance grouped by Advertised ASIN | seller + campaign + asin |
| SKU Level | Performance grouped by Advertised SKU + ASIN | seller + campaign + sku + asin |

---

## Query Details

### ASIN Level Queries

These queries show performance at the ASIN level. Multiple SKUs that share the same ASIN are combined into one row.

---

#### Query 1: ASIN Level - Period A Range (Date Range)

| Property | Value |
|----------|-------|
| **Question ID** | 726 |
| **Question Name** | Advertised Product: ASIN Level - Period A Range |
| **Uniqueness Key** | seller_name + campaign_name + advertised_asin |
| **Temporal Configuration** | Date Range (user specifies start_date and end_date) |
| **Granularity** | Seller > Campaign > ASIN |

**Available Filters:**

| Filter | Type | Required | Description |
|--------|------|----------|-------------|
| start_date | Date | Yes | Start of the analysis period |
| end_date | Date | Yes | End of the analysis period |
| seller_name | Dropdown | No | Filter by seller |
| campaign_name | Dropdown | No | Filter by campaign |
| min_clicks / max_clicks | Number | No | Filter by click volume |
| min_orders / max_orders | Number | No | Filter by order volume |
| min_roas / max_roas | Number | No | Filter by ROAS range |

**Business Question:**

*"During this time period, which of my advertised ASINs are performing best? Which ASINs are spending money but not converting?"*

This query helps Account Managers:
- Identify top-performing products by sales, ROAS, or conversion rate
- Find underperforming products that may need optimization or pausing
- Compare product performance within the same campaign
- See which products are driving the most ad spend vs. organic value

---

#### Query 2: ASIN Level - Week-over-Week (WOW)

| Property | Value |
|----------|-------|
| **Question ID** | 727 |
| **Question Name** | Advertised Product: ASIN Level - WOW |
| **Uniqueness Key** | week_start + week_end + seller_name + campaign_name + advertised_asin |
| **Temporal Configuration** | Week-over-Week (Sunday to Saturday weeks) |
| **Granularity** | Seller > Campaign > ASIN |

**Available Filters:**

| Filter | Type | Required | Description |
|--------|------|----------|-------------|
| seller_name | Dropdown | No | Filter by seller |
| campaign_name | Dropdown | No | Filter by campaign |
| product_name | Dropdown | No | Filter by product name |
| min_clicks / max_clicks | Number | No | Filter by click volume |
| min_orders / max_orders | Number | No | Filter by order volume |
| min_roas / max_roas | Number | No | Filter by ROAS range |

**Business Question:**

*"How is each ASIN performing this week compared to previous weeks? Are any products trending up or down?"*

This query helps Account Managers:
- Track weekly performance trends for each advertised product
- Identify products gaining or losing momentum
- Spot seasonal patterns or the impact of recent changes
- Monitor week-over-week improvements after optimization

---

#### Query 3: ASIN Level - Month-over-Month (MOM)

| Property | Value |
|----------|-------|
| **Question ID** | 728 |
| **Question Name** | Advertised Product: ASIN Level - MOM |
| **Uniqueness Key** | month + seller_name + campaign_name + advertised_asin |
| **Temporal Configuration** | Month-over-Month (YYYY-MM format) |
| **Granularity** | Seller > Campaign > ASIN |

**Available Filters:**

| Filter | Type | Required | Description |
|--------|------|----------|-------------|
| seller_name | Dropdown | No | Filter by seller |
| campaign_name | Dropdown | No | Filter by campaign |
| min_clicks / max_clicks | Number | No | Filter by click volume |
| min_orders / max_orders | Number | No | Filter by order volume |
| min_roas / max_roas | Number | No | Filter by ROAS range |

**Business Question:**

*"What are the monthly trends for each advertised ASIN? How did product performance change from last month to this month?"*

This query helps Account Managers:
- Analyze long-term product performance trends
- Compare monthly performance across the product catalog
- Identify seasonal products or cyclical patterns
- Support monthly business reviews with product-level data

---

### SKU Level Queries

These queries show performance at the SKU level. Each unique SKU is shown separately, even if multiple SKUs share the same parent ASIN.

---

#### Query 4: SKU Level - Period A Range (Date Range)

| Property | Value |
|----------|-------|
| **Question ID** | 729 |
| **Question Name** | Advertised Product: SKU Level - Period A Range |
| **Uniqueness Key** | seller_name + campaign_name + advertised_sku + advertised_asin |
| **Temporal Configuration** | Date Range (user specifies start_date and end_date) |
| **Granularity** | Seller > Campaign > SKU > ASIN |

**Available Filters:**

| Filter | Type | Required | Description |
|--------|------|----------|-------------|
| start_date | Date | Yes | Start of the analysis period |
| end_date | Date | Yes | End of the analysis period |
| seller_name | Dropdown | No | Filter by seller |
| campaign_name | Dropdown | No | Filter by campaign |
| min_clicks / max_clicks | Number | No | Filter by click volume |
| min_orders / max_orders | Number | No | Filter by order volume |
| min_roas / max_roas | Number | No | Filter by ROAS range |

**Business Question:**

*"For products with multiple variations (sizes, colors), which specific SKUs are performing best? Should I adjust bids for specific variations?"*

This query helps Account Managers:
- Drill down into variation-level performance
- Identify which size/color/style variations are most profitable
- Optimize advertising for specific SKUs within a product family
- Make decisions about inventory allocation based on ad performance

---

## Output Columns (All Queries)

Each query returns the following metrics:

| Column | Description |
|--------|-------------|
| seller_name | The seller's name |
| campaign_name | Campaign containing the advertised product |
| advertised_asin | The ASIN being advertised |
| advertised_sku | The SKU being advertised (SKU-level queries only) |
| product_name | Human-readable product name (when available) |
| impressions | Total times the ad was shown |
| clicks | Total clicks on the ad |
| orders | Total orders attributed to the ad (7-day attribution) |
| spend | Total ad spend in dollars |
| sales | Total sales attributed to the ad (7-day attribution) |
| roas | Return on Ad Spend (sales / spend) |
| acos | Advertising Cost of Sales (spend / sales * 100) |
| aov | Average Order Value (sales / orders) |
| cpc | Cost Per Click (spend / clicks) |
| ctr | Click-Through Rate (clicks / impressions * 100) |
| cvr | Conversion Rate (orders / clicks * 100) |
| cac | Customer Acquisition Cost (spend / orders) |

---

## How to Use These Queries

### For Weekly Performance Reviews

1. Use the **ASIN Level - WOW** query (ID: 727)
2. Filter by your seller name
3. Look at the most recent weeks to identify:
   - Products with declining ROAS (may need bid adjustments)
   - Products with increasing CTR (gaining relevance)
   - High-spend, low-conversion products (candidates for pausing)

### For Monthly Business Reviews

1. Use the **ASIN Level - MOM** query (ID: 728)
2. Compare current month vs previous months
3. Create charts showing performance trends over time
4. Identify seasonal patterns for planning

### For Deep-Dive Analysis

1. Start with **ASIN Level - Period A Range** (ID: 726) for overview
2. If a product shows unusual performance, drill into **SKU Level - Period A Range** (ID: 729)
3. Identify which specific variations are driving the numbers

---

## Technical Notes

- **Week Model:** Weeks run Sunday to Saturday (MerchantBots standard)
- **Attribution Window:** All sales use 7-day attribution
- **Product Names:** Pulled from `mv_asin_details` materialized view when available
- **Data Source:** `orange_schema.rpt_sponsored_products_advertised_product`

---

## Collection Information

| Property | Value |
|----------|-------|
| Collection ID | 111 |
| Collection Name | Advertised Product report queries FOR campaign analysis |
| Parent Collection | Campaign Analysis V1 (ID: 103) |
| Database | Jeff Azure Db Public (ID: 4) |

---

*Last Updated: February 2026*
