# Search Term Report Queries Documentation

> Collection: Search Term report queries FOR campaign analysis (ID: 108)

---

## What is a Search Term Report?

The Search Term Report shows **what customers actually typed** when they saw and clicked on your ads. This is different from targeting (what you set up in your campaign).

**Example:** You might target the keyword "guitar picks", but a customer actually typed "thin guitar picks for beginners". The search term report reveals this real customer intent.

**Key Insight:** Some customers search directly for product codes (ASINs like "B07TXTBP4J") instead of typing descriptive keywords. These are called "ASIN searches" and indicate high purchase intent.

---

## Summary of Available Queries

### Temporal Configurations

| Temporal Model | Description | Queries Using This |
|----------------|-------------|-------------------|
| **Date Range** | User-specified start and end dates | 718, 719 |
| **Week-over-Week** | Data grouped by week (Sunday-Saturday) | 720, 724 |
| **Month-over-Month** | Data grouped by calendar month | 725 |

### Granularity Levels

| Granularity | Description | Queries Using This |
|-------------|-------------|-------------------|
| **Seller Level** | Aggregated across all campaigns for a seller | 719, 720 |
| **Campaign Level** | Data broken down by individual campaign | 718, 724, 725 |
| **With Targeting/Match Type** | Includes what you targeted and match type | 718 |
| **Without Targeting** | Search terms only, no targeting details | 719, 720, 724, 725 |

---

## Query Details

### Group A: Seller-Level Queries (Without Targeting)

These queries show search term performance across all campaigns for a seller. Best for understanding overall customer search behavior.

---

#### Query 719: ST: Seller Level (Date Range)

| Property | Value |
|----------|-------|
| **Question ID** | 719 |
| **Name** | ST: Seller Level (Date Range) |
| **Uniqueness Key** | seller_name, customer_search_term |
| **Temporal Configuration** | Date Range (user-specified start_date and end_date) |
| **Granularity** | Seller Level - Search Term |

**Available Filters:**

| Filter | Type | Required |
|--------|------|----------|
| start_date | Date | Yes |
| end_date | Date | Yes |
| seller_name | Dropdown | No |
| customer_search_term | Text | No |
| min_clicks / max_clicks | Number | No |
| min_orders / max_orders | Number | No |
| min_roas / max_roas | Number | No |

**Business Question:**

*"What are the top search terms driving traffic and sales for this seller during a specific time period?"*

Use this query when you want to:
- See all search terms ranked by spend for a seller
- Identify which customer searches are most valuable
- Find search terms to add as new keywords to campaigns
- Compare search term performance across a custom date range

---

#### Query 720: ST: Seller Level (Week-over-Week)

| Property | Value |
|----------|-------|
| **Question ID** | 720 |
| **Name** | ST: Seller Level (Week-over-Week) |
| **Uniqueness Key** | seller_name, customer_search_term, week_start, week_end |
| **Temporal Configuration** | Week-over-Week (Sunday to Saturday weeks) |
| **Granularity** | Seller Level - Search Term - Weekly |

**Available Filters:**

| Filter | Type | Required |
|--------|------|----------|
| seller_name | Dropdown | No |
| customer_search_term | Text | No |
| min_clicks / max_clicks | Number | No |
| min_orders / max_orders | Number | No |
| min_roas / max_roas | Number | No |

**Business Question:**

*"How did search term performance change week over week for this seller?"*

Use this query when you want to:
- Track weekly trends for search terms
- Identify search terms that are gaining or losing momentum
- Spot seasonal patterns in customer searches
- Compare this week's search behavior to previous weeks

---

### Group B: Campaign-Level Queries (With Targeting)

These queries include targeting and match type information, showing exactly which target triggered each search term.

---

#### Query 718: ST: Search term split on campaign level (W Targeting)

| Property | Value |
|----------|-------|
| **Question ID** | 718 |
| **Name** | ST: Search term split on campaign level (W Targeting) |
| **Uniqueness Key** | seller_name, campaign_name, targeting, match_type, customer_search_term |
| **Temporal Configuration** | Date Range (user-specified start_date and end_date) |
| **Granularity** | Seller - Campaign - Targeting - Match Type - Search Term |

**Available Filters:**

| Filter | Type | Required |
|--------|------|----------|
| start_date | Date | Yes |
| end_date | Date | Yes |
| seller_name | Dropdown | No |
| campaign_name | Contains Text | No |
| customer_search_term | Text | No |
| min_clicks / max_clicks | Number | No |
| min_orders / max_orders | Number | No |
| min_roas / max_roas | Number | No |

**Business Question:**

*"Within each campaign, which search terms are performing well or poorly, and which target/match type triggered them?"*

Use this query when you want to:
- See the full mapping of targets to search terms
- Identify negative keyword candidates (irrelevant searches triggered by broad match)
- Understand which match types are finding the best search terms
- Make precise optimization decisions like adding negative keywords to specific ad groups

---

### Group C: Campaign-Level Queries (Without Targeting)

These queries show search term performance at the campaign level but without targeting/match type details. They are simpler and focus on which campaigns are driving which search terms.

