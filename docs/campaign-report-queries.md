# Campaign Report Queries Documentation

> Collection: Campaign report query FOR campaign level views (ID: 104)

This document describes all queries available in the Campaign Report section of the Campaign Analysis Dashboard. These queries help account managers analyze campaign-level advertising performance across different time periods.

---

## Overview

| Report Type | Purpose |
|-------------|---------|
| Weekly Performance | Track campaign trends week over week |
| Monthly Performance | Analyze campaign performance month over month |
| Date Range Analysis | Examine performance for any custom date range |
| Period Comparison | Compare two time periods side by side |
| Targeting Analysis | Compare Manual vs Automatic targeting performance |

---

## Summary: Temporal Configurations and Granularities

| Query ID | Temporal Configuration | Granularity | Use Case |
|----------|------------------------|-------------|----------|
| 697 | Week-over-Week | Campaign Level | Weekly trend analysis |
| 698 | Month-over-Month | Campaign Level | Monthly trend analysis |
| 699 | Custom Date Range | Campaign Level | Specific period analysis |
| 700 | Period A vs Period B | Campaign Level | Side-by-side comparison |
| 701 | Period A vs Period B (% Change) | Campaign Level | Growth/decline analysis |
| 702 | Period A vs Period B (% Change - Visual) | Campaign Level | Quick visual comparison |
| 703 | Custom Date Range | Seller + Targeting Type | Manual vs Auto analysis |

---

## Query Details

### Weekly Performance Queries

#### Question 697: Campaign Weekly Performance Report

| Property | Value |
|----------|-------|
| **Question ID** | 697 |
| **Question Name** | Campaign Weekly Performance Report |
| **Uniqueness Key** | seller_name, campaign_name, status, week_start |
| **Temporal Configuration** | Week-over-Week (Sunday to Saturday) |
| **Granularity** | Campaign Level |
| **Available Filters** | Seller Name, Campaign Name, Status, Start Date, End Date |

**Business Question:** How is each campaign performing week by week? Use this to spot trends, identify campaigns that are improving or declining, and track weekly progress toward goals. Great for weekly performance reviews with clients.

---

### Monthly Performance Queries

#### Question 698: Campaign Monthly Performance Report

| Property | Value |
|----------|-------|
| **Question ID** | 698 |
| **Question Name** | Campaign Monthly Performance Report |
| **Uniqueness Key** | seller_name, campaign_name, status, month_start |
| **Temporal Configuration** | Month-over-Month |
| **Granularity** | Campaign Level |
| **Available Filters** | Seller Name, Campaign Name, Status |

**Business Question:** How is each campaign performing month by month? Use this for monthly business reviews, identifying seasonal patterns, and long-term trend analysis. Ideal for client monthly reports and strategic planning.

---

### Date Range Analysis Queries

#### Question 699: Campaign view for Period A range

| Property | Value |
|----------|-------|
| **Question ID** | 699 |
| **Question Name** | Campaign view for Period A range |
| **Uniqueness Key** | seller_name, campaign_name, status |
| **Temporal Configuration** | Custom Date Range (start_date to end_date) |
| **Granularity** | Campaign Level |
| **Available Filters** | Seller Name, Campaign Name, Status, Start Date (required), End Date (required), Min ROAS, Max ROAS, Min Clicks, Max Clicks, Min Orders, Max Orders |

**Business Question:** What was the total performance of each campaign during a specific time period? Use this to analyze holiday periods, promotional events, or any custom date range. The ROAS/Clicks/Orders filters help you quickly find campaigns that need attention (e.g., "show me campaigns with ROAS below 2").

---

### Period Comparison Queries

#### Question 700: Period A vs Period B campaign data comparison

| Property | Value |
|----------|-------|
| **Question ID** | 700 |
| **Question Name** | Period A vs Period B campaign data comparison |
| **Uniqueness Key** | period, seller_name, campaign_name, status |
| **Temporal Configuration** | Two Custom Periods Comparison |
| **Granularity** | Campaign Level |
| **Available Filters** | Seller Name, Campaign Name, Status, Period A Start (required), Period A End (required), Period B Start (required), Period B End (required) |

**Business Question:** How did campaigns perform in Period A compared to Period B? This shows raw numbers for both periods side by side. Use this to compare this week vs last week, this month vs same month last year, or before vs after a strategy change.

