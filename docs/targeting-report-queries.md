# Targeting Report Queries Documentation

> Reference guide for all queries in the "Targeting report queries FOR campaign analysis" collection (ID: 106)

---

## What is Targeting?

**Targeting** refers to the specific things advertisers bid on to show their ads to potential customers. When you run Amazon ads, you choose what to "target":

- **Keywords** - Search terms customers type (e.g., "guitar pick", "surfboard")
- **ASINs** - Specific competitor or related products (e.g., a particular brand's product page)
- **Categories** - Entire product categories (e.g., "Guitar Picks", "Surfboards")
- **Auto Targets** - Amazon automatically matches your ads to relevant searches/products

The Targeting Report helps account managers understand which targets are performing well (generating sales efficiently) and which are wasting ad spend.

---

## Summary: Query Configurations

| Granularity | Date Range | Week-over-Week | Month-over-Month | Period A vs B |
|-------------|------------|----------------|------------------|---------------|
| **Seller Level** | 709, 710, 711, 712 | - | - | - |
| **Campaign Level** | 713 | 714 | 715 | 716 |

### Temporal Configurations Explained

| Configuration | Description | Use Case |
|---------------|-------------|----------|
| **Date Range** | Single time period (start to end date) | "How did targeting perform last month?" |
| **Week-over-Week (WOW)** | Each week shown as separate row (Sunday-Saturday) | "Which week had the best ROAS?" |
| **Month-over-Month (MOM)** | Each month shown as separate row | "How is performance trending over months?" |
| **Period A vs B** | Compare two custom date ranges side by side | "Compare Black Friday week to the week before" |

---

## Seller-Level Queries

These queries aggregate all data at the seller level, allowing you to see overall targeting performance across all campaigns.

---

### Query 709: Product vs Keyword Targeting (Seller Level)

| Property | Value |
|----------|-------|
| **Question ID** | 709 |
| **Name** | Targeting: Product vs Keyword Targeting (Seller Level) |
| **Uniqueness Key** | seller_name + targeting_strategy |
| **Temporal Config** | Date Range |
| **Granularity** | Seller Level |
| **Available Filters** | start_date (required), end_date (required), seller_name (optional) |

**Business Question:**
*"Should I invest more in keyword targeting or product targeting?"*

This query classifies all targets into two strategies:
- **Keyword Targeting** - Targets based on search terms (BROAD, PHRASE, EXACT keywords, plus auto keyword matching)
- **Product Targeting** - Targets based on specific products or categories (ASIN, Category, Substitutes, Complements)

**What Account Managers Learn:**
- Which targeting strategy generates better ROAS
- Where to shift budget for better efficiency
- The overall split between keyword and product targeting spend

---

### Query 710: Keyword Match Type Breakdown (Seller Level)

| Property | Value |
|----------|-------|
| **Question ID** | 710 |
| **Name** | Targeting: Keyword Match Type Breakdown (Seller Level) |
| **Uniqueness Key** | seller_name + match_type |
| **Temporal Config** | Date Range |
| **Granularity** | Seller Level |
| **Available Filters** | start_date (required), end_date (required), seller_name (optional) |

**Business Question:**
*"Which keyword match type is most efficient? Should I shift budget from BROAD to EXACT?"*

This query breaks down keyword performance by match type:
- **BROAD** - Ads show for loosely related searches
- **PHRASE** - Ads show for searches containing the phrase
- **EXACT** - Ads show only for exact keyword match

**What Account Managers Learn:**
- Which match type has the best conversion rate
- Whether broad targeting is wasting spend
- If there are opportunities to move budget to more efficient match types

---

### Query 711: Product Targeting Depth (Seller Level)

| Property | Value |
|----------|-------|
| **Question ID** | 711 |
| **Name** | Targeting: Product Targeting Depth (Seller Level) |
| **Uniqueness Key** | seller_name + product_targeting_type |
| **Temporal Config** | Date Range |
| **Granularity** | Seller Level |
| **Available Filters** | start_date (required), end_date (required), seller_name (optional) |

**Business Question:**
*"Which specific product targeting types are working? Should I expand ASIN or Category targeting?"*

This query breaks down product targeting by type:
- **ASIN** - Targeting specific competitor products
- **ASIN Expanded** - Amazon's expanded ASIN matching
- **Category** - Targeting entire product categories
- **Category Filtered** - Categories with price/rating filters
- **Substitutes** - Auto-matched substitute products
- **Complements** - Auto-matched complementary products

**What Account Managers Learn:**
- Which product targeting type has highest ROAS
- Whether to invest more in competitor ASIN targeting
- If category targeting is outperforming ASIN targeting

---

### Query 712: All Targetings Compared (Seller Level)

| Property | Value |
|----------|-------|
| **Question ID** | 712 |
| **Name** | Targeting: All Targetings Compared (Seller Level) |
| **Uniqueness Key** | seller_name + targeting + match_type |
| **Temporal Config** | Date Range |
| **Granularity** | Seller Level (individual targets) |
| **Available Filters** | start_date (required), end_date (required), seller_name (optional), min_roas, max_roas, min_clicks, max_clicks, min_orders, max_orders |

**Business Question:**
*"What are my top and bottom performing targets across all targeting types?"*

This query shows every individual target with its performance metrics, ordered by spend. The filters allow you to find specific targets:
- High ROAS targets to expand
- Low ROAS targets to pause
- Targets with many clicks but few orders (poor conversion)

**What Account Managers Learn:**
- Which specific keywords or ASINs are driving the most sales
- Which targets are wasting spend
- Where to increase or decrease bids

---

## Campaign-Level Queries

These queries break down targeting performance within each campaign, allowing you to optimize at a more granular level.

---

### Query 713: All Targetings by Campaign (Period A)

| Property | Value |
|----------|-------|
| **Question ID** | 713 |
| **Name** | Targeting: All Targetings by Campaign (Period A) |
| **Uniqueness Key** | seller_name + campaign_name + targeting + match_type |
| **Temporal Config** | Date Range |
| **Granularity** | Campaign + Targeting Level |
| **Available Filters** | start_date (required), end_date (required), seller_name (optional), campaign_name (optional) |

**Business Question:**
*"How did each target perform within each campaign during this period?"*

**What Account Managers Learn:**
- Performance of each target within its campaign context
- Which targets to adjust within specific campaigns
- Campaign-by-campaign optimization opportunities

---

### Query 714: All Targetings by Campaign (Week-over-Week)

| Property | Value |
|----------|-------|
| **Question ID** | 714 |
| **Name** | Targeting: All Targetings by Campaign (WOW) |
| **Uniqueness Key** | week_start + week_end + seller_name + campaign_name + targeting + match_type |
| **Temporal Config** | Week-over-Week (Sunday to Saturday) |
| **Granularity** | Campaign + Targeting Level |
| **Available Filters** | seller_name (optional), campaign_name (optional) |

**Business Question:**
*"Which targets improved or declined this week vs last week within each campaign?"*

No date input required - shows all available weeks automatically.

**What Account Managers Learn:**
- Weekly trends for each target
- Which targets are improving or declining
- Seasonality patterns in targeting performance

---

### Query 715: All Targetings by Campaign (Month-over-Month)

| Property | Value |
|----------|-------|
| **Question ID** | 715 |
| **Name** | Targeting: All Targetings by Campaign (MOM) |
| **Uniqueness Key** | month + seller_name + campaign_name + targeting + match_type |
| **Temporal Config** | Month-over-Month |
| **Granularity** | Campaign + Targeting Level |
| **Available Filters** | seller_name (optional), campaign_name (optional) |

**Business Question:**
*"Monthly trend: which targets are gaining or losing efficiency within each campaign?"*

No date input required - shows all available months automatically.

**What Account Managers Learn:**
- Monthly performance trends
- Long-term target efficiency changes
- Strategic targeting decisions for upcoming months

---

### Query 716: All Targetings by Campaign (Period A vs B)

| Property | Value |
|----------|-------|
| **Question ID** | 716 |
| **Name** | Targeting: All Targetings by Campaign (Period A vs B) |
| **Uniqueness Key** | period + period_start + period_end + seller_name + campaign_name + targeting + match_type |
| **Temporal Config** | Two custom date ranges |
| **Granularity** | Campaign + Targeting Level |
| **Available Filters** | period_a_start (required), period_a_end (required), period_b_start (required), period_b_end (required), seller_name (optional), campaign_name (optional) |

**Business Question:**
*"How did targeting performance change between two specific time periods?"*

**What Account Managers Learn:**
- Before/after comparisons (e.g., before and after bid changes)
- Event comparisons (e.g., Black Friday vs normal week)
- Year-over-year targeting efficiency

---

## Common Output Metrics

All queries return these standard advertising metrics:

| Metric | Description | What Good Looks Like |
|--------|-------------|---------------------|
| **impressions** | Number of times ads were shown | Higher = more visibility |
| **clicks** | Number of times ads were clicked | Higher = engaging ads |
| **orders** | Number of orders attributed to ads | Higher = effective targeting |
| **spend** | Total ad spend in dollars | Lower is better (with same sales) |
| **sales** | Total sales attributed to ads | Higher = effective ads |
| **roas** | Return on Ad Spend (sales / spend) | Higher = more efficient (2.0+ is good) |
| **acos** | Ad Cost of Sales (spend / sales) % | Lower = more efficient (under 30% typical goal) |
| **aov** | Average Order Value | Higher = bigger purchases |
| **cpc** | Cost Per Click | Lower = cheaper traffic |
| **ctr** | Click-Through Rate % | Higher = more engaging ads (0.5%+ typical) |
| **cvr** | Conversion Rate % | Higher = better targeting (10%+ is good) |
| **cac** | Customer Acquisition Cost | Lower = cheaper to acquire customers |

---

## Collection Information

| Property | Value |
|----------|-------|
| Collection ID | 106 |
| Collection Name | Targeting report queries FOR campaign analysis |
| Parent Collection | Campaign Analysis V1 (ID: 103) |
| Database | Jeff Azure Db Public (ID: 4) |
| Schema | orange_schema |

---

*Last updated: February 2026*