---

#### Query 724: ST: Campaign Level (Week-over-Week)

| Property | Value |
|----------|-------|
| **Question ID** | 724 |
| **Name** | ST: Campaign Level (Week-over-Week) |
| **Uniqueness Key** | seller_name, campaign_name, customer_search_term, search_term_type, week_start, week_end |
| **Temporal Configuration** | Week-over-Week (Sunday to Saturday weeks) |
| **Granularity** | Seller - Campaign - Search Term - Weekly |

**Available Filters:**

| Filter | Type | Required |
|--------|------|----------|
| seller_name | Dropdown | No |
| campaign_name | Dropdown | No |
| customer_search_term | Text | No |
| min_clicks / max_clicks | Number | No |
| min_orders / max_orders | Number | No |
| min_roas / max_roas | Number | No |

**Special Feature:** Includes `search_term_type` column that classifies each search term as either "ASIN Search" or "Keyword Search" automatically.

**Business Question:**

*"How are search terms performing week-over-week within each campaign? Are ASIN searches or keyword searches more effective?"*

Use this query when you want to:
- Track weekly performance trends by campaign
- Identify if a campaign's best search terms are changing over time
- Compare ASIN-based searches vs. keyword-based searches
- Spot campaigns where search term performance is declining

---

#### Query 725: ST: Campaign Level (Month-over-Month)

| Property | Value |
|----------|-------|
| **Question ID** | 725 |
| **Name** | ST: Campaign Level (Month-over-Month) |
| **Uniqueness Key** | month, seller_name, campaign_name, customer_search_term, search_term_type |
| **Temporal Configuration** | Month-over-Month (Calendar months as YYYY-MM) |
| **Granularity** | Seller - Campaign - Search Term - Monthly |

**Available Filters:**

| Filter | Type | Required |
|--------|------|----------|
| seller_name | Dropdown | No |
| campaign_name | Dropdown | No |
| customer_search_term | Text | No |
| min_clicks / max_clicks | Number | No |
| min_orders / max_orders | Number | No |
| min_roas / max_roas | Number | No |

**Special Feature:** Includes `search_term_type` column that classifies each search term as either "ASIN Search" or "Keyword Search" automatically.

**Business Question:**

*"What are the monthly trends for search terms within each campaign? Which search terms are consistently performing well?"*

Use this query when you want to:
- View longer-term search term trends
- Smooth out weekly fluctuations to see overall patterns
- Make strategic decisions about keyword expansion
- Identify seasonality in customer search behavior

---

## Metrics Included in All Queries

All search term queries include these advertising metrics:

| Metric | Description | Good Value |
|--------|-------------|------------|
| **impressions** | How many times ads were shown for this search term | Higher = more visibility |
| **clicks** | How many times customers clicked on ads | Higher = more traffic |
| **orders** | Number of orders attributed to this search term | Higher = better |
| **spend** | Total ad spend on this search term ($) | Lower relative to sales |
| **sales** | Total sales attributed to this search term ($) | Higher = better |
| **roas** | Return on Ad Spend (sales / spend) | Higher = better (2+ is good) |
| **acos** | Advertising Cost of Sales (spend / sales as %) | Lower = better (under 30% is good) |
| **aov** | Average Order Value (sales / orders) | Varies by product |
| **cpc** | Cost Per Click (spend / clicks) | Lower = better |
| **ctr** | Click-Through Rate (clicks / impressions as %) | Higher = better (0.5%+ is good) |
| **cvr** | Conversion Rate (orders / clicks as %) | Higher = better (5%+ is good) |
| **cac** | Customer Acquisition Cost (spend / orders) | Lower = better |

---

## How to Choose the Right Query

| If you want to... | Use Query |
|-------------------|-----------|
| See top search terms for a seller during a custom date range | 719 |
| Track weekly search term trends at seller level | 720 |
| See full detail with targeting and match type info | 718 |
| Track weekly search term trends by campaign | 724 |
| Track monthly search term trends by campaign | 725 |

---

## Quick Reference Table

| ID | Name | Temporal | Granularity | Has Targeting |
|----|------|----------|-------------|---------------|
| 718 | ST: Search term split on campaign level (W Targeting) | Date Range | Campaign + Search Term + Targeting | Yes |
| 719 | ST: Seller Level (Date Range) | Date Range | Seller + Search Term | No |
| 720 | ST: Seller Level (Week-over-Week) | Week-over-Week | Seller + Search Term + Week | No |
| 724 | ST: Campaign Level (Week-over-Week) | Week-over-Week | Campaign + Search Term + Week | No |
| 725 | ST: Campaign Level (Month-over-Month) | Month-over-Month | Campaign + Search Term + Month | No |

---

## Notes

- **Week Definition:** Weeks run from Sunday to Saturday (MerchantBots standard)
- **Search Term Type:** Queries 724 and 725 automatically classify search terms as "ASIN Search" (product code searches) or "Keyword Search" (descriptive text searches)
- **ASIN Searches:** Approximately 18% of search terms are ASINs - these indicate customers searching for specific products by their Amazon product code
