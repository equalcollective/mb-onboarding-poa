# Amazon Marketing Agency Fundamentals

**Source:** Training Documentation
**Date:** 2026-01-29
**Tags:** `fundamentals`, `structure`, `ASIN`, `SKU`, `brand`, `seller`, `reports`, `brand-analytics`, `SQP`, `inventory`, `fees`, `advertising`, `bidding`, `campaigns`, `targeting`, `match-types`, `buy-box`, `analysis`, `strategy`

---

## Document Purpose

Comprehensive foundational knowledge for Amazon marketing agency operations. Covers platform structure, reports, analytics, advertising mechanics, and strategic frameworks. This is the "how Amazon works" reference document.

---

## 1. Amazon Account Access Structure

### Solution Provider Network (SPN)

**What It Is:** Amazon's official way for agencies to access client accounts (replacing legacy employee-access method).

**Benefits:**
- More secure access management
- Agency-level access with multiple user permissions
- Centralized ticket creation and management
- Future-proof (Amazon phasing out employee access)

**Process:**
1. Register as solution provider on Amazon
2. Get approval for specific permissions
3. Client authorizes agency through SPN portal
4. Agency adds team members

**Current Service:** Advertising Optimization
**Additional Capability:** API access for product development ($1,400 fee for solution providers; free for individual sellers)

### Important Notes
- **Amazon Week Definition:** Sunday to Saturday (US), Monday to Sunday (UK)
- Use Sunday-Saturday consistently across all reports

---

## 2. Amazon Structural Hierarchy

### Understanding the Core Units

```
Brand
  └── Parent ASIN (backend only, not visible on amazon.com)
      └── Child ASIN (individual product page)
          └── SKU (seller's internal inventory identifier)
```

### SKU (Stock Keeping Unit)

**What It Is:**
- Seller-created identifier
- Internal tracking only
- Multiple SKUs can exist for one Child ASIN

**Use Cases:**
- Different sticker versions (Sticker A vs Sticker B)
- Condition differences (new vs used)
- Internal batch tracking

**Key Point:** Completely flexible naming - seller controls

### Child ASIN

**What It Is:**
- Amazon-owned product page
- Contains all customer-facing data

**Contains:**
- Reviews and ratings
- Images
- Product details
- Price

**Key Points:**
- One Child ASIN = One product page
- Amazon controls what changes are allowed
- Changes require Amazon approval with justification

### Parent ASIN

**What It Is:**
- Backend grouping mechanism
- NOT visible to customers on amazon.com
- Groups variant Child ASINs together

**Variant Examples:**
- Colors
- Sizes
- Flavors
- Depths (e.g., 4-inch, 5-inch, 6-inch mattress)

**Key Point:** Amazon defines allowed variation types (limited list)

### Brand

**What It Is:**
- Collection of ASINs under one brand name
- Can contain multiple product types

**Key Point:** One Child ASIN belongs to exactly one brand

### Seller

**Key Points:**
- Can sell multiple brands
- Multiple sellers can sell the same ASIN

**Example:** Nike shoes - 50+ sellers, 1 ASIN, 1 brand

### Seller Types

| Type | Description |
|------|-------------|
| **Brand Owner** | Controls brand registry and brand analytics access |
| **Wholesaler** | Sells other brands' products with inventory |
| **Drop Shipper** | Lists products without holding inventory |
| **FBA** | Fulfilled by Amazon (Amazon handles storage and shipping) |
| **FBM** | Fulfilled by Merchant (seller handles own fulfillment) |

### Key Access Rules
- **Brand Analytics:** Only accessible to brand owners
- Wholesalers cannot see brand analytics for brands they don't own
- One seller can manage multiple brands (separate business units)

---

## 3. Seller Central Reports & Data Sources

### 3.1 Business Reports (Primary Sales Data)

**Location:** Seller Central > Reports > Business Reports

**Date Range Capabilities:**
- Week-over-week (any custom range)
- Month-to-date
- Custom date ranges
- Maximum 2 years historical data

**Key Metrics:**

