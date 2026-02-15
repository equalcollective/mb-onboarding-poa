# Placement Report Queries for Campaign Optimisation

> Documentation for queries in the "Placement report query for campaign optimisation" collection (ID: 105)

---

## What is a Placement Report?

The Placement Report shows **where on Amazon your ads appeared** when customers saw them. Amazon offers four placement locations for Sponsored Products ads:

| Placement | Description | What It Means |
|-----------|-------------|---------------|
| **Top of Search** | First row of search results page | Premium visibility, often highest CTR |
| **Rest of Search** | All other positions on search results | Lower visibility, often lower CPC |
| **Product Pages** | Ads on product detail pages | Shown to customers viewing specific products |
| **Off Amazon** | Ads outside Amazon.com (DSP) | Extended reach beyond Amazon |

### Why Placement Analysis Matters

Account managers use placement data to:
- **Optimise bid adjustments** - Increase bids for high-performing placements
- **Budget allocation** - Shift spend to placements with better ROAS
- **Performance diagnosis** - Understand why a campaign might be underperforming

---

## Query Framework Overview

Every placement query is defined by three dimensions:

| Dimension | Description | Options |
|-----------|-------------|---------|
| **Temporal Model** | How time is handled | Date Range, WOW (Week-over-Week), MOM (Month-over-Month), Period A vs B |
| **Aggregation Level** | What level data is rolled up to | Seller Level, Campaign Level, Placement Level |
| **Comparison Level** | What we are comparing across rows | Overall Campaign, By Placement Type |

---

## Summary of Temporal Configurations and Granularities

| Query Type | Temporal Config | Granularity | Primary Use Case |
|------------|-----------------|-------------|------------------|
| Seller-Level Period A | Date Range (single period) | Seller + Placement | Overall placement health check |
| Seller-Level WOW | Week-over-Week | Seller + Placement + Week | Weekly performance trends |
| Seller-Level MOM | Month-over-Month | Seller + Placement + Month | Monthly strategic review |
| Campaign-Level Period A | Date Range (single period) | Campaign + Placement | Campaign placement breakdown |
| Campaign-Level WOW | Week-over-Week | Campaign + Placement + Week | Weekly campaign optimisation |
| Campaign-Level MOM | Month-over-Month | Campaign + Placement + Month | Monthly campaign trends |
| Campaign-Level Period A vs B | Two custom periods | Campaign + Placement + Period | Promotional period comparison |
| Placement-Level Period A | Date Range (single period) | Campaign + Individual Placement | Deep placement analysis |

---

## Seller-Level Queries

These queries aggregate performance across all campaigns for a seller, grouped by placement type.

---

### Query: Placement Performance by Seller (Date Range)

| Field | Value |
|-------|-------|
| **Question ID** | 737 |
| **Question Name** | Placement: Seller Level (Period A Range) |
| **Uniqueness Key** | seller_name + placement |
| **Temporal Config** | Date Range (single period) |
| **Granularity** | Seller + Placement |
| **Available Filters** | start_date (required), end_date (required), seller_name (optional) |

**Business Question:**

*"How does each placement type perform for this seller during the selected period?"*

This helps account managers understand which placements are driving the most value. For example, if Top of Search has high spend but low ROAS, the seller may need to reduce Top of Search bid modifiers.

---

### Query: Placement Performance by Seller (Week-over-Week)

| Field | Value |
|-------|-------|
| **Question ID** | 738 |
| **Question Name** | Placement: Seller Level (Week on Week) |
| **Uniqueness Key** | seller_name + placement + week_start + week_end |
| **Temporal Config** | WOW (MerchantBots weeks: Sunday-Saturday) |
| **Granularity** | Seller + Placement + Week |
| **Available Filters** | seller_name (optional) |

**Business Question:**

*"How did each placement type trend week-over-week for this seller?"*

This enables account managers to spot placement performance changes over time. If Product Pages ROAS dropped significantly this week compared to last week, it may indicate increased competition or seasonal shifts.

---

### Query: Placement Performance by Seller (Month-over-Month)

| Field | Value |
|-------|-------|
| **Question ID** | 739 |
| **Question Name** | Placement: Seller Level (Month on Month) |
| **Uniqueness Key** | seller_name + placement + month |
| **Temporal Config** | MOM (Calendar months: YYYY-MM) |
| **Granularity** | Seller + Placement + Month |
| **Available Filters** | seller_name (optional) |

**Business Question:**

*"What are the monthly placement performance trends for this seller?"*

Monthly views help with strategic planning. Account managers can see long-term trends in placement performance and adjust overall placement strategy accordingly.

---

## Campaign-Level Queries

These queries break down placement performance by individual campaign, enabling granular optimisation.

---

### Query: Placement Performance by Campaign (Date Range)

| Field | Value |
|-------|-------|
| **Question ID** | 740 |
| **Question Name** | Placement: Campaign Level (Period A Range) |
| **Uniqueness Key** | seller_name + campaign_name + placement |
| **Temporal Config** | Date Range (single period) |
| **Granularity** | Campaign + Placement |
| **Available Filters** | start_date (required), end_date (required), seller_name (optional), campaign_name (optional) |

**Business Question:**

*"Within each campaign, how does each placement type perform?"*

This is the core campaign optimisation query. Account managers can see whether a campaign performs better on search results vs. product pages, and adjust bid modifiers accordingly. For example, if a campaign has 5x ROAS on Top of Search but only 1.5x on Product Pages, they should increase the Top of Search bid modifier.

---

### Query: Placement Performance by Campaign (Week-over-Week)