---

#### Question 701: Period A vs Period B: % Change

| Property | Value |
|----------|-------|
| **Question ID** | 701 |
| **Question Name** | Period A vs Period B: % Change |
| **Uniqueness Key** | seller_name, campaign_name, status |
| **Temporal Configuration** | Two Custom Periods Comparison (Percentage) |
| **Granularity** | Campaign Level |
| **Available Filters** | Seller Name, Campaign Name, Status, Period A Start (required), Period A End (required), Period B Start (required), Period B End (required) |

**Business Question:** By what percentage did each campaign's metrics change between Period A and Period B? Use this to quickly identify which campaigns improved or declined the most. Positive percentages mean improvement (for metrics like ROAS, sales, clicks), negative means decline.

---

#### Question 702: Period A vs Period B: % Change (colourful)

| Property | Value |
|----------|-------|
| **Question ID** | 702 |
| **Question Name** | Period A vs Period B: % Change (colourful) |
| **Uniqueness Key** | seller_name, campaign_name, status |
| **Temporal Configuration** | Two Custom Periods Comparison (Percentage with Visual Formatting) |
| **Granularity** | Campaign Level |
| **Available Filters** | Seller Name, Campaign Name, Status, Period A Start (required), Period A End (required), Period B Start (required), Period B End (required) |

**Business Question:** Same as Question 701, but with color-coded formatting for easier visual analysis. Green indicates positive changes (improvements), red indicates negative changes (declines). Use this version when presenting data to clients or for quick visual scanning.

---

### Targeting Analysis Queries

#### Question 703: Auto vs Manual comparison on period A date range

| Property | Value |
|----------|-------|
| **Question ID** | 703 |
| **Question Name** | Auto vs Manual comparison on period A date range |
| **Uniqueness Key** | seller_name, targeting_type |
| **Temporal Configuration** | Custom Date Range |
| **Granularity** | Seller + Targeting Type Level |
| **Available Filters** | Seller Name, Status, Start Date (required), End Date (required) |

**Business Question:** How does Automatic targeting compare to Manual targeting for each seller? Use this to determine the right balance between auto and manual campaigns. Shows aggregated performance by targeting type, helping you decide whether to invest more in manual keyword targeting or automatic targeting.

---

## Filter Descriptions

| Filter Name | Description | Common Use Cases |
|-------------|-------------|------------------|
| **Seller Name** | Filter by brand/seller account | Focus on one client at a time |
| **Campaign Name** | Filter by specific campaign(s) | Analyze specific campaign types |
| **Status** | Filter by campaign status (ENABLED, PAUSED, ARCHIVED) | Default shows ENABLED only |
| **Start Date / End Date** | Define the analysis period | Custom period analysis |
| **Period A / Period B Dates** | Define two periods for comparison | Before/after analysis |
| **Min/Max ROAS** | Filter campaigns by ROAS range | Find underperforming campaigns |
| **Min/Max Clicks** | Filter campaigns by click volume | Find high/low traffic campaigns |
| **Min/Max Orders** | Filter campaigns by order volume | Find converting campaigns |

---

## Metrics Included in All Queries

### Base Metrics
- **Impressions** - How many times ads were shown
- **Clicks** - How many times ads were clicked
- **Orders** - Number of orders attributed to ads
- **Spend** - Total advertising cost
- **Sales** - Total revenue attributed to ads

### Calculated Metrics
- **ROAS** (Return on Ad Spend) - Sales / Spend (higher is better)
- **ACOS** (Advertising Cost of Sales) - (Spend / Sales) x 100% (lower is better)
- **AOV** (Average Order Value) - Sales / Orders
- **CPC** (Cost Per Click) - Spend / Clicks
- **CTR** (Click-Through Rate) - (Clicks / Impressions) x 100%
- **CVR** (Conversion Rate) - (Orders / Clicks) x 100%

---

## Tips for Account Managers

1. **Start with Weekly Reports (697)** for regular monitoring
2. **Use Period Comparison (700, 701)** to justify strategy changes to clients
3. **Filter by ROAS** in Question 699 to quickly find campaigns that need optimization
4. **Compare Auto vs Manual (703)** when deciding budget allocation between targeting types
5. **Use the colourful version (702)** for client presentations - the visual formatting makes trends immediately clear

---

*Last Updated: February 2026*