| Metric | Description |
|--------|-------------|
| Ordered Product Sales | Total revenue |
| Units Ordered | Total units sold |
| Total Order Items | Total order line items |
| Sessions | Unique 24-hour visits to any product page |
| Page Views | Total page views (including repeats) |
| Session Percentage | Conversion rate from sessions |

**Important Definitions:**

**Session:**
- One person visiting your page = 1 session within 24 hours
- Multiple visits from same person within 24 hours = still 1 session
- Same person visiting after 24 hours = new session

**Page View:**
- Each individual page load
- Can be multiple per session

**Advanced Metrics (Often Underutilized):**
- Page Views / Sessions ratio (indicates user behavior)
- Mobile vs Desktop breakdown (device preference)

### 3.2 Sales and Traffic Report

**Use Cases:**
- Historical trending (cross-month comparisons)
- Week-over-week performance
- Can visualize with graphs
- Add metrics to compare multiple KPIs

### 3.3 Detail Page Sales and Traffic

**Two Versions:**

| Version | Use Case |
|---------|----------|
| By Parent Item | All metrics for one parent ASIN, entire date range |
| By Child Item | Most granular level, downloaded by day/week/month |

**Note:** Child item data is what gets uploaded to internal systems

### Data Reconciliation Warning

**Critical Understanding:**
- Different reports use different attribution methods
- Business Report sessions ≠ Search Query Performance sessions
- Reconciling across reports is difficult by design
- Each report serves its specific purpose

---

## 4. Brand Analytics (Brand Owner Only)

### 4.1 Customer Journey Analytics

**Funnel Structure:**
1. Awareness
2. Consideration
3. Intent (add to cart stage)
4. Purchase

### 4.2 Customer Loyalty Analytics

**What It Measures:**
- Repeat customer purchases
- Customer retention metrics

**When Most Relevant:**
- Consumable products
- Multi-packs with repeat potential
- Products with complementary items

**When Less Relevant:**
- Small/growing brands
- One-time purchase products (surfboards, mattress toppers)

### 4.3 Search Query Performance (SQP) - CRITICAL TOOL

**What It Provides:**
- Top search queries for your brand
- Complete funnel from search to purchase
- 24-hour attribution window from search query