| Field | Value |
|-------|-------|
| **Question ID** | 741 |
| **Question Name** | Placement: Campaign Level (Week on Week) |
| **Uniqueness Key** | seller_name + campaign_name + placement + week_start + week_end |
| **Temporal Config** | WOW (MerchantBots weeks: Sunday-Saturday) |
| **Granularity** | Campaign + Placement + Week |
| **Available Filters** | seller_name (optional), campaign_name (optional) |

**Business Question:**

*"Which placements improved or declined this week for each campaign?"*

Weekly trending helps identify placement performance shifts that need attention. If Top of Search performance suddenly dropped this week, it might indicate a competitor outbid the seller for that placement.

---

### Query: Placement Performance by Campaign (Month-over-Month)

| Field | Value |
|-------|-------|
| **Question ID** | 742 |
| **Question Name** | Placement: Campaign Level (Month on Month) |
| **Uniqueness Key** | seller_name + campaign_name + placement + month |
| **Temporal Config** | MOM (Calendar months) |
| **Granularity** | Campaign + Placement + Month |
| **Available Filters** | seller_name (optional), campaign_name (optional) |

**Business Question:**

*"What are the monthly placement trends for each campaign?"*

Monthly views smooth out weekly fluctuations and reveal consistent patterns. This helps with longer-term campaign strategy decisions.

---

### Query: Placement Performance by Campaign (Period A vs Period B)

| Field | Value |
|-------|-------|
| **Question ID** | 743 |
| **Question Name** | Placement: Campaign Level (Period A vs B) |
| **Uniqueness Key** | seller_name + campaign_name + placement + period + period_start + period_end |
| **Temporal Config** | Two user-defined date ranges |
| **Granularity** | Campaign + Placement + Period |
| **Available Filters** | period_a_start (required), period_a_end (required), period_b_start (required), period_b_end (required), seller_name (optional), campaign_name (optional) |

**Business Question:**

*"How did placement performance change between two custom time periods?"*

This is essential for promotional analysis. Account managers can compare placement performance during Prime Day vs. a normal week, or before and after a bid modifier change. This shows the impact of strategic decisions on placement performance.

---

## Understanding Placement Metrics

All placement queries return these standard metrics:

### Base Metrics
| Metric | What It Shows |
|--------|---------------|
| **Impressions** | Number of times ads were shown in this placement |
| **Clicks** | Number of times customers clicked ads in this placement |
| **Orders** | Number of orders attributed to this placement |
| **Spend** | Amount spent on ads in this placement |
| **Sales** | Revenue from orders attributed to this placement |

### Calculated Metrics
| Metric | Formula | What It Tells You |
|--------|---------|-------------------|
| **ROAS** | Sales / Spend | Return on ad spend - higher is better |
| **ACOS** | (Spend / Sales) x 100 | Ad cost as % of sales - lower is better |
| **CTR** | (Clicks / Impressions) x 100 | Click-through rate - measures ad appeal |
| **CVR** | (Orders / Clicks) x 100 | Conversion rate - measures purchase intent |
| **CPC** | Spend / Clicks | Cost per click - measures click efficiency |
| **AOV** | Sales / Orders | Average order value |
| **CAC** | Spend / Orders | Customer acquisition cost |

---

## How to Use These Queries

### Scenario 1: Initial Placement Analysis
1. Start with **Seller Level (Period A Range)** to get the overall picture
2. Identify which placement has the best and worst performance
3. Drill into **Campaign Level (Period A Range)** to see which campaigns are driving those trends

### Scenario 2: Weekly Optimisation
1. Run **Campaign Level (Week on Week)** to see weekly changes
2. Flag placements with significant performance drops
3. Adjust bid modifiers for underperforming placements

### Scenario 3: Promotional Analysis
1. Use **Campaign Level (Period A vs B)**
2. Set Period A = promotional period (e.g., Black Friday week)
3. Set Period B = baseline period (e.g., previous week)
4. Compare placement performance to see which placements benefited from the promotion

---

## Placement Normalization

Raw data from Amazon may have different naming conventions. These are standardized:

| Raw Value | Normalized Name |
|-----------|-----------------|
| Top of Search on Amazon | Top of Search |
| Rest of Search on Amazon | Rest of Search |
| Rest of search on Amazon | Rest of Search |
| Product pages on Amazon | Product Pages |
| Off Amazon | Off Amazon |

---

## Key Insights for Placement Optimisation

### Top of Search
- Usually highest CTR but also highest CPC
- Best for brand visibility and high-intent searches
- Consider increasing bid modifiers if ROAS justifies it

### Rest of Search
- Lower visibility but often lower CPC
- Good for efficient spend when Top of Search is too expensive
- Default placement - no bid modifier available

### Product Pages
- Shown to customers already browsing products
- Good for conquest (showing ads on competitor pages)
- Performance varies greatly by product category

### Off Amazon
- Extends reach beyond Amazon
- Often lowest performance but increases brand awareness
- Consider only for brands with awareness goals

---

## Database Reference

| Property | Value |
|----------|-------|
| Collection ID | 105 |
| Collection Name | Placement report query for campaign optimisation |
| Database ID | 4 |
| Table | orange_schema.rpt_sponsored_products_placement |
| Parent Collection | Campaign Analysis V1 (103) |

---

## Related Documentation

- [Campaign Analysis Dashboard Reference](/docs/campaign-analysis-dashboard.md) - Query patterns and conventions
- [Metabase General Context](/docs/metabase-general-context.md) - Database tables and temporal models
- [Targeting Report Analysis](/docs/targeting-report-analysis.md) - Similar query patterns for targeting data