**Brand View:**
- Top 1,000 search queries across all products in brand
- Ranked by "Search Query Score" (Amazon's algorithm)
- Score weights: Purchases > Add to Cart > Clicks > Impressions

**ASIN View:**
- Same funnel metrics for individual Child ASINs
- Limited to top 100 search queries per ASIN
- **Limitation:** Child ASIN level (not parent)

**Key Metrics:**
- Search Query Score
- Impressions
- Clicks
- Click-through Rate (CTR)
- Add to Cart
- Purchases
- Conversion Rate

**Critical Understanding - 24-Hour Attribution:**
- Only counts actions within 24 hours of search
- Products with longer consideration windows show lower conversion rates
- Only search-originated traffic counted
- Product page visits without search = NOT counted

**Competitive Intelligence Feature:**
- Click on any search term
- See top 10 products selling for that keyword
- View their metrics: Price, Impression share, Click share, Conversion

**Strategic Use:**
- Pricing decisions
- Competitive benchmarking
- Market positioning

### 4.4 Search Catalog Performance

**Purpose:** Focus on improving catalog performance
**Provides:**
- Traffic metrics
- Conversion opportunities
- Areas for optimization investment

**Key Insight:** Shows where conversion improvements would yield growth if traffic increased

### 4.5 Top Search Terms

**What It Is:**
- Amazon-wide search data (includes Prime Video, all Amazon properties)
- Most searched terms across entire Amazon ecosystem

**Strategic Use:**
1. Competitor research - see which terms drive their sales
2. Discovery - find top 100 search terms with top 3 ASINs each
3. Understand market demand
4. Identify keyword opportunities

**Recommendation:** Monthly or bi-monthly competitive review using this tool

---

## 5. Inventory Management

### Manage All Inventory Page

**Location:** Seller Central > Inventory > Manage All Inventory

**Primary Functions:**
1. SKU Management - view all SKUs, see which ASINs have multiple SKUs
2. Listing Management - edit listings (subject to Amazon approval)
3. Pricing Updates - set and modify prices

**Warning:** Very difficult to delete listings - Amazon heavily restricts deletions

### Revenue Calculator - Critical Fee Tool

**Access:** Manage All Inventory > Actions > Revenue Calculator (for specific ASIN)

### Amazon Fee Structure

**1. Referral Fee**
- Percentage of sale price: 5% to 15% (depends on category and price)
- Calculated after sale completes

**2. Closing Fee**
- Rarely applies (mainly digital services)
- Country-specific

**3. Fulfillment Costs (FBA)**
- Pick, Pack, Ship Fee based on:
  - Product dimensions
  - Weight
  - Category
  - Destination

**Storage Costs:**
- First 90 days: FREE
- After 90 days: Fees begin
- Long-term storage fees escalate significantly

**Removal/Disposal Fees:**
- Return to seller: Fee charged
- Dispose of inventory: Fee charged
- Liquidation: Fee charged

### Complete Seller Cost Structure (FBA)

1. **Inbound Costs:** Shipping to Amazon, prep/labeling, receiving fees
2. **Storage Costs:** Monthly (after 90 days), long-term, seasonal increases
3. **Fulfillment Costs:** Pick and pack, shipping to customer
4. **Referral Fees:** 5-15% of sale (category-dependent)
5. **Returns Processing:** Return shipping, inspection, restocking

---

## 6. Amazon Advertising Fundamentals

### 6.1 Core Advertising Concepts

**Amazon's Dual Revenue Model:**
1. Commission on Sales: ~15% referral fee
2. Advertising Revenue: Pay-per-click (PPC)

**Key Understanding:** Revenue comes from CLICKS, not impressions or purchases. Sellers pay for clicks regardless of conversion.

### Ad Inventory Concept

**Limited by:**
- Search volume for each keyword
- Number of ad placements per search results page
- Customer behavior (clicks, scrolls)

### Three Main Ad Placement Types

| Placement | Description |
|-----------|-------------|
| **Top of Search** | First 5 sponsored product slots - highest visibility, most valuable |
| **Rest of Search** | Scrolled results (positions 6-80) - lower visibility, higher volume |
| **Product Pages** | On competitors' or related product detail pages |

### 6.2 Bidding Fundamentals

**How Bidding Works:**
- You set maximum cost-per-click (CPC)
- You pay: next highest bid + small increment (~$0.01-$0.02)
- Example: You bid $2.00, next highest is $1.50 → You pay $1.52

**Critical Rule:** Amazon won't show irrelevant ads regardless of bid amount. Algorithm protects customer experience.

### 6.3 Amazon's Ad Algorithm - What Determines Ad Showing

**Priority Hierarchy:**

**1. Relevance Matching (First Filter)**
- Product matches user intent
- Keyword in listing
- Category alignment

**2. Performance Factors (Ranking Within Pool)**
- Conversion Rate
- Price Competitiveness
- Reviews & Ratings
- Listing Quality
- Sales Velocity

**3. Bid Amount (Final Determinant)**
- Among similarly performing products, higher bid wins

**Critical Rule:** If conversion rate is poor on a keyword, high bids won't help. Algorithm self-regulates to protect customer experience.

### 6.4 Organic Search Indexing

**Indexing Hierarchy (Highest to Lowest Weight):**

1. **Title** - Most important for keyword indexing
2. **Bullet Points** - Key features and benefits
3. **Description** - Detailed product information
4. **Backend Keywords** - Hidden from customers, 250-byte limit
5. **A+ Content Alt Text** - Alternative text for images

---

## 7. Campaign Structure & Organization

### 7.1 Hierarchical Organization

```
Portfolio (Budget container)
  └── Campaign (Budget + Bidding control)
      └── Ad Group (Products + Targeting)
          └── Products (What to advertise)
          └── Targeting (How to find customers)
```

### 7.2 Portfolio Level

**Controls:**
- Budget cap (maximum spend across all campaigns)
- Aggregated metrics

**Use Case:** Group related campaigns, set overall spending limits

### 7.3 Campaign Level

**Primary Controls:**

**1. On/Off Toggle**

**2. Budget Setting**
- Daily budget per campaign
- Can exceed if Amazon sees opportunity (usually 20-30% over)

**3. Bidding Strategy:**

| Strategy | Behavior | Use Case |
|----------|----------|----------|
| Fixed Bids | No automatic adjustments | Full manual control |
| Dynamic Down Only | Amazon can lower bids when conversion unlikely | Most common, safer |
| Dynamic Up and Down | Amazon can increase or decrease bids | High-growth periods |

**4. Placement Bid Adjustments**
- Control bidding by placement type
- Adjustments applied as percentages

**Example:**
- Base bid: $1.00
- Top of Search adjustment: +50% → $1.50 effective bid
- Product Pages adjustment: -25% → $0.75 effective bid

### 7.4 Ad Group Level

**Two Primary Controls:**
1. Product Selection - which ASINs to advertise
2. Targeting Settings - how Amazon finds customers

### 7.5 Best Practice: 1-1-1 Rule

**Recommended Setup:**
```
1 Portfolio → 1 Campaign → 1 Ad Group → 1 Parent ASIN
```

**Benefits:**
- Independent budget per product
- Separate bidding strategies
- Isolated performance tracking
- Clear cause-effect relationships

**Problem with Multiple Products:**
- Amazon decides budget allocation
- Often indexes heavily on one product
- Other products get minimal spend
- Can't test different products fairly

**Targeting Best Practice:**
- Maximum 5 keywords per campaign
- More keywords = less control over spend

---

## 8. Targeting Types & Match Types

### 8.1 Two Main Targeting Methods

| Method | Description |
|--------|-------------|
| **Keyword Targeting** | Specify search terms to target |
| **Product Targeting** | Target products/categories |

### 8.2 Keyword Match Types

| Match Type | Shows For | Does NOT Show For |
|------------|-----------|-------------------|
| **Exact** | Exact keyword only | Any variation |
| **Phrase** | Keyword as phrase | Phrase broken up |
| **Broad** | Variations and related terms | - |

**Examples for "guitar picks":**
- **Exact:** Only "guitar picks"
- **Phrase:** "red guitar picks", "guitar picks bulk"
- **Broad:** "picks for guitar", "plectrum"

### 8.3 Product Targeting Refinements

**Category Targeting Filters:**
- Price Range
- Star Rating
- Shipping Speed
- Brand (include/exclude)

### 8.4 Negative Targeting

**Purpose:** Exclude irrelevant traffic

**Keyword Example:**
- Selling watercolor blush makeup
- Getting clicks for "NARS watercolor blush"
- Add "NARS" as negative keyword

**Product Example:**
- Force diversity when Amazon favors one product
- Negative target the favored product in Campaign 2
- Forces spend on other products

---

## 9. Campaign Performance Metrics

### 9.1 Core Metrics

| Metric | Formula | Indicates |
|--------|---------|-----------|
| Impressions | - | Visibility level |
| Clicks | - | User interest |
| CTR | Clicks ÷ Impressions | Ad relevance/appeal |
| Spend | Sum of (Clicks × CPC) | Total cost |
| CPC | Spend ÷ Clicks | Average cost per click |
| Orders | - | Purchases from ad clicks |
| Sales | - | Revenue from orders |
| ACoS | Spend ÷ Sales | Advertising efficiency (lower is better) |
| Conversion Rate | Orders ÷ Clicks | Listing quality/product appeal |

### 9.2 Interpreting Metric Combinations

| CTR | Conversion | Interpretation | Action |
|-----|------------|----------------|--------|
| High | Low | Ad attractive, product disappoints | Improve listing, images, reviews |
| Low | High | Product great, ad not attracting | Increase bids for more volume |
| High | High | Winner | Scale this keyword/product |
| Low | Low | Neither working | Pause or heavily optimize |

---

## 10. Buy Box Mechanics

### What Is the Buy Box?

The "Add to Cart" button. When multiple sellers offer same product, only one gets buy box.

### Buy Box Factors (Priority Order)

1. **Price** - Must be competitive (not necessarily lowest)
2. **Fulfillment/Delivery Speed** - FBA strongly favored, Prime badge helps
3. **Seller Rating** - Order Defect Rate, feedback score
4. **Inventory Availability** - Consistent stock levels

### Buy Box Dynamics

- Same ASIN can have multiple sellers
- Only one wins buy box at a time
- Buy box can rotate between qualified sellers
- Losing buy box = can't run sponsored product ads

**Amazon Removes Buy Box When:**
- Price increases dramatically (>X% above recent average)
- Seller rating drops significantly
- Stock issues
- Customer complaints spike

**Critical Rule:** Cannot run sponsored product ads without buy box. "Ineligible" in campaign often means buy box issue.

---

## 11. Analysis Approach & Strategic Thinking

### Starting Points for Account Analysis

1. **Business Reports Overview** - Week/month trends, unusual spikes
2. **Brand Analytics SQP** - Top keywords, opportunities, negative candidates
3. **Inventory Health** - Stock risks, slow-moving, ineligible listings
4. **Campaign Performance** - Profitable vs bleeding, spending issues

### Data Reconciliation Rules

**Critical Understanding:**
- Different reports ≠ Compatible data
- Different attribution windows, inclusion criteria, update frequencies
- Use each report for its intended purpose
- Look for directional trends, not exact matches

### Iterative Testing Philosophy

**Why Manual Optimization Still Matters:**
- If all sellers use same auto-campaign settings, only bid differentiates
- Human pattern recognition accelerates what AI would eventually discover
- Equilibrium breaking through strategic innovation

---

## 12. Common Mistakes & Pitfalls

### Campaign Structure Mistakes

| Mistake | Problem | Solution |
|---------|---------|----------|
| Too many products per campaign | Amazon picks favorites, some never tested | 1-1-1 rule |
| Too many keywords per ad group | Budget dilution, lose granular control | Max 5 keywords |
| Ignoring negative keywords | Waste spend on irrelevant clicks | Regular negative review |

### Bidding Mistakes

| Mistake | Problem | Solution |
|---------|---------|----------|
| "Set and forget" | Market changes, competitors adjust | Regular review |
| Bidding above breakeven | Unsustainable profitability | Calculate ACoS ceiling |
| Uniform bids across placements | Different placements have different conversion | Placement adjustments |

### Listing Issues Causing Ad Ineligibility

- Images violate guidelines
- Missing required attributes
- Pricing errors
- Stock-outs
- Suppressed listings

---

## 13. Key Takeaways for Analysis

### When Analyzing Amazon Accounts

1. Start with Business Reports - understand sales baseline
2. Check Brand Analytics SQP - identify keyword opportunities
3. Review campaign structure - ensure 1-1-1 rule compliance
4. Audit targeting - negative keywords, irrelevant terms
5. Check buy box status - can't advertise without it
6. Calculate true profitability - all costs considered
7. Compare placements - optimize bid adjustments
8. Look for ineligible products - fix blocking issues

### Red Flags to Watch

- ACoS >50% (unless intentional brand awareness)
- Zero impressions on active products (eligibility issue)
- High CTR + low conversion (listing problem)
- Declining buy box win rate (price/performance issue)
- One product dominating multi-product campaign (split needed)
- Irrelevant search terms driving clicks (negative keywords needed)

---

## Recommended Learning Path

| Phase | Focus | Duration |
|-------|-------|----------|
| 1 | Fundamentals - Amazon structure, Seller Central reports, metrics | Weeks 1-2 |
| 2 | Analytics - SQP deep dive, competitive analysis, Revenue Calculator | Weeks 3-4 |
| 3 | Advertising Basics - Campaign structure, targeting, bidding, reports | Weeks 5-6 |
| 4 | Optimization - Negatives, placement bids, budget pacing, A/B testing | Weeks 7-8 |
| 5 | Advanced - Custom reporting, seasonal strategy, launch playbooks | Weeks 9-12 |

---

**Document Version:** 1.0
**Last Updated:** 2026-01-29
**Usage:** Foundational reference for Amazon marketing platform understanding and agency operations
