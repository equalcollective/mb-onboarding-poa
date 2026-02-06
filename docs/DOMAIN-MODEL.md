# Domain Model: Amazon Brand Management AI Copilot

> **Purpose:** Formalize the domain for an AI copilot that assists agency account managers (AMs) through the full brand lifecycle: onboarding, weekly management, decision-making, action execution, performance tracking, and client communication.

---

## Part 1: Data Sources (Amazon Reports)

Every metric in this system originates from a specific Amazon report. Understanding the data source map is foundational.

### Report Taxonomy

```
Amazon Data Sources
├── Seller Central
│   └── Business Reports
│       ├── Detail Page Sales & Traffic (by Date)
│       ├── Detail Page Sales & Traffic (by ASIN / Parent / Child)
│       └── Inventory Health Report
│
├── Advertising Console
│   ├── Campaign Report
│   ├── Search Term Report
│   ├── Advertised Product Report
│   ├── Purchased Product Report (Halo)
│   ├── Placement Report
│   ├── Targeting Report
│   └── Budget Report
│
├── Brand Analytics (requires Brand Registry)
│   ├── Search Query Performance (SQP)
│   ├── Search Catalog Performance
│   ├── Top Search Terms
│   ├── Market Basket Analysis
│   ├── Repeat Purchase Behavior
│   └── Demographics
│
├── Account Health Dashboard
│   ├── Order Defect Rate
│   ├── Late Shipment Rate
│   ├── Cancellation Rate
│   └── Policy Compliance
│
└── External / Third-Party
    ├── Helium 10 (keyword research, competitor tracking)
    ├── Jungle Scout (market intelligence)
    ├── Keepa (price/BSR history)
    └── Seller Analytics MCP (our integrated data layer)
```

### Report → Metric Map

| Report | Key Metrics Provided | Granularity | Frequency |
|--------|---------------------|-------------|-----------|
| **Business Report** | Sessions, Page Views, Units, Sales, CVR (Unit Session %), Buy Box % | ASIN / daily | Hourly (by date), Daily (by ASIN) |
| **SQP Report** | Search Query Volume, Impressions, Clicks, Cart Adds, Purchases + YOUR SHARE vs TOTAL for each | Query x Brand / weekly | Weekly, Monthly |
| **Search Catalog Performance** | ASIN-level impressions, clicks, conversion in search | ASIN / weekly | Weekly |
| **Top Search Terms** | Search Frequency Rank, Top 3 clicked ASINs, Click Share, Conversion Share | Query / market-wide | Daily, Weekly, Monthly |
| **Market Basket Analysis** | Co-purchase combinations, Combination % | ASIN pairs | Weekly, Monthly |
| **Repeat Purchase Behavior** | Orders, Unique Customers, Repeat %, Repeat Revenue % | ASIN or Brand | Weekly, Monthly, Quarterly |
| **Search Term Report** | Customer search term, Impressions, Clicks, CTR, CPC, Spend, Sales, ACOS, ROAS, Advertised SKU Sales, Other SKU Sales | Search term x Campaign x Ad Group | Daily |
| **Advertised Product Report** | Per-ASIN: Impressions, Clicks, Spend, Sales, ACOS, ROAS, Advertised SKU Sales/Units, Other SKU Sales/Units | ASIN x Campaign | Daily |
| **Purchased Product Report** | Advertised ASIN → Purchased ASIN mapping, sales and units per purchased product | ASIN pair | Daily |
| **Placement Report** | Top of Search, Rest of Search, Product Pages: Impressions, Clicks, CPC, Spend, Sales, ACOS, ROAS, CVR | Placement type x Campaign | Daily |
| **Demographics** | Age, Income, Education, Gender, Marital Status distributions | Brand-level | Monthly |

### Critical Report Relationships

```
                    ┌──────────────────────┐
                    │   Top Search Terms   │  ← Market-level: what's being searched?
                    │  (market opportunity) │
                    └──────────┬───────────┘
                               │ "For these queries, how do WE perform?"
                               ▼
                    ┌──────────────────────┐
                    │         SQP          │  ← Brand-level: our share of the funnel
                    │  (your share vs all) │
                    │  Organic + Paid      │
                    └──────────┬───────────┘
                               │ "What paid terms drive this?"
                               ▼
                    ┌──────────────────────┐
                    │   Search Term Report │  ← Campaign-level: paid performance only
                    │   (ad attribution)   │
                    └──────────┬───────────┘
                               │ "Which products benefit?"
                               ▼
              ┌────────────────┴─────────────────┐
              ▼                                  ▼
┌──────────────────────┐          ┌──────────────────────┐
│ Advertised Product   │          │ Purchased Product    │
│ (direct ad perf)     │          │ (halo effect detail) │
└──────────┬───────────┘          └──────────────────────┘
           │ "How does this product perform overall?"
           ▼
┌──────────────────────┐
│   Business Report    │  ← Product-level: total traffic + sales (organic + paid blended)
│ (sessions, CVR, BB%) │
└──────────────────────┘
```

### The SQP Report: Why It's Special

The SQP report is the **only report** that shows the full customer funnel with competitive share data:

```
For search query "bbq sauce":

                    Total Market    Your Brand    Your Share
Impressions:          100,000         8,000          8.0%
Clicks:                12,000           960          8.0%
Cart Adds:              3,600           320          8.9%
Purchases:              1,800           180         10.0%

Funnel Conversion:
  Click Rate:           12.0%          12.0%
  Cart Add Rate:        30.0%          33.3%    ← You convert clicks to carts better
  Purchase Rate:        50.0%          56.3%    ← You convert carts to purchases better
```

This reveals WHERE in the funnel you win or lose vs. the market.

---

## Part 2: Metrics Taxonomy

### Complete Metric Classification

Every metric belongs to a **category**, has a **data source**, a **direction** (higher/lower is better), and a **level** (where it's measured).

#### Traffic Metrics

| Metric | Definition | Source | Direction | Level | Computed? |
|--------|-----------|--------|-----------|-------|-----------|
| Sessions | Unique visitors to listing in 24hrs | Business Report | ↑ better | Child ASIN | Raw |
| Page Views | Total listing views (incl. repeat) | Business Report | ↑ better | Child ASIN | Raw |
| Page Views / Session | Avg pages per visit | Business Report | ↑ better | Child ASIN | Computed (PV / Sessions) |
| Impressions (Organic) | Times product appeared in search results | SQP / Search Catalog | ↑ better | Child ASIN x Query | Raw |
| Impressions (Paid) | Times ad was displayed | Ad Reports | ↑ better | Campaign / Ad Group | Raw |
| External Traffic | Visits from outside Amazon | Amazon Attribution | ↑ better | ASIN | Raw |

#### Conversion Metrics

| Metric | Definition | Source | Direction | Level | Computed? |
|--------|-----------|--------|-----------|-------|-----------|
| CTR (Organic) | Clicks / Organic Impressions | SQP | ↑ better | Query x Brand | Computed |
| CTR (Paid) | Clicks / Ad Impressions | Ad Reports | ↑ better | Campaign / Ad Group | Computed |
| CVR / Unit Session % | Units / Sessions | Business Report | ↑ better | Child ASIN | Computed |
| Add to Cart Rate | Cart Adds / Clicks | SQP | ↑ better | Query x Brand | Computed |
| Purchase Rate | Purchases / Cart Adds | SQP | ↑ better | Query x Brand | Computed |
| Buy Box % | % of page views with your offer featured | Business Report | ↑ better | Child ASIN | Raw |

#### Advertising Metrics

| Metric | Definition | Source | Direction | Level | Computed? |
|--------|-----------|--------|-----------|-------|-----------|
| Ad Spend | Total money spent on ads | Ad Reports | Context-dependent | Campaign / Account | Raw |
| Ad Sales | Revenue attributed to ads | Ad Reports | ↑ better | Campaign / Account | Raw |
| ACOS | Ad Spend / Ad Sales x 100 | Ad Reports | ↓ better | Campaign / Ad Group | Computed |
| ROAS | Ad Sales / Ad Spend | Ad Reports | ↑ better | Campaign / Ad Group | Computed (1/ACOS) |
| TACoS | Ad Spend / Total Sales x 100 | Cross-report | ↓ better | Account / Brand | Computed |
| CPC | Average cost per click | Ad Reports | ↓ better | Campaign / Keyword | Computed |
| Clicks (Paid) | Total ad clicks | Ad Reports | ↑ better | Campaign / Ad Group | Raw |
| Advertised SKU Sales | Sales of the exact advertised product | Ad Reports | ↑ better | ASIN x Campaign | Raw |
| Other SKU Sales (Halo) | Sales of other products from ad clicks | Ad Reports | ↑ better | ASIN x Campaign | Raw |
| Halo % | Other SKU Sales / Total Ad Sales x 100 | Ad Reports | Contextual | ASIN x Campaign | Computed |
| New-to-Brand % | Sales from first-time brand customers | Ad Reports (SB/SD) | ↑ better | Campaign | Raw |

#### Revenue Metrics

| Metric | Definition | Source | Direction | Level | Computed? |
|--------|-----------|--------|-----------|-------|-----------|
| Total Sales | All revenue (organic + paid) | Business Report | ↑ better | ASIN / Account | Raw |
| Total Units | All units sold | Business Report | ↑ better | ASIN / Account | Raw |
| Avg Selling Price | Total Sales / Total Units | Business Report | Context-dependent | ASIN | Computed |
| Organic Sales | Total Sales - Ad Sales | Cross-report | ↑ better | ASIN / Account | Computed |
| Organic Share % | Organic Sales / Total Sales x 100 | Cross-report | ↑ better (long-term) | Account | Computed |
| B2B Sales | Revenue from business customers | Business Report | ↑ better | ASIN | Raw |

#### Ranking & Visibility Metrics

| Metric | Definition | Source | Direction | Level | Computed? |
|--------|-----------|--------|-----------|-------|-----------|
| BSR | Best Seller Rank in category | Product page / Keepa | ↓ better (lower = higher rank) | ASIN x Category | Raw |
| Organic Rank | Position in search results for keyword | Helium 10 / Manual | ↓ better | ASIN x Keyword | Raw |
| Impression Share | Your impressions / Total impressions for query | SQP | ↑ better | Brand x Query | Computed |
| Click Share | Your clicks / Total clicks for query | SQP / Top Search Terms | ↑ better | Brand x Query | Computed |
| Conversion Share | Your purchases / Total purchases for query | SQP | ↑ better | Brand x Query | Computed |
| Share of Voice | % of first-page positions you hold | Third-party | ↑ better | Brand x Category | Computed |

#### Customer Metrics

| Metric | Definition | Source | Direction | Level | Computed? |
|--------|-----------|--------|-----------|-------|-----------|
| Star Rating | Average product rating | Product page | ↑ better | ASIN | Raw |
| Review Count | Total reviews | Product page | ↑ better | ASIN | Raw |
| Review Velocity | New reviews per period | Tracked over time | ↑ better | ASIN | Computed |
| Repeat Purchase % | % customers who buy again | Repeat Purchase Report | ↑ better | ASIN / Brand | Raw |
| Repeat Revenue % | % revenue from repeat buyers | Repeat Purchase Report | ↑ better | ASIN / Brand | Raw |
| Return Rate | % of orders returned | Account Health | ↓ better | ASIN | Raw |

#### Account Health Metrics

| Metric | Definition | Source | Direction | Level | Computed? |
|--------|-----------|--------|-----------|-------|-----------|
| Order Defect Rate | Defective orders / total orders | Account Health | ↓ better (must be <1%) | Account | Computed |
| Late Shipment Rate | Late shipments / total shipments | Account Health | ↓ better (must be <4%) | Account | Computed |
| Cancellation Rate | Cancelled orders / total orders | Account Health | ↓ better (must be <2.5%) | Account | Computed |
| IPI Score | Inventory Performance Index | Inventory Dashboard | ↑ better (target >400) | Account | Computed |
| Days of Cover | Current stock / daily sales rate | Inventory Report | Context-dependent | ASIN | Computed |

---

## Part 3: The Causal Model

### 3.1 The Complete Action Taxonomy

Every action an AM takes falls into one of these categories:

```
AM Actions
├── Advertising
│   ├── Bid Management
│   │   ├── increase_bid
│   │   ├── decrease_bid
│   │   └── set_bid_rule (automated)
│   ├── Budget Management
│   │   ├── increase_budget
│   │   ├── decrease_budget
│   │   └── set_budget_cap (portfolio)
│   ├── Keyword Management
│   │   ├── add_keyword (with match type)
│   │   ├── negate_keyword
│   │   ├── pause_keyword
│   │   └── change_match_type
│   ├── Campaign Management
│   │   ├── create_campaign
│   │   ├── pause_campaign
│   │   ├── archive_campaign
│   │   └── change_campaign_setting
│   ├── Targeting
│   │   ├── add_product_target
│   │   ├── add_category_target
│   │   ├── negate_product_target
│   │   └── change_targeting_type
│   └── Placement
│       ├── adjust_top_of_search_modifier
│       ├── adjust_product_page_modifier
│       └── adjust_rest_of_search_modifier
│
├── Listing Optimization
│   ├── Title
│   │   ├── update_title
│   │   └── add_keywords_to_title
│   ├── Images
│   │   ├── update_main_image
│   │   ├── add_lifestyle_image
│   │   ├── add_infographic
│   │   └── add_video
│   ├── Content
│   │   ├── update_bullet_points
│   │   ├── update_description
│   │   ├── create_a_plus_content
│   │   └── update_a_plus_content
│   ├── Backend
│   │   ├── update_backend_keywords
│   │   └── update_search_terms
│   └── Storefront
│       ├── create_brand_store
│       └── update_brand_store
│
├── Pricing
│   ├── change_price
│   ├── create_coupon
│   ├── create_promotion
│   ├── submit_lightning_deal
│   ├── enroll_subscribe_and_save
│   └── change_map_price
│
├── Inventory
│   ├── restock_fba
│   ├── create_removal_order
│   ├── switch_fulfillment (FBA/FBM)
│   ├── create_multi_channel_fulfillment
│   └── update_restock_settings
│
├── Reviews & Reputation
│   ├── enroll_vine
│   ├── respond_to_review
│   ├── request_review
│   └── respond_to_question
│
├── Account Operations
│   ├── open_case_with_amazon
│   ├── submit_brand_registry_ticket
│   ├── appeal_suppression
│   └── update_account_settings
│
└── External
    ├── drive_external_traffic
    ├── social_media_campaign
    └── influencer_partnership
```

### 3.2 Action → Metric Impact Matrix

This is the core of the causal model. Each action has **primary**, **secondary**, and **lagging** effects.

#### Advertising Actions

| Action | Primary Impact | Secondary Impact | Lagging Impact | Time to Measure |
|--------|---------------|-----------------|----------------|-----------------|
| **increase_bid** | ↑ Impressions, ↑ Clicks | ↑ CPC, ↑ Ad Spend, ↑ Sponsored Rank | ↑ Sales velocity → ↑ Organic Rank (flywheel) | 3-7 days primary, 2-4 weeks organic |
| **decrease_bid** | ↓ Impressions, ↓ Clicks | ↓ Ad Spend, ↓ CPC | ↓ Sales velocity → risk ↓ Organic Rank | 3-7 days primary |
| **add_keyword (exact)** | ↑ Targeted Impressions | ↑ Relevant Clicks, potentially ↓ ACOS | ↑ Organic Rank for that term | 7-14 days primary, 30-60 days organic |
| **add_keyword (broad)** | ↑↑ Impressions (wide) | ↑ Clicks (mixed relevance), risk ↑ ACOS | Discovery of new converting terms | 7-14 days |
| **negate_keyword** | ↓ Irrelevant Impressions | ↓ Wasted Spend, ↓ ACOS | Improved campaign efficiency | 3-7 days |
| **create_campaign (SP Auto)** | ↑ Discovery Impressions | Data collection on search terms | Keyword harvesting opportunities | 14-21 days |
| **create_campaign (SP Manual)** | ↑ Targeted Impressions | ↑ Controlled Clicks | ↑ Rank for targeted terms | 7-14 days |
| **pause_campaign** | ↓↓ Impressions, ↓ Ad Sales | ↓ Total Sales (if organic can't compensate) | Risk ↓ BSR, ↓ Organic Rank | Immediate, 7-14 days for organic impact |
| **adjust_top_of_search_modifier** | ↑ Top-of-search visibility | ↑ CTR (better placement), ↑ CPC | ↑ Brand awareness, ↑ Organic Rank | 7-14 days |
| **add_product_target** | ↑ Impressions on competitor pages | ↑ Conquest clicks | Customer acquisition from competitors | 7-14 days |

#### Listing Actions

| Action | Primary Impact | Secondary Impact | Lagging Impact | Time to Measure |
|--------|---------------|-----------------|----------------|-----------------|
| **update_main_image** | ↑ CTR (organic + paid) | ↑ Sessions, ↑ Clicks | ↑ Sales → ↑ BSR → ↑ Organic Rank | 7-14 days CTR, 30 days rank |
| **update_title** | ↑ Organic Impressions (indexing), ↑ CTR | ↑ Sessions | ↑ Organic Rank for new keywords | 7-14 days indexing, 30 days rank |
| **update_bullet_points** | ↑ CVR | ↑ Units/Session | ↓ ACOS (same spend, more sales) | 14-30 days |
| **create_a_plus_content** | ↑ CVR (+3-10% typical), ↑ PV/Session | ↑ Sales, ↓ Return Rate | ↑ BSR, ↓ ACOS | 14-30 days |
| **add_video** | ↑ CVR, ↑ PV/Session | ↑ Time on page, ↑ Sales | ↓ Return Rate (better understanding) | 14-30 days |
| **update_backend_keywords** | ↑ Organic Impressions (more indexed terms) | ↑ Sessions from new terms | ↑ Organic Rank for new terms | 7-14 days indexing |

#### Pricing Actions

| Action | Primary Impact | Secondary Impact | Lagging Impact | Time to Measure |
|--------|---------------|-----------------|----------------|-----------------|
| **decrease_price** | ↑ CVR, ↑ Buy Box % | ↑ Sales velocity, ↓ Margin | ↑ BSR, ↑ Organic Rank, ↓ ACOS | Immediate CVR, 7 days BSR |
| **increase_price** | ↓ CVR, risk ↓ Buy Box % | ↑ Margin (if volume holds) | Risk ↓ BSR, ↓ Organic Rank | Immediate CVR, 7 days BSR |
| **create_coupon** | ↑ CTR (badge), ↑ CVR | ↑ Sales velocity, ↓ Effective Price | ↑ BSR, social proof | Immediate on activation |
| **submit_lightning_deal** | ↑↑ Sales velocity (spike) | ↑↑ Sessions, ↑ BSR | Organic Rank boost (if sustained) | During deal + 7-14 days after |
| **enroll_subscribe_and_save** | ↑ Repeat Purchase % | ↑ Customer LTV, ↑ Revenue stability | Organic sales floor | 30-90 days to see patterns |

#### Inventory Actions

| Action | Primary Impact | Secondary Impact | Lagging Impact | Time to Measure |
|--------|---------------|-----------------|----------------|-----------------|
| **restock_fba** | ↑ Buy Box % (if was low/OOS) | ↑ Sessions (suppression lifted) | Gradual BSR recovery | Immediate BB%, 7-30 days BSR |
| **stockout (inaction)** | ↓↓ Buy Box %, listing suppressed | ↓↓ Sales, ↓ Sessions to zero | ↓↓ BSR, ↓↓ Organic Rank (hard to recover) | Immediate, weeks to recover |
| **switch_to_FBA** | ↑ Buy Box %, ↑ Prime badge | ↑ CVR (Prime trust), ↑ CTR | ↑ Organic Rank | 7-14 days |

#### Review Actions

| Action | Primary Impact | Secondary Impact | Lagging Impact | Time to Measure |
|--------|---------------|-----------------|----------------|-----------------|
| **enroll_vine** | ↑ Review Count (+30 potential) | ↑ Star Rating (if product is good) | ↑ CVR (social proof), ↑ CTR | 30-90 days for reviews |
| **respond_to_negative_review** | Neutral (direct) | ↑ Customer perception | ↑ Brand trust | Ongoing |
| **request_review** | ↑ Review Velocity | ↑ Review Count | ↑ CVR (social proof) | 7-30 days |

### 3.3 The Flywheel Model

The Amazon flywheel is the most important concept in the causal model. It describes how paid actions create organic momentum.

```
                    ┌──────────────────┐
                    │   Ad Spend       │
                    │   (investment)   │
                    └────────┬─────────┘
                             │ generates
                             ▼
                    ┌──────────────────┐
                    │   Ad Sales       │
                    │   (paid revenue) │
                    └────────┬─────────┘
                             │ increases
                             ▼
                    ┌──────────────────┐
                    │  Sales Velocity  │◄──────────────────────┐
                    │  (units/time)    │                       │
                    └────────┬─────────┘                       │
                             │ improves                        │
                             ▼                                 │
                    ┌──────────────────┐                       │
                    │  BSR / Organic   │                       │
                    │  Rank            │                       │
                    └────────┬─────────┘                       │
                             │ increases                       │
                             ▼                                 │
                    ┌──────────────────┐                       │
                    │  Organic         │                       │
                    │  Impressions     │                       │
                    └────────┬─────────┘                       │
                             │ drives                          │
                             ▼                                 │
                    ┌──────────────────┐                       │
                    │  Organic Sales   │───────────────────────┘
                    │  (free revenue)  │
                    └────────┬─────────┘
                             │ reduces need for
                             ▼
                    ┌──────────────────┐
                    │  TACoS ↓         │
                    │  (efficiency)    │
                    └──────────────────┘

FLYWHEEL HEALTH INDICATOR: TACoS trending ↓ while Total Sales trending ↑
FLYWHEEL BROKEN INDICATOR: TACoS trending ↑ or flat while Total Sales flat/↓
```

### 3.4 The Diagnostic Model

When a metric goes wrong, this is the decision tree for root-cause analysis.

#### ACOS Increased

```
ACOS ↑
├── CPC increased?
│   ├── YES → Competition for keywords increased
│   │         → Check: new competitors? seasonal demand?
│   │         → Action: audit keywords, reduce bids on low-converting
│   └── NO  → Conversion issue (same cost, fewer sales per click)
│
├── CVR decreased?
│   ├── YES → Listing or offer problem
│   │   ├── Buy Box lost? → Check price, stock, fulfillment
│   │   ├── Price increased? → Evaluate margin vs. volume tradeoff
│   │   ├── Negative reviews? → Check recent review velocity
│   │   ├── Listing changed? → Revert or optimize
│   │   └── Seasonal? → Compare YoY data
│   └── NO  → Traffic quality problem
│
├── Irrelevant search terms?
│   ├── YES → Check Search Term Report
│   │         → Action: add negative keywords
│   └── NO  → Check campaign structure
│
└── New keywords/campaigns added?
    ├── YES → Expected ramp period (7-14 days)
    │         → Monitor, don't react yet
    └── NO  → Deeper audit needed
```

#### Sales Dropped

```
Sales ↓
├── Sessions dropped?
│   ├── YES → Traffic problem
│   │   ├── Ad Impressions down? → Budget exhausted? Bids too low?
│   │   ├── Organic Impressions down? → Check SQP impression share
│   │   │   ├── Share dropped? → Competitor gained rank
│   │   │   └── Total market down? → Seasonal / category trend
│   │   └── External traffic stopped? → Check campaigns outside Amazon
│   └── NO → Conversion problem (same traffic, fewer sales)
│
├── CVR dropped?
│   ├── YES → Offer or listing issue
│   │   ├── Buy Box %? (check Business Report)
│   │   ├── Price competitive? (check vs. competitors)
│   │   ├── Reviews? (star rating or count dropped?)
│   │   ├── Listing suppressed or changed?
│   │   └── Stock low? (low-stock badge)
│   └── NO → Something else
│
├── Buy Box lost?
│   ├── YES → Price, fulfillment, or competitor won it
│   │         → Action: check price, verify FBA, check for new sellers
│   └── NO  → Not a Buy Box issue
│
└── Stockout?
    ├── YES → Lost all sales + organic rank damage
    │         → Action: restock urgently, plan recovery campaign
    └── NO  → Continue investigation
```

#### TACoS Increasing

```
TACoS ↑
├── Ad Spend growing faster than Total Sales?
│   ├── YES → Efficiency problem
│   │   ├── ACOS also increasing? → Campaign optimization needed
│   │   └── ACOS stable? → Spending more but not converting proportionally
│   └── NO → Organic sales declining
│
├── Organic Sales declining?
│   ├── YES → Flywheel broken
│   │   ├── Organic Rank dropping? → Check SQP impression share
│   │   ├── Seasonal? → Compare YoY
│   │   ├── Competition increased? → New entrants in category
│   │   └── Listing quality degraded? → Audit listing
│   └── NO  → Unusual pattern, deeper analysis needed
│
└── Total Sales flat while Ad Spend grows?
    ├── YES → Diminishing returns on ad spend
    │         → Action: optimize instead of spending more
    └── NO  → Review data period and attribution
```

### 3.5 Metric Interaction Effects

Some actions have **compound effects** that multiply through the system:

| Compound Effect | Mechanism | Example |
|----------------|-----------|---------|
| **Listing + Ads Synergy** | Better listing → higher CVR → lower ACOS → more efficient ads → more spend available | Updating A+ content while running ads: ACOS drops 15-20% |
| **Price + Velocity Loop** | Lower price → higher CVR → more sales → better BSR → more organic traffic → more sales | Coupon launch: sales spike, BSR improves, coupon ends but organic sustains |
| **Review + CVR Cascade** | More reviews → higher CVR → more sales → more organic reviews → even higher CVR | Vine enrollment: reviews arrive → CVR improves → organic sales increase |
| **Stockout Recovery Penalty** | Stockout → lost rank → need aggressive ads to recover → high TACoS period | 2 weeks OOS: takes 4-8 weeks and 2x ad spend to recover previous organic position |
| **Image + CTR + Rank** | Better main image → higher CTR → more clicks → more sales → higher rank → more impressions | Main image update: CTR improves 20% → sales up 25% → BSR improves 30% |

### 3.6 Time Horizons for Measurement

Not all metrics respond at the same speed. This is critical for setting expectations.

| Timeframe | What to Measure | Why |
|-----------|----------------|-----|
| **Immediate (0-3 days)** | Impressions, Clicks, Spend, CPC | These respond instantly to bid/budget changes |
| **Short-term (3-7 days)** | CTR, CVR, ACOS, ROAS, Sessions | Enough data for statistical significance on conversion metrics |
| **Medium-term (7-30 days)** | Sales trends, BSR movement, TACoS | Sales velocity needs time to establish patterns |
| **Long-term (30-90 days)** | Organic Rank changes, Review accumulation, Flywheel effects | Amazon's algorithm needs sustained signals |
| **Strategic (90+ days)** | Market share (SQP), Brand awareness (NTB%), Customer LTV | Competitive positioning shifts slowly |

### 3.7 SQP-Driven Funnel Diagnostics

The SQP report enables a unique diagnostic angle: **where in the funnel are you losing vs. the market?**

```
For a given search query, compare YOUR funnel rates vs. MARKET funnel rates:

Step 1: Impression Share
  Your share LOW → You're not showing up for this term
    → Check: organic rank, ad coverage, backend keywords
    → Action: add keyword targeting, optimize listing for this term

Step 2: Click Share vs Impression Share
  Click share < Impression share → You show up but don't get clicked
    → Diagnosis: Poor CTR relative to market
    → Check: main image, title, price, star rating, review count
    → Action: improve main image, competitive pricing, get more reviews

Step 3: Cart Add Share vs Click Share
  Cart share < Click share → People click but don't add to cart
    → Diagnosis: Listing doesn't convince at detail page level
    → Check: A+ content, bullet points, images, price comparison
    → Action: improve listing content, review price positioning

Step 4: Purchase Share vs Cart Add Share
  Purchase share < Cart share → People add to cart but don't buy
    → Diagnosis: Last-mile conversion problem
    → Check: Buy Box %, shipping speed, stock availability
    → Action: fix Buy Box issues, improve delivery, check stock

Step 5: Purchase Share > Impression Share
  → You're converting ABOVE your fair share of impressions
  → This means your listing is strong, you need MORE traffic
  → Action: increase ad spend, expand keyword coverage, drive external traffic
```

### 3.8 Cross-Report Attribution Model

Understanding which report tells you WHAT about a given metric:

```
Question: "Why did ACOS increase for Product X?"

Data Sources to Check (in order):

1. Search Term Report
   → Are new search terms triggering ads that don't convert?
   → Are CPCs increasing for existing terms?

2. Placement Report
   → Is one placement (e.g., Product Pages) dragging down efficiency?
   → Did Top of Search modifier change?

3. Business Report
   → Did overall CVR drop? (affecting ad conversion too)
   → Did Buy Box % drop? (purchases going to other sellers)

4. SQP Report
   → Did market competition increase? (more impressions competing)
   → Did our click share drop? (losing prominence)

5. Purchased Product Report
   → Is halo effect masking direct ACOS? (other SKU sales contributing)
   → Did product mix shift?

6. Advertised Product Report
   → Which specific ASINs have ACOS issues?
   → Is it concentrated or across the board?
```

```
Question: "Sessions dropped for Product X"

Data Sources to Check (in order):

1. Business Report
   → Confirm sessions drop with exact numbers
   → Check Buy Box % (lost BB = lost sessions)

2. SQP Report
   → Did YOUR impression share drop? → Lost organic rank
   → Did TOTAL search volume drop? → Market/seasonal decline
   → Did click share drop while impressions held? → CTR problem

3. Search Term Report (Advertising)
   → Did ad impressions drop? → Budget or bid issue
   → Were campaigns paused or budget capped?

4. Top Search Terms
   → Did the search terms themselves lose volume?
   → Did competitor ASINs take top positions?

5. Account Health
   → Listing suppressed?
   → Account health issues?
```

### 3.9 Dimensional Model: Metrics as Multi-Dimensional Facts

**Key Insight:** Every metric exists at the intersection of multiple dimensions. The same metric (e.g., CTR) means completely different things depending on which "cut" you're looking at. This dimensional model is the foundation for the analytics engine.

#### The Dimensions

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         DIMENSION SPACE                                 │
│                                                                         │
│  Every metric data point sits at the intersection of these dimensions:  │
│                                                                         │
│  1. TIME           When?        Daily / Weekly / Monthly / Custom       │
│  2. PRODUCT        What?        Child ASIN (what's being advertised)    │
│  3. PURCHASED      What sold?   Child ASIN (what was actually bought)   │
│  4. TARGETING      How found?   Keyword, Product ASIN, Category         │
│  5. MATCH TYPE     How matched? Broad / Phrase / Exact                  │
│  6. CAMPAIGN TYPE  What kind?   SP / SB / SD / Auto / Manual            │
│  7. CAMPAIGN       Which one?   Specific campaign name                  │
│  8. PLACEMENT      Where shown? Top of Search / Rest / Product Pages    │
│  9. SEARCH QUERY   What typed?  Customer's actual search term           │
│  10. BRAND         Whose?       Your brand (for SQP: vs. market)        │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

#### Metric Fact Table

Instead of a flat `MetricsSnapshot`, metrics are stored as **facts** with dimensional keys:

```
MetricFact {
  id

  # === DIMENSION KEYS (the "cuts") ===
  # Each dimension can be a specific value or null (meaning "all" / aggregated)

  timePeriod: DateRange               # ALWAYS present
  granularity: "daily" | "weekly" | "monthly"

  # Product dimensions
  childASIN: string | null            # null = all products
  parentASIN: string | null           # null = all parents
  brand: Brand                        # always scoped to a brand

  # Advertising dimensions
  campaignId: Campaign | null         # null = all campaigns
  campaignType: "SP" | "SB" | "SD" | null
  adGroupId: AdGroup | null           # null = all ad groups
  targetingType: "auto" | "manual_keyword" | "manual_product" | null

  # Targeting dimensions
  targetKeyword: string | null        # the keyword being targeted
  targetASIN: string | null           # the product being targeted
  matchType: "broad" | "phrase" | "exact" | null

  # Search dimensions
  customerSearchTerm: string | null   # actual search query (from Search Term Report)
  searchQuery: string | null          # search query (from SQP)

  # Placement dimension
  placement: "top_of_search" | "rest_of_search" | "product_pages" | null

  # Attribution dimension
  purchasedASIN: string | null        # what was actually bought (from Purchased Product Report)

  # === METRIC VALUES ===
  # Not all metrics available at every dimensional combination
  impressions: number | null
  clicks: number | null
  ctr: Percentage | null
  spend: Money | null
  sales: Money | null
  units: number | null
  orders: number | null
  cpc: Money | null
  acos: Percentage | null
  roas: number | null
  cvr: Percentage | null
  cartAdds: number | null

  # Share metrics (SQP only - when searchQuery is set)
  impressionShare: Percentage | null
  clickShare: Percentage | null
  cartAddShare: Percentage | null
  conversionShare: Percentage | null
  totalMarketImpressions: number | null
  totalMarketClicks: number | null
  totalMarketCartAdds: number | null
  totalMarketPurchases: number | null

  # Business Report metrics (when no ad dimensions set)
  sessions: number | null
  pageViews: number | null
  buyBoxPct: Percentage | null
  totalSales: Money | null
  organicSales: Money | null

  # SKU Attribution (when purchasedASIN is set)
  advertisedSkuSales: Money | null
  advertisedSkuUnits: number | null
  otherSkuSales: Money | null
  otherSkuUnits: number | null

  # Source tracking
  dataSource: "business_report" | "search_term_report" | "advertised_product_report"
             | "purchased_product_report" | "placement_report" | "sqp_report"
             | "search_catalog_performance" | "top_search_terms"
             | "repeat_purchase_report" | "mcp_server"
}
```

#### Which Dimensions Are Available Per Report

Not every dimension exists in every report. This is the availability matrix:

```
                        Time  Product  Purchased  Keyword  MatchType  Campaign  Placement  SearchQuery  Brand
Business Report          ✓      ✓                                                                        ✓
SQP Report               ✓      ✗         ✗        ✗        ✗          ✗          ✗           ✓          ✓ (share)
Search Catalog Perf      ✓      ✓                                                                        ✓
Top Search Terms         ✓      ✗ (top3)                                                     ✓          ✗ (market)
Search Term Report       ✓      ✓*                  ✓        ✓          ✓                     ✓**
Advertised Product       ✓      ✓                                       ✓
Purchased Product        ✓      ✓         ✓                             ✓
Placement Report         ✓                                              ✓          ✓
Market Basket            ✓      ✓         ✓ (co-purchased)
Repeat Purchase          ✓      ✓                                                                        ✓

* Search Term Report: product = the advertised ASIN
** Search Term Report: searchQuery = customer_search_term (what they actually typed)
```

#### The "Cuts" In Practice

Here are the specific analytical questions the dimensional model enables, and which dimensions you combine:

**Cut 1: Product Performance (which products perform best?)**
```
Dimensions: childASIN + timePeriod
Metrics: sessions, CVR, totalSales, BSR, buyBoxPct
Source: Business Report
Question: "How is each product performing this week vs last week?"
```

**Cut 2: Keyword Performance (which keywords drive results?)**
```
Dimensions: targetKeyword + matchType + childASIN + timePeriod
Metrics: impressions, clicks, CTR, CPC, spend, sales, ACOS, ROAS
Source: Search Term Report
Question: "Which keywords convert best for Product X at what match type?"
```

**Cut 3: Search Query × Product (how do we show up for each search?)**
```
Dimensions: searchQuery + timePeriod + brand
Metrics: impressionShare, clickShare, cartAddShare, conversionShare
Source: SQP Report
Question: "For 'bbq sauce', what's our impression share? Click share? Where do we lose in the funnel?"
```

**Cut 4: Campaign × Product (which campaigns work for which products?)**
```
Dimensions: campaignId + childASIN + timePeriod
Metrics: impressions, clicks, spend, sales, ACOS, ROAS
Source: Advertised Product Report
Question: "Campaign 'SP-BBQ-Exact' - which ASINs are profitable in it?"
```

**Cut 5: Placement Analysis (where do ads perform best?)**
```
Dimensions: placement + campaignId + timePeriod
Metrics: impressions, clicks, CTR, CPC, spend, sales, ACOS, CVR
Source: Placement Report
Question: "Does Top of Search convert better than Product Pages for this campaign?"
```

**Cut 6: Halo Analysis (what's actually being bought?)**
```
Dimensions: childASIN (advertised) + purchasedASIN + campaignId + timePeriod
Metrics: advertisedSkuSales, otherSkuSales, otherSkuUnits
Source: Purchased Product Report
Question: "When we advertise the 2-pack, do people buy the 4-pack instead?"
```

**Cut 7: Match Type Efficiency (broad vs phrase vs exact)**
```
Dimensions: matchType + targetKeyword + childASIN + timePeriod
Metrics: impressions, clicks, CTR, ACOS, ROAS, CVR
Source: Search Term Report (aggregated by match type)
Question: "Is 'bbq sauce' more efficient in exact or phrase match?"
```

**Cut 8: Cross-Product Comparison (same time, different products)**
```
Dimensions: [childASIN_1, childASIN_2, ...] + timePeriod (fixed)
Metrics: ANY metric
Source: Any report
Question: "Compare CVR of 2-pack vs 4-pack vs 6-pack this month"
```

**Cut 9: Time Series Comparison (same product, different periods)**
```
Dimensions: childASIN (fixed) + [timePeriod_1, timePeriod_2, ...]
Metrics: ANY metric
Source: Any report
Question: "How has ACOS trended for Product X over the last 12 weeks?"
```

**Cut 10: Campaign Type Comparison (SP vs SB vs SD)**
```
Dimensions: campaignType + childASIN + timePeriod
Metrics: impressions, clicks, spend, sales, ACOS, ROAS, NTB%
Source: Advertising Reports
Question: "Is Sponsored Brands driving more NTB sales than Sponsored Products?"
```

#### Action Impact at the Right Dimensional Level

This is crucial: **every action operates at a specific dimensional level**, and its impact should be measured at that same level.

```
┌──────────────────────────┬──────────────────────────────────────────────┐
│ Action                   │ Dimensional Level of Impact                  │
├──────────────────────────┼──────────────────────────────────────────────┤
│ increase_bid             │ Keyword × MatchType × Campaign × ASIN       │
│ negate_keyword           │ Keyword × Campaign                          │
│ change_match_type        │ Keyword × Campaign × ASIN                   │
│ adjust_placement_mod     │ Placement × Campaign                        │
│ increase_budget          │ Campaign (all ASINs, all keywords in it)    │
│ create_campaign          │ Campaign (new) × ASINs × Keywords           │
│ update_main_image        │ ASIN (across ALL keywords, campaigns)       │
│ update_title             │ ASIN (across ALL search queries)            │
│ create_a_plus_content    │ ASIN (CVR across all traffic sources)       │
│ change_price             │ ASIN (CVR, Buy Box across everything)       │
│ create_coupon            │ ASIN (CTR badge + CVR across everything)    │
│ restock_fba              │ ASIN (Buy Box, sessions across everything)  │
│ enroll_vine              │ ASIN (reviews → CVR long-term)              │
│ enroll_subscribe_save    │ ASIN (repeat purchase behavior)             │
└──────────────────────────┴──────────────────────────────────────────────┘
```

**Why this matters for the engine:**

When you change a bid on keyword "bbq sauce" (exact match) in campaign "SP-BBQ-Exact":
- **Measure at:** keyword "bbq sauce" × exact × "SP-BBQ-Exact" × time
- **Compare:** same keyword × same match × same campaign in prior period
- **Attribution confidence:** HIGH (very targeted change)

When you update the main image for ASIN B07P15CB6X:
- **Measure at:** ASIN B07P15CB6X × time (across ALL campaigns, keywords, placements)
- **Compare:** same ASIN in prior period, but ALSO compare sibling ASINs (did they improve too? if so, might be market-level)
- **Attribution confidence:** MEDIUM (many factors influence ASIN-level CTR)

#### Correlation & Prediction Framework

The dimensional model enables a correlation engine:

```
Given: Action A was taken at Dimension Level D at Time T

Step 1: ISOLATE
  - Pull MetricFacts at dimension level D for periods [T-1, T, T+1]
  - This is the "treatment" group

Step 2: COMPARE (control groups)
  - Same ASIN, different keywords (did ALL keywords improve, or just the targeted one?)
  - Same keyword, different ASINs (did ALL ASINs improve for this keyword?)
  - Same ASIN, same keyword, different time ranges (was it already trending?)

Step 3: ATTRIBUTE
  - If treatment improved AND control didn't → HIGH attribution to action
  - If treatment AND control both improved → LOW attribution (external factor)
  - If treatment improved less than control → action may have HURT

Step 4: LEARN
  - Store (Action Type, Dimension Level, Metric, Outcome) as a pattern
  - Over time: "When we increase bids on exact match keywords with >2% CVR,
    ACOS improves 80% of the time by ~12%"
  - This becomes PREDICTIVE: "If you increase this bid, expect ACOS to drop ~12%"
```

#### Composite Views (Pre-Built Cuts)

For the AM copilot, these are the pre-built analytical views:

| View Name | Dimensions Combined | Primary Question |
|-----------|-------------------|------------------|
| **Product Scorecard** | ASIN × Weekly | How is each product doing overall? |
| **Keyword P&L** | Keyword × Match × Campaign × ASIN × Weekly | Which keywords make money? |
| **Funnel Health** | SearchQuery × Brand × Weekly (SQP) | Where do we win/lose in the funnel? |
| **Campaign Efficiency** | Campaign × ASIN × Weekly | Which campaigns are efficient? |
| **Placement ROI** | Placement × Campaign × Weekly | Where should we bid more? |
| **Halo Map** | Advertised ASIN × Purchased ASIN × Weekly | What's the cross-sell effect? |
| **Market Position** | SearchQuery (top terms) × Weekly | How do we rank vs. competitors? |
| **Time Trend** | ASIN × [12 weeks] | What's the trajectory? |
| **Match Type Compare** | MatchType × Keyword × ASIN × Weekly | Broad vs Exact - which wins? |
| **Basket Analysis** | ASIN × Co-purchased ASIN | What bundles make sense? |
| **Customer Loyalty** | ASIN × Monthly | Who comes back? |

### 3.10 Prediction Engine: From Observation to Recommendation

The prediction engine is the intelligence layer that connects actions to outcomes and learns over time. It operates in 5 phases:

```
OBSERVE → CORRELATE → LEARN → PREDICT → RECOMMEND
```

#### Phase 1: OBSERVE (Data Collection)

Every time an action is taken and metrics are recorded, the system collects an **Observation Record**:

```
ObservationRecord {
  id

  # What was done
  action: Action                  # from Action entity (includes type, dimensional level)
  actionDate: Date
  actionMagnitude: number | null  # e.g., bid changed by +$0.15, price changed by -$2.00

  # Context at the time of action
  contextSnapshot: {
    # Metrics at the dimensional level of the action, BEFORE the action
    metricsBefore: MetricFact     # the exact dimensional cut, prior period
    metricsBaseline: MetricFact   # same cut, 4-week average (establishes "normal")

    # Product context
    childASIN: string
    parentASIN: string
    price: Money
    reviewCount: number
    starRating: number
    buyBoxPct: Percentage
    bsr: number

    # Competitive context
    competitorCount: number       # how many competitors in this space
    priceRank: number             # where is our price vs. competitors
    reviewRank: number            # where are our reviews vs. competitors

    # Advertising context (if ad-related action)
    campaignAge: number           # days since campaign created
    keywordAge: number | null     # days since keyword added
    currentBid: Money | null
    currentBudget: Money | null
    budgetUtilization: Percentage | null  # % of daily budget being spent

    # Temporal context
    dayOfWeek: string
    weekOfYear: number
    isHolidaySeason: boolean      # Q4, Prime Day, etc.
    daysSinceLastAction: number   # time since last change in this dimensional space
  }

  # What happened after (filled in during CORRELATE phase)
  outcomes: [MeasuredOutcome]     # measured at the right dimensional level

  # Control group data (filled in during CORRELATE phase)
  controlData: ControlGroupAnalysis | null
}
```

#### Phase 2: CORRELATE (Attribution Analysis)

When the measurement period arrives, the engine determines whether the action actually caused the observed metric change. This is the hardest part.

```
ControlGroupAnalysis {
  observationRecord: ObservationRecord

  # ═══════════════════════════════════════════════════════════════
  # TEST 1: Dimensional Isolation
  # Did only this dimension change, or did everything change?
  # ═══════════════════════════════════════════════════════════════

  dimensionalIsolation: {
    # For a bid change on keyword "bbq sauce" exact match:
    # Check: did OTHER keywords in the same campaign also improve?
    sameLevel: {
      metric: MetricType
      actionDimension: { before: number, after: number, changePct: Percentage }
      peerDimensions: [{           # other keywords in same campaign
        dimensionValue: string     # e.g., "grilling sauce"
        before: number
        after: number
        changePct: Percentage
      }]
    }
    # Result: if only "bbq sauce" improved → action likely caused it
    #         if all keywords improved → something else (listing change? seasonal?)
    isolationScore: Percentage     # 0% = everything moved, 100% = only this moved
  }

  # ═══════════════════════════════════════════════════════════════
  # TEST 2: Pre-existing Trend
  # Was the metric already trending before the action?
  # ═══════════════════════════════════════════════════════════════

  preTrend: {
    metric: MetricType
    priorPeriods: [{               # 4 weeks before action
      period: DateRange
      value: number
    }]
    trendDirection: "improving" | "flat" | "declining"
    trendMagnitude: Percentage     # avg weekly change before action

    postAction: {
      period: DateRange
      value: number
    }
    # Result: if metric was already improving at same rate → action didn't help
    #         if metric reversed or accelerated → action likely caused it
    trendBreakScore: Percentage    # 0% = no change from trend, 100% = complete reversal
  }

  # ═══════════════════════════════════════════════════════════════
  # TEST 3: Cross-Dimensional Check
  # Did the metric change at levels the action SHOULDN'T affect?
  # ═══════════════════════════════════════════════════════════════

  crossDimensional: {
    # For a bid change on one keyword:
    # Check: did this ASIN's organic metrics also change?
    # (A bid change shouldn't directly affect organic impressions)
    unaffectedDimensions: [{
      dimension: string            # e.g., "organic impressions"
      before: number
      after: number
      changePct: Percentage
    }]
    # Result: if unaffected dimensions also changed → external factor
    externalFactorScore: Percentage  # 0% = nothing else changed, 100% = everything changed
  }

  # ═══════════════════════════════════════════════════════════════
  # TEST 4: Confounding Action Check
  # Were other actions taken in overlapping dimensions?
  # ═══════════════════════════════════════════════════════════════

  confoundingActions: {
    overlappingActions: [{         # other actions that affect the same dimensional space
      action: Action
      overlapDegree: "full" | "partial" | "none"
      # e.g., a main image update (ASIN-level) overlaps with a keyword bid change
      # because the image affects CTR across ALL keywords
    }]
    confoundingScore: Percentage   # 0% = no overlapping actions, 100% = many overlapping actions
  }

  # ═══════════════════════════════════════════════════════════════
  # COMPOSITE ATTRIBUTION SCORE
  # ═══════════════════════════════════════════════════════════════

  attributionScore: {
    overall: Percentage            # 0-100% confidence that action caused outcome
    breakdown: {
      isolationContribution: Percentage
      trendBreakContribution: Percentage
      externalFactorPenalty: Percentage
      confoundingPenalty: Percentage
    }
    verdict: "high_confidence" | "likely" | "possible" | "unlikely" | "no_attribution"
    explanation: string            # human-readable: "High confidence: only this keyword
                                   # improved while peers were flat, and the metric
                                   # reversed a declining trend."
  }
}
```

**Attribution Scoring Formula:**

```
attributionScore = (
    isolationScore × 0.35          # most important: did ONLY this change?
  + trendBreakScore × 0.25         # did the action change the trajectory?
  - externalFactorScore × 0.20     # penalty: external factors present
  - confoundingScore × 0.20        # penalty: other actions overlap
)

Verdict:
  ≥ 80% → "high_confidence"
  ≥ 60% → "likely"
  ≥ 40% → "possible"
  ≥ 20% → "unlikely"
  < 20% → "no_attribution"
```

#### Phase 3: LEARN (Pattern Accumulation)

Over time, each attributed observation becomes a **Pattern** in the pattern database:

```
ActionPattern {
  id

  # What was done (generalized)
  actionType: ActionType
  actionMagnitudeRange: { min: number, max: number } | null  # e.g., bid increase 10-20%
  dimensionalLevel: string        # e.g., "keyword×match×campaign"

  # Context conditions (what made this pattern apply)
  conditions: [{
    field: string                  # e.g., "cvr", "cpc", "reviewCount", "competitorCount"
    operator: ">" | "<" | ">=" | "<=" | "between" | "equals"
    value: any
  }]

  # Observed outcomes (aggregated across many observations)
  outcomes: [{
    metric: MetricType
    direction: "increase" | "decrease"
    avgChangePct: Percentage
    medianChangePct: Percentage
    stdDev: Percentage
    successRate: Percentage        # % of times the metric moved in expected direction
    avgTimeToEffect: number        # days until effect visible
    sampleSize: number             # how many observations
    confidenceInterval: { low: Percentage, high: Percentage }
  }]

  # Pattern metadata
  totalObservations: number
  avgAttributionScore: Percentage
  lastUpdated: DateTime
  applicableCategories: [string]   # product categories where this pattern holds
  seasonalVariation: boolean       # does this pattern behave differently by season?
}
```

**Example Patterns (what the system learns over time):**

```
Pattern: "Exact Match Bid Increase on High-CVR Keywords"
─────────────────────────────────────────────────────────
Action: increase_bid
Dimensional Level: keyword × exact × campaign
Conditions:
  - CVR > 10%
  - Current bid < $1.50
  - Keyword has 100+ impressions/week
Outcomes:
  - Impressions: ↑ 25-40% (92% success rate, 3-5 day effect)
  - Clicks: ↑ 20-35% (90% success rate, 3-5 day effect)
  - ACOS: ↓ 5-12% (68% success rate, 7-14 day effect)
  - Organic Rank: ↑ (54% success rate, 30-60 day effect)
Sample Size: 47 observations
Avg Attribution Score: 78%

Pattern: "Main Image Update on Low-CTR Products"
─────────────────────────────────────────────────
Action: update_main_image
Dimensional Level: ASIN (across all keywords)
Conditions:
  - CTR (organic) < 0.3%
  - Review count > 50 (listing is established)
  - No other listing changes in prior 14 days
Outcomes:
  - CTR (organic): ↑ 15-30% (84% success rate, 7-14 day effect)
  - CTR (paid): ↑ 12-25% (80% success rate, 7-14 day effect)
  - Sessions: ↑ 10-20% (78% success rate, 14-21 day effect)
  - Sales: ↑ 8-18% (72% success rate, 14-30 day effect)
Sample Size: 23 observations
Avg Attribution Score: 71%

Pattern: "Negating Irrelevant Search Terms (>$5 spend, 0 sales)"
────────────────────────────────────────────────────────────────
Action: negate_keyword
Dimensional Level: keyword × campaign
Conditions:
  - Keyword spend > $5
  - Keyword sales = $0
  - Keyword impressions > 500
Outcomes:
  - ACOS (campaign): ↓ 3-8% (88% success rate, 3-7 day effect)
  - Wasted spend: ↓ 100% on that term (100% success rate, immediate)
  - Total sales: neutral (95% of the time)
Sample Size: 156 observations
Avg Attribution Score: 92%

Pattern: "Price Decrease During Competitive Season"
───────────────────────────────────────────────────
Action: change_price (decrease 5-15%)
Dimensional Level: ASIN
Conditions:
  - Q4 or competitive period
  - Current price > category median
  - Buy Box % < 95%
Outcomes:
  - CVR: ↑ 12-25% (82% success rate, immediate)
  - Buy Box %: ↑ to ~100% (90% success rate, immediate)
  - Sales: ↑ 20-40% (78% success rate, 3-7 day effect)
  - BSR: ↑ (lower number) 30-50% (70% success rate, 7-14 day effect)
  - Margin: ↓ proportional (100% certainty, immediate)
Sample Size: 34 observations
Avg Attribution Score: 85%
```

#### Phase 4: PREDICT (Forward-Looking Estimates)

Given a proposed action and current context, the engine predicts outcomes:

```
Prediction {
  id

  # The proposed action
  proposedAction: {
    type: ActionType
    target: string                 # ASIN, keyword, campaign
    magnitude: number | null       # how much (bid change amount, price change %, etc.)
    dimensionalLevel: string
  }

  # Current context (auto-populated from latest MetricFacts)
  currentContext: {
    metricsAtDimension: MetricFact
    productContext: { price, reviews, rating, bsr, buyBoxPct, ... }
    competitiveContext: { competitorCount, priceRank, ... }
    temporalContext: { season, daysSinceLastAction, ... }
  }

  # Matching patterns (which learned patterns apply?)
  matchingPatterns: [{
    pattern: ActionPattern
    matchScore: Percentage          # how well does current context match pattern conditions
    weight: number                  # contribution to prediction (based on match + sample size)
  }]

  # Predicted outcomes (weighted average across matching patterns)
  predictions: [{
    metric: MetricType
    expectedDirection: "increase" | "decrease" | "neutral"
    expectedChangePct: {
      low: Percentage              # pessimistic estimate (25th percentile)
      mid: Percentage              # expected value (median)
      high: Percentage             # optimistic estimate (75th percentile)
    }
    probability: Percentage         # chance of moving in expected direction
    timeToEffect: {
      earliest: number             # days (fastest expected)
      typical: number              # days (most likely)
      latest: number               # days (slowest expected)
    }
    confidence: "high" | "medium" | "low"  # based on pattern sample size + match quality
  }]

  # Risk assessment
  risks: [{
    description: string            # e.g., "ACOS may increase if bid goes too high"
    probability: Percentage
    severity: "high" | "medium" | "low"
    mitigation: string             # e.g., "Set a max bid of $1.80 and review in 7 days"
  }]

  # Compound effects (what else might happen)
  secondaryEffects: [{
    metric: MetricType
    effect: string                 # e.g., "Increased sales velocity may improve organic rank"
    timeframe: string              # e.g., "30-60 days"
    certainty: "likely" | "possible" | "speculative"
  }]

  # What to monitor
  monitoringPlan: {
    primaryMetric: MetricType
    measureAt: string              # dimensional level
    checkAfterDays: number
    successThreshold: {            # what counts as success
      metric: MetricType
      direction: "increase" | "decrease"
      minimumChange: Percentage
    }
    failureThreshold: {            # when to abort/reverse
      metric: MetricType
      direction: "increase" | "decrease"
      maximumTolerance: Percentage
    }
  }
}
```

**Example Prediction:**

```
Proposed Action: Increase bid on "bbq sauce" (exact) from $1.10 to $1.35 (+23%)
Campaign: SP-BBQ-Exact
ASIN: B07P15CB6X

Current Context:
  - CTR: 0.42%, CVR: 12.3%, ACOS: 34%
  - CPC: $0.89, Impressions: 2,100/week
  - BSR: #4,521, Reviews: 287, Rating: 4.3
  - Competitor density: Medium (8 competitors in price range)
  - Season: Non-peak

Matching Patterns:
  1. "Exact Match Bid Increase on High-CVR Keywords" (match: 89%, weight: 0.6)
  2. "Bid Increase on Mid-Competition Keywords" (match: 72%, weight: 0.3)
  3. "General Bid Increase" (match: 55%, weight: 0.1)

Predictions:
  ┌────────────────┬───────────┬──────────────────┬───────┬───────────────┐
  │ Metric         │ Direction │ Change (lo/mid/hi)│ Prob  │ Time to Effect│
  ├────────────────┼───────────┼──────────────────┼───────┼───────────────┤
  │ Impressions    │ ↑         │ +18% / +30% / +45%│ 92%  │ 3-5 days      │
  │ Clicks         │ ↑         │ +15% / +26% / +38%│ 90%  │ 3-5 days      │
  │ CPC            │ ↑         │ +8% / +15% / +22% │ 88%  │ Immediate     │
  │ Ad Spend       │ ↑         │ +25% / +40% / +55%│ 95%  │ Immediate     │
  │ ACOS           │ ↓         │ -2% / -8% / -14%  │ 68%  │ 7-14 days     │
  │ Ad Sales       │ ↑         │ +20% / +35% / +50%│ 85%  │ 5-10 days     │
  │ Organic Rank   │ ↑         │ subtle            │ 54%  │ 30-60 days    │
  └────────────────┴───────────┴──────────────────┴───────┴───────────────┘

Risks:
  1. CPC may spike above $1.20 if competition is bidding aggressively (prob: 35%)
     → Mitigation: Monitor CPC daily for first week, reduce if CPC > $1.25

  2. ACOS may increase if new impressions come from less-relevant placements (prob: 32%)
     → Mitigation: Check Placement Report after 7 days, adjust TOS modifier if needed

Monitoring Plan:
  Primary: ACOS at keyword "bbq sauce" × exact × SP-BBQ-Exact
  Check after: 10 days
  Success: ACOS decreases by ≥3%
  Failure: ACOS increases by >5% → consider reverting bid
```

#### Phase 5: RECOMMEND (Goal-Driven Action Selection)

The recommendation engine works backward from goals to suggest optimal actions:

```
Recommendation {
  id

  # What we're trying to achieve
  goal: Goal                       # e.g., "Reduce TACoS from 18% to 14% by March"
  currentState: {
    metric: MetricType
    currentValue: number
    targetValue: number
    gapToClose: Percentage
    deadlineDays: number
  }

  # Available action space (what CAN we do?)
  availableActions: [{
    action: ActionType
    target: string                 # specific ASIN, keyword, campaign
    magnitude: number | null
    prediction: Prediction         # from Phase 4
    impactOnGoal: {
      metric: MetricType
      expectedContribution: Percentage  # how much of the gap this closes
    }
    effort: "low" | "medium" | "high"
    reversibility: "easy" | "moderate" | "difficult"
    risk: "low" | "medium" | "high"
  }]

  # Recommended action plan (ranked by impact/effort/risk)
  plan: [{
    priority: number
    action: ActionType
    target: string
    magnitude: number | null
    reasoning: string              # why this action, in plain language
    expectedImpactOnGoal: Percentage
    timeToEffect: string
    dependencies: [number] | null  # other actions that should happen first
  }]

  # Cumulative projection
  projection: {
    ifAllActionsExecuted: {
      expectedGoalValue: number
      expectedGoalDate: Date
      confidence: "high" | "medium" | "low"
    }
    milestones: [{
      date: Date
      expectedValue: number
      actionsCompleted: [number]
    }]
  }

  # What could go wrong
  riskAnalysis: {
    bestCase: string
    expectedCase: string
    worstCase: string
    contingencyActions: [{          # what to do if things go wrong
      trigger: string              # e.g., "If TACoS hasn't improved after 14 days"
      action: string               # e.g., "Audit Search Term Report for wasted spend"
    }]
  }
}
```

**Example Recommendation:**

```
Goal: Reduce TACoS from 18% to 14% in 6 weeks
Current: $4,500 ad spend / $25,000 total sales = 18% TACoS
Target: $3,500 ad spend / $25,000 total sales = 14% OR
        $4,500 ad spend / $32,100 total sales = 14%

Strategy: TWO PATHS (reduce waste + increase organic sales)

Recommended Plan:
┌────┬────────────────────────────────┬──────────┬────────────────┬──────┐
│ #  │ Action                         │ Impact   │ Time to Effect │ Risk │
├────┼────────────────────────────────┼──────────┼────────────────┼──────┤
│ 1  │ Negate 23 zero-sale keywords   │ -$380/wk │ 3-7 days       │ Low  │
│    │ ($380/wk wasted spend)         │ spend    │                │      │
├────┼────────────────────────────────┼──────────┼────────────────┼──────┤
│ 2  │ Move 8 high-CVR broad keywords │ -1.2%    │ 7-14 days      │ Low  │
│    │ to exact match (reduce waste)  │ ACOS     │                │      │
├────┼────────────────────────────────┼──────────┼────────────────┼──────┤
│ 3  │ Update A+ content on top 2     │ +3-5%    │ 14-30 days     │ Low  │
│    │ ASINs (improve CVR→lower ACOS) │ CVR      │                │      │
├────┼────────────────────────────────┼──────────┼────────────────┼──────┤
│ 4  │ Increase TOS modifier to 40%   │ +20%     │ 7-14 days      │ Med  │
│    │ on top 5 keywords (rank push)  │ TOS imps │                │      │
├────┼────────────────────────────────┼──────────┼────────────────┼──────┤
│ 5  │ Launch 10% coupon for 2 weeks  │ +15%     │ Immediate      │ Med  │
│    │ on lead ASIN (velocity boost)  │ sales vel│                │      │
└────┴────────────────────────────────┴──────────┴────────────────┴──────┘

Projection:
  Week 1-2: Negation saves $380/wk → TACoS drops to ~16.5%
  Week 2-3: Exact match migration improves ACOS → TACoS ~15.8%
  Week 3-4: A+ content improves CVR → TACoS ~15.2%
  Week 4-5: TOS push + coupon drive sales velocity → TACoS ~14.5%
  Week 6:   Organic rank improvements from velocity → TACoS ~14.0%

  Confidence: MEDIUM (dependent on organic rank improvement materializing)

Contingency:
  If TACoS > 16% after week 3:
    → Deep audit of Search Term Report for new wasted spend sources
    → Consider pausing underperforming campaigns entirely
  If coupon doesn't lift velocity:
    → Switch to Lightning Deal if available
    → Increase bid aggressively on top 3 keywords for 2-week push
```

#### The Learning Feedback Loop

The complete cycle creates a self-improving system:

```
                  ┌─────────────────────────┐
                  │                         │
                  │    RECOMMEND action     │◄────────────────────┐
                  │    (based on patterns)  │                     │
                  └───────────┬─────────────┘                     │
                              │ AM executes                       │
                              ▼                                   │
                  ┌─────────────────────────┐                     │
                  │                         │                     │
                  │    OBSERVE outcomes     │                     │
                  │    (collect MetricFacts) │                     │
                  └───────────┬─────────────┘                     │
                              │ measurement period                │
                              ▼                                   │
                  ┌─────────────────────────┐                     │
                  │                         │                     │
                  │    CORRELATE cause      │                     │
                  │    (attribution tests)  │                     │
                  └───────────┬─────────────┘                     │
                              │ attributed outcome                │
                              ▼                                   │
                  ┌─────────────────────────┐                     │
                  │                         │                     │
                  │    LEARN patterns       │                     │
                  │    (update ActionPattern)│                     │
                  └───────────┬─────────────┘                     │
                              │ improved predictions              │
                              ▼                                   │
                  ┌─────────────────────────┐                     │
                  │                         │                     │
                  │    PREDICT better       │─────────────────────┘
                  │    (more accurate)      │
                  └─────────────────────────┘

Each cycle improves:
  1. Attribution accuracy (better control groups)
  2. Pattern precision (more observations, tighter confidence intervals)
  3. Prediction accuracy (better context matching)
  4. Recommendation quality (knows what works for this brand/category)
```

#### Cold Start: When Patterns Don't Exist Yet

For new brands or actions with no history, the system uses fallback layers:

```
Pattern Lookup Hierarchy:
  1. THIS brand, THIS product, THIS action type
     → Best: most relevant patterns
     → Problem: may have 0 observations

  2. THIS brand, ANY product, THIS action type
     → Good: same brand behavior
     → Available after a few weeks

  3. SAME category, ANY brand, THIS action type
     → Decent: category-level patterns
     → Available from agency-wide data

  4. ANY category, ANY brand, THIS action type
     → Baseline: action-level averages
     → Available from day 1 (built from research + industry data)

  5. Expert knowledge (from KnowledgeDocuments)
     → Fallback: frameworks from training, calls, research
     → Always available (see Section 3.2 Action→Metric Impact Matrix)
```

#### Confounding Factor Detection

The engine tracks known confounders that can explain metric changes independent of actions:

```
ConfounderRegistry {
  confounders: [

    # Temporal
    {
      name: "Seasonality"
      detection: "Compare YoY data for same period"
      affectsMetrics: ["sessions", "cvr", "total_sales", "cpc", "impressions"]
      severity: "high" (Q4, Prime Day) | "medium" (back to school) | "low"
    }

    # Competitive
    {
      name: "New Competitor Entry"
      detection: "SQP impression share decline + new ASINs in Top Search Terms"
      affectsMetrics: ["impression_share", "click_share", "ctr", "cvr", "bsr"]
      severity: varies
    }

    # Platform
    {
      name: "Amazon Algorithm Change"
      detection: "Broad metric shifts across many brands simultaneously"
      affectsMetrics: ["organic_rank", "impressions_organic", "sessions"]
      severity: "high"
    }

    # Product
    {
      name: "Review Shock"
      detection: "Star rating drop >0.2 in short period OR negative review goes viral"
      affectsMetrics: ["cvr", "ctr", "total_sales"]
      severity: "high"
    }

    # Operational
    {
      name: "Stock Level Change"
      detection: "Days of cover < 14 OR Buy Box % drop without price change"
      affectsMetrics: ["buy_box_pct", "sessions", "total_sales", "bsr"]
      severity: "critical"
    }

    # External
    {
      name: "External Traffic Event"
      detection: "Session spike without corresponding impression increase"
      affectsMetrics: ["sessions", "total_sales", "bsr"]
      severity: "medium"
    }

    # Pricing
    {
      name: "Competitor Price Change"
      detection: "CVR change without own price change + competitor price data"
      affectsMetrics: ["cvr", "buy_box_pct", "total_sales"]
      severity: "medium"
    }
  ]
}
```

---

## Part 4: Core Entity Map

### 4.1 Entity Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                        ORGANIZATION                                 │
│  Agency ──has──▶ Team ──has──▶ AccountManager                       │
└──────────────────────────┬──────────────────────────────────────────┘
                           │ manages (via BrandAssignment)
                           ▼
┌─────────────────────────────────────────────────────────────────────┐
│                        CLIENT                                       │
│  Brand ──has──▶ Marketplace(s)                                      │
│    │                                                                │
│    ├──has──▶ Product (Parent ASIN)                                   │
│    │           └──has──▶ Variant (Child ASIN)                        │
│    │                      └──has──▶ Listing                          │
│    │                      └──has──▶ SKU(s)                           │
│    │                                                                │
│    ├──has──▶ AdvertisingAccount                                      │
│    │           └──has──▶ Portfolio                                    │
│    │                      └──has──▶ Campaign                         │
│    │                                 └──has──▶ AdGroup               │
│    │                                            ├──targets──▶Keyword │
│    │                                            └──advertises──▶ASIN │
│    │                                                                │
│    ├──has──▶ BrandMemory (rolling context)                           │
│    │                                                                │
│    ├──has──▶ OnboardingProcess                                       │
│    │           └──has──▶ Phase ──has──▶ ChecklistItem                │
│    │                                                                │
│    └──has──▶ many of:                                                │
│              LogEntry, Decision, Strategy, Goal, Report,             │
│              ClientUpdate, ActionItem, Alert, Competitor             │
└─────────────────────────────────────────────────────────────────────┘
```

### 4.2 Entity Definitions

#### Organization Entities

**Agency**
```
Agency {
  id
  name
  teams: [Team]
}
```

**Team**
```
Team {
  id
  name
  agency: Agency
  members: [AccountManager]
  brands: [Brand] (via BrandAssignment)
}
```

**AccountManager**
```
AccountManager {
  id
  name
  email
  role: "am" | "specialist" | "strategist" | "director"
  specializations: ["ads", "listings", "analytics", ...]
  team: Team
  assignments: [BrandAssignment]
}
```

**BrandAssignment**
```
BrandAssignment {
  id
  accountManager: AccountManager
  brand: Brand
  role: "primary" | "support"
  scope: "full_service" | "ads_only" | "listings_only" | "strategy_only"
  startDate: Date
  endDate: Date | null
  active: boolean
}
```

#### Client Entities

**Brand**
```
Brand {
  id
  name
  slug                          # lowercase-hyphenated
  marketplace: "US" | "UK" | "CA" | "DE" | ...
  category: string
  subcategory: string
  businessModel: "FBA" | "FBM" | "Hybrid" | "Vendor"
  status: "prospect" | "onboarding" | "active" | "paused" | "churned"
  sellerCentralId: string
  brandRegistryStatus: "registered" | "pending" | "not_registered"

  # Financials
  monthlyRetainer: Money | null
  adBudget: Money | null
  marginTarget: Percentage | null

  # Relationships
  products: [Product]
  assignments: [BrandAssignment]
  memory: BrandMemory
  onboarding: OnboardingProcess
  strategies: [Strategy]
  goals: [Goal]
  logs: [LogEntry]
  decisions: [Decision]
  reports: [Report]
  alerts: [Alert]
  competitors: [Competitor]
  clientUpdates: [ClientUpdate]
}
```

**Product (Parent ASIN)**
```
Product {
  id
  brand: Brand
  parentASIN: string
  name: string
  category: string
  subcategory: string
  variationTheme: "Size" | "Color" | "SizeColor" | "Flavor" | ...
  status: "active" | "inactive" | "discontinued"

  variants: [Variant]
  competitors: [Competitor]
  goals: [Goal]
  actionItems: [ActionItem]
}
```

**Variant (Child ASIN)**
```
Variant {
  id
  product: Product
  childASIN: string
  variantName: string            # e.g., "16oz 2-Pack"
  sku: string
  price: Money
  fulfillment: "FBA" | "FBM" | "SFP"
  status: "active" | "suppressed" | "out_of_stock" | "inactive"

  # Physical
  weight: Weight | null
  dimensions: Dimensions | null
  unitsPerPack: number

  # Listing
  listing: Listing

  # Performance (latest snapshot)
  latestMetrics: MetricsSnapshot | null
}
```

**Listing**
```
Listing {
  id
  variant: Variant
  title: string
  bulletPoints: [string]          # typically 5
  description: string
  mainImage: URL
  additionalImages: [URL]
  video: URL | null
  aPlusContent: boolean
  aPlusVersion: string | null
  backendKeywords: string
  lastUpdated: DateTime

  # Audit fields
  qualityScore: number | null     # 0-100 internal assessment
  lastAuditDate: Date | null
}
```

#### Advertising Entities

**Portfolio**
```
Portfolio {
  id
  brand: Brand
  name: string
  budgetCap: Money | null
  budgetPeriod: "monthly" | "daily" | null
  campaigns: [Campaign]
}
```

**Campaign**
```
Campaign {
  id
  portfolio: Portfolio | null
  brand: Brand
  name: string
  type: "SP" | "SB" | "SD"
  targeting: "auto" | "manual_keyword" | "manual_product"
  status: "enabled" | "paused" | "archived"
  dailyBudget: Money
  startDate: Date
  endDate: Date | null
  biddingStrategy: "dynamic_down" | "dynamic_up_down" | "fixed"
  placementModifiers: {
    topOfSearch: Percentage | null
    productPages: Percentage | null
    restOfSearch: Percentage | null
  }
  adGroups: [AdGroup]
}
```

**AdGroup**
```
AdGroup {
  id
  campaign: Campaign
  name: string
  defaultBid: Money
  status: "enabled" | "paused"
  advertisedASINs: [Variant]      # the products being shown
  targets: [Target]               # what triggers the ad
}
```

**Target**
```
Target {
  id
  adGroup: AdGroup
  type: "keyword" | "product" | "category" | "audience"
  value: string                   # the keyword text, ASIN, or category
  matchType: "broad" | "phrase" | "exact" | null  # null for non-keyword
  bid: Money
  status: "enabled" | "paused" | "negated"
}
```

#### Activity & Work Entities

**LogEntry**
```
LogEntry {
  id
  brand: Brand
  author: AccountManager
  date: Date
  type: "weekly" | "poa" | "action" | "observation"
  products: [Product | Variant]    # ASINs referenced
  content: StructuredMarkdown
  tags: [string]

  # Extracted entities
  actions: [Action]
  decisions: [Decision]
  actionItems: [ActionItem]

  # File reference (current system)
  filePath: string                 # e.g., "brands/acme/logs/2026-01-15-john-weekly.md"
}
```

**Action** (an activity that was executed)
```
Action {
  id
  logEntry: LogEntry              # source log
  type: ActionType                # from the Action Taxonomy (section 3.1)
  target: Campaign | Variant | Listing | Brand  # what was acted on
  description: string
  details: {
    before: any                   # previous state
    after: any                    # new state
  }

  # Dimensional context (at what level does this action operate?)
  # See Section 3.9 "Action Impact at the Right Dimensional Level"
  dimensionalLevel: {
    childASIN: string | null      # which product was acted on
    campaignId: string | null     # which campaign
    targetKeyword: string | null  # which keyword
    matchType: string | null      # which match type
    placement: string | null      # which placement
  }

  # Impact tracking
  expectedImpacts: [MetricImpact]
  status: "planned" | "executed" | "monitoring" | "measured"
  measuredOutcomes: [MeasuredOutcome]
  measurementDueDate: Date | null  # when to check results
}
```

**MetricImpact** (expected effect of an action)
```
MetricImpact {
  metric: MetricType              # from Metrics Taxonomy
  direction: "increase" | "decrease" | "stabilize"
  magnitude: string | null        # "~5%", "significant", etc.
  confidence: "high" | "medium" | "low"
  timeToMeasure: Duration         # when to check
  measureAtDimension: string      # e.g., "keyword×match×campaign" or "asin" or "account"
}
```

**MeasuredOutcome** (actual result of an action, measured at the right dimensional level)
```
MeasuredOutcome {
  action: Action
  metric: MetricType

  # What was measured
  dimensionalCut: {               # the specific "cut" used for measurement
    childASIN: string | null
    campaignId: string | null
    targetKeyword: string | null
    matchType: string | null
    placement: string | null
  }

  before: number                  # metric value in prior period
  after: number                   # metric value in measurement period
  change: Percentage
  measurementPeriod: DateRange

  # Attribution analysis (see Section 3.9 "Correlation & Prediction Framework")
  attribution: "high" | "medium" | "low"
  attributionReason: string       # e.g., "Only this keyword improved; others flat"

  # Control group comparison
  controlComparison: {
    sameASINOtherKeywords: Percentage | null    # did other keywords also change?
    sameKeywordOtherASINs: Percentage | null    # did other ASINs also change?
    priorTrend: Percentage | null               # was this already trending?
  } | null

  notes: string | null
}
```

**ActionItem** (a task to be done)
```
ActionItem {
  id
  brand: Brand
  product: Product | Variant | null
  description: string
  priority: "critical" | "high" | "medium" | "low"
  category: ActionType            # what kind of action
  dueDate: Date | null
  status: "open" | "in_progress" | "done" | "cancelled"
  assignee: AccountManager | null
  sourceLog: LogEntry | null      # where it was identified
  sourceDecision: Decision | null # or which decision spawned it
  completedLog: LogEntry | null   # the log that records its completion
}
```

#### Decision & Strategy Entities

**Decision**
```
Decision {
  id
  brand: Brand
  product: Product | null
  date: Date
  author: AccountManager
  title: string
  context: string                 # why this decision was needed
  options: [{
    name: string
    pros: [string]
    cons: [string]
  }]
  chosen: string                  # which option was selected
  rationale: string               # why this option
  expectedOutcome: string
  reviewDate: Date                # when to evaluate
  actualOutcome: string | null    # filled later
  status: "active" | "superseded" | "validated" | "reversed"
  resultingActions: [ActionItem]  # what needs to happen because of this decision
  sourceLog: LogEntry | null
}
```

**Strategy**
```
Strategy {
  id
  brand: Brand
  period: string                  # "Q1 2026", "February 2026", etc.
  author: AccountManager
  status: "draft" | "active" | "completed" | "revised"

  # Strategic framework
  goals: [Goal]
  constraints: [{
    type: "budget" | "margin" | "timeline" | "resource"
    description: string
    value: any
  }]
  tactics: [{
    area: "advertising" | "listing" | "pricing" | "inventory" | "reviews"
    description: string
    expectedImpact: [MetricImpact]
    actionItems: [ActionItem]
  }]

  # Review
  reviewDate: Date
  outcomes: string | null
}
```

**Goal**
```
Goal {
  id
  brand: Brand
  product: Product | null
  metric: MetricType
  targetValue: number
  currentValue: number
  direction: "increase" | "decrease" | "maintain"
  deadline: Date
  status: "on_track" | "at_risk" | "off_track" | "achieved" | "abandoned"
  strategy: Strategy | null
  history: [{
    date: Date
    value: number
  }]
}
```

#### Performance Entities

**MetricFact** (see Section 3.9 for full dimensional model)

The core data entity. Every metric data point is stored with its dimensional keys, enabling any analytical "cut". See Section 3.9 for the complete schema.

```
MetricFact {
  id

  # Dimension keys (each can be null = aggregated across that dimension)
  timePeriod: DateRange
  granularity: "daily" | "weekly" | "monthly"
  brand: Brand
  childASIN: string | null
  parentASIN: string | null
  campaignId: Campaign | null
  campaignType: "SP" | "SB" | "SD" | null
  adGroupId: AdGroup | null
  targetKeyword: string | null
  targetASIN: string | null
  matchType: "broad" | "phrase" | "exact" | null
  customerSearchTerm: string | null
  searchQuery: string | null
  placement: "top_of_search" | "rest_of_search" | "product_pages" | null
  purchasedASIN: string | null

  # Metric values (availability depends on dimensions + data source)
  impressions, clicks, ctr, spend, sales, units, orders, cpc, acos, roas,
  cvr, cartAdds, sessions, pageViews, buyBoxPct, totalSales, organicSales,
  impressionShare, clickShare, cartAddShare, conversionShare,
  advertisedSkuSales, otherSkuSales, ...

  dataSource: string              # which Amazon report this came from
}
```

**ProductScorecard** (a pre-aggregated summary view per ASIN per period)

This is a convenience entity - a materialized view over MetricFacts at the ASIN × Weekly level. Used for quick dashboards and client updates.

```
ProductScorecard {
  id
  variant: Variant
  period: DateRange
  granularity: "weekly" | "monthly"

  # Traffic
  sessions: number | null
  pageViews: number | null
  impressionsPaid: number | null

  # Conversion
  ctrPaid: Percentage | null
  cvr: Percentage | null
  buyBoxPct: Percentage | null

  # Advertising
  adSpend: Money | null
  adSales: Money | null
  acos: Percentage | null
  roas: number | null
  tacos: Percentage | null
  cpc: Money | null

  # Revenue
  totalSales: Money | null
  totalUnits: number | null
  organicSales: Money | null
  organicSharePct: Percentage | null

  # Ranking
  bsr: number | null

  # Customer
  starRating: number | null
  reviewCount: number | null

  # Change vs prior period
  changes: {
    [metricName]: {
      prior: number
      current: number
      changePct: Percentage
      direction: "up" | "down" | "flat"
      significance: "significant" | "minor" | "noise"
    }
  }
}
```

**AccountScorecard** (brand-level rollup)

```
AccountScorecard {
  id
  brand: Brand
  period: DateRange
  granularity: "weekly" | "monthly"

  # Rolled-up metrics across all products
  totalSales: Money
  totalUnits: number
  totalAdSpend: Money
  totalAdSales: Money
  tacos: Percentage
  avgAcos: Percentage
  totalSessions: number
  avgCvr: Percentage
  organicSharePct: Percentage

  # Per-product breakdown
  productScorecards: [ProductScorecard]

  # Top SQP positions
  topSearchQueries: [{
    query: string
    impressionShare: Percentage
    clickShare: Percentage
    conversionShare: Percentage
    trend: "gaining" | "stable" | "losing"
  }]

  # Flywheel health
  flywheelStatus: "healthy" | "stalling" | "broken"
  tacosDirection: "improving" | "flat" | "worsening"
  organicTrend: "growing" | "flat" | "declining"
}
```

**Alert**
```
Alert {
  id
  brand: Brand
  product: Product | Variant | null
  type: "threshold_breach" | "anomaly" | "trend" | "stockout_risk"
       | "buybox_loss" | "rank_drop" | "acos_spike" | "session_drop"
  severity: "critical" | "warning" | "info"
  metric: MetricType
  condition: string               # "Buy Box % dropped below 50%"
  currentValue: number
  thresholdValue: number | null
  triggeredAt: DateTime
  acknowledgedAt: DateTime | null
  resolvedAt: DateTime | null
  acknowledgedBy: AccountManager | null
  relatedAction: ActionItem | null

  # Diagnostic context
  suggestedDiagnostic: string | null  # which diagnostic tree to use
  relatedReports: [string]            # which Amazon reports to check
}
```

**Report**
```
Report {
  id
  brand: Brand
  type: "weekly_performance" | "monthly_review" | "quarterly_strategy"
       | "sqp_analysis" | "ad_analysis" | "competitor_analysis"
       | "business_report" | "data_dive" | "inventory_analysis"
  period: DateRange
  author: AccountManager
  status: "draft" | "final" | "sent_to_client"

  # Content
  metricsSnapshot: MetricsSnapshot
  insights: [{
    category: string
    finding: string
    severity: "positive" | "neutral" | "negative"
    dataSource: string            # which Amazon report supports this
    recommendation: string | null
  }]
  recommendations: [ActionItem]

  # File reference (current system)
  filePath: string | null
}
```

#### Client Communication Entities

**ClientUpdate**
```
ClientUpdate {
  id
  brand: Brand
  date: Date
  author: AccountManager
  type: "weekly_email" | "monthly_report" | "quarterly_review" | "ad_hoc"
  channel: "email" | "call" | "video" | "slack"

  # Content
  highlights: [{                  # wins to celebrate
    metric: MetricType | null
    description: string
    value: string | null
  }]
  concerns: [{                    # things to address proactively
    description: string
    severity: "high" | "medium" | "low"
    proposedAction: string
  }]
  clientRequests: [{              # things the client asked for
    description: string
    priority: "high" | "medium" | "low"
    status: "new" | "acknowledged" | "in_progress" | "done"
    actionItem: ActionItem | null
  }]
  nextSteps: [string]
  sentAt: DateTime | null

  # Links
  report: Report | null
  sourceLog: LogEntry | null
}
```

#### Competitive Intelligence Entities

**Competitor**
```
Competitor {
  id
  brand: Brand                    # OUR brand this competitor is tracked against
  product: Product | null         # specific product or brand-level competitor

  # Competitor info
  competitorASIN: string
  competitorBrand: string
  competitorProductName: string

  # Tracked metrics
  price: Money | null
  pricePerUnit: Money | null      # e.g., price per ounce
  starRating: number | null
  reviewCount: number | null
  estimatedBSR: number | null
  fulfillment: "FBA" | "FBM" | null
  hasAPlusContent: boolean | null
  mainImageQuality: "high" | "medium" | "low" | null

  # Analysis
  strengths: [string]
  weaknesses: [string]
  threatLevel: "high" | "medium" | "low"
  lastResearched: Date

  # SQP positioning
  sharedKeywords: [{
    query: string
    theirClickShare: Percentage
    ourClickShare: Percentage
    theirConversionShare: Percentage
    ourConversionShare: Percentage
  }] | null
}
```

#### Knowledge & Memory Entities

**BrandMemory**
```
BrandMemory {
  id
  brand: Brand
  lastUpdated: DateTime
  logsSinceUpdate: number         # triggers update at 4+

  currentState: {
    focus: string                 # current priority
    challenges: [string]          # active issues
    openItems: [ActionItem]       # pending work
  }
  keyDecisions: [Decision]        # recent important decisions
  patterns: [{                    # observed patterns
    description: string
    evidence: string
    implication: string
  }]
  goals: [Goal]                   # current goals with progress
  flags: [{                       # concerns and warnings
    type: "risk" | "opportunity" | "blocker"
    description: string
    raisedDate: Date
    resolvedDate: Date | null
  }]
  importantReferences: {
    campaignNames: [string]
    keyASINs: [string]
    keyKeywords: [string]
    clientPreferences: [string]   # "client prefers conservative pricing"
  }
}
```

**KnowledgeDocument**
```
KnowledgeDocument {
  id
  title: string
  source: "training" | "call" | "lesson" | "research" | "sop"
  date: Date
  tags: [string]
  applicableTo: [string]          # categories, scenarios, brands

  # Content
  frameworks: [{
    name: string
    description: string
    steps: [string]
    applicableWhen: string
  }]
  keyInsights: [string]
  questionsToAsk: [string]
  redFlags: [string]
  commonMistakes: [string]

  # File reference
  filePath: string
}
```

#### Prediction Engine Entities

**ObservationRecord** (see Section 3.10 Phase 1)
```
ObservationRecord {
  id
  action: Action
  actionDate: Date
  contextSnapshot: { ... }        # metrics, product, competitive, temporal context
  outcomes: [MeasuredOutcome]
  controlData: ControlGroupAnalysis | null
}
```

**ActionPattern** (see Section 3.10 Phase 3)
```
ActionPattern {
  id
  actionType: ActionType
  actionMagnitudeRange: { min, max } | null
  dimensionalLevel: string
  conditions: [{ field, operator, value }]
  outcomes: [{
    metric, direction, avgChangePct, successRate,
    avgTimeToEffect, sampleSize, confidenceInterval
  }]
  totalObservations: number
  avgAttributionScore: Percentage
}
```

**Prediction** (see Section 3.10 Phase 4)
```
Prediction {
  id
  proposedAction: { type, target, magnitude, dimensionalLevel }
  currentContext: { ... }
  matchingPatterns: [{ pattern, matchScore, weight }]
  predictions: [{ metric, expectedDirection, expectedChangePct, probability, timeToEffect }]
  risks: [{ description, probability, severity, mitigation }]
  monitoringPlan: { primaryMetric, checkAfterDays, successThreshold, failureThreshold }
}
```

**Recommendation** (see Section 3.10 Phase 5)
```
Recommendation {
  id
  goal: Goal
  availableActions: [{ action, prediction, impactOnGoal, effort, risk }]
  plan: [{ priority, action, target, reasoning, expectedImpact }]
  projection: { expectedGoalValue, expectedGoalDate, milestones }
  riskAnalysis: { bestCase, expectedCase, worstCase, contingencyActions }
}
```

#### Onboarding Entities

**OnboardingProcess**
```
OnboardingProcess {
  id
  subject: Brand | Product
  type: "brand" | "product"
  status: "not_started" | "discovery" | "analysis" | "planning" | "execution" | "complete"
  startDate: Date
  completionDate: Date | null
  assignee: AccountManager

  phases: [OnboardingPhase]
  currentPhase: OnboardingPhase
}
```

**OnboardingPhase**
```
OnboardingPhase {
  id
  process: OnboardingProcess
  name: "discovery" | "analysis" | "planning" | "execution"
  status: "not_started" | "in_progress" | "complete" | "skipped"
  checklistItems: [ChecklistItem]
  startDate: Date | null
  completionDate: Date | null
}
```

**ChecklistItem**
```
ChecklistItem {
  id
  phase: OnboardingPhase
  description: string
  status: "pending" | "in_progress" | "done" | "skipped"
  assignee: AccountManager | null
  dueDate: Date | null
  notes: string | null
}
```

---

## Part 5: Domain Events

Domain events represent things that happen in the system that other parts need to react to.

### Event Catalog

| Event | Trigger | Reactions |
|-------|---------|-----------|
| `BrandOnboarded` | Onboarding process completes | Create initial strategy, set up alerts, schedule first report |
| `ProductOnboarded` | Product onboarding completes | Add to advertising strategy, set product goals |
| `ActivityLogged` | AM creates a log entry | Extract actions and decisions, increment memory counter, check for alerts |
| `ActionExecuted` | AM records completing an action | Set measurement timer, update action status |
| `OutcomeMeasured` | Measurement period completes for an action | Update action with results, validate/invalidate decision |
| `MemoryUpdateDue` | 4+ logs since last memory update | Trigger memory agent to summarize and update |
| `GoalStatusChanged` | Goal moves to at_risk or off_track | Create alert, suggest corrective actions |
| `AlertTriggered` | Metric crosses threshold | Notify AM, suggest diagnostic steps, create action item |
| `AlertResolved` | Metric returns to acceptable range | Close alert, log resolution |
| `DecisionMade` | AM records a strategic decision | Create action items, set review date |
| `DecisionReviewDue` | Review date reached for a decision | Prompt AM to evaluate outcome |
| `ReportGenerated` | Analysis report created | Suggest client update, extract action items |
| `ClientUpdateSent` | Client communication sent | Log the communication, track requests |
| `ClientRequestReceived` | Client makes a request | Create action item, assign to AM |
| `CompetitorDetected` | New competitor identified | Create competitor record, trigger research |
| `StockoutRisk` | Inventory days < threshold | Critical alert, suggest restock action |
| `BuyBoxLost` | Buy Box % drops below threshold | Critical alert, trigger diagnostic |
| `RankDrop` | BSR or organic rank significant decline | Warning alert, trigger analysis |
| `StrategyPeriodEnd` | Strategy period expires | Prompt review, suggest new strategy |
| `SQPShareDrop` | Impression/Click/Conversion share declines significantly | Warning alert, identify competitor gains |
| `FlywheelHealthChange` | TACoS trend reverses (improving → worsening or vice versa) | Strategy review, flag in memory |

---

## Part 6: Bounded Contexts

### Context Map

```
┌─────────────────────────────────────────────────────────────────┐
│                        CORE DOMAIN                              │
│  (What makes this product unique)                               │
│                                                                 │
│  ┌─────────────────┐  ┌─────────────────┐  ┌────────────────┐  │
│  │ Activity         │  │ Decision &      │  │ Performance    │  │
│  │ Tracking         │  │ Strategy        │  │ Intelligence   │  │
│  │                  │  │                 │  │                │  │
│  │ LogEntry         │  │ Decision        │  │ MetricsSnapshot│  │
│  │ Action           │  │ Strategy        │  │ Alert          │  │
│  │ MeasuredOutcome  │  │ Goal            │  │ Report         │  │
│  │ ActionItem       │  │ MetricImpact    │  │ Diagnostic     │  │
│  └────────┬─────────┘  └────────┬────────┘  └───────┬────────┘  │
│           │                     │                    │           │
│           └─────────────────────┼────────────────────┘           │
│                                 │                                │
│                    ┌────────────▼───────────┐                    │
│                    │    Brand Memory &      │                    │
│                    │    Context Engine       │                    │
│                    │                         │                    │
│                    │    BrandMemory          │                    │
│                    │    KnowledgeDocument    │                    │
│                    │    Pattern Recognition  │                    │
│                    └─────────────────────────┘                    │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                     SUPPORTING DOMAIN                           │
│  (Amazon-specific operations we model but don't reinvent)       │
│                                                                 │
│  ┌─────────────────┐  ┌─────────────────┐  ┌────────────────┐  │
│  │ Product Catalog  │  │ Advertising     │  │ Competitive    │  │
│  │                  │  │                 │  │ Intelligence   │  │
│  │ Product          │  │ Portfolio       │  │                │  │
│  │ Variant          │  │ Campaign        │  │ Competitor     │  │
│  │ Listing          │  │ AdGroup         │  │ MarketPosition │  │
│  │ SKU              │  │ Target          │  │ SQP Analysis   │  │
│  └──────────────────┘  └─────────────────┘  └────────────────┘  │
│                                                                 │
│  ┌─────────────────┐  ┌─────────────────┐                       │
│  │ Client Comms     │  │ Onboarding      │                       │
│  │                  │  │                 │                       │
│  │ ClientUpdate     │  │ OnboardingProc  │                       │
│  │ ClientRequest    │  │ Phase           │                       │
│  │ Report           │  │ ChecklistItem   │                       │
│  └──────────────────┘  └─────────────────┘                       │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                      GENERIC DOMAIN                             │
│  (Standard infrastructure, use existing solutions)              │
│                                                                 │
│  ┌─────────────────┐  ┌─────────────────┐  ┌────────────────┐  │
│  │ Organization     │  │ Data Ingestion  │  │ Version        │  │
│  │                  │  │                 │  │ Control        │  │
│  │ Agency           │  │ Report Import   │  │                │  │
│  │ Team             │  │ MCP Server      │  │ Git            │  │
│  │ AccountManager   │  │ API Connections │  │ File System    │  │
│  │ BrandAssignment  │  │                 │  │                │  │
│  └──────────────────┘  └─────────────────┘  └────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Part 7: The AM Lifecycle (Process Model)

This is how all entities flow together through the AM's work.

### Phase 1: Onboard

```
New Brand → OnboardingProcess (brand)
  ├── Discovery Phase
  │     ├── Gather brand info → Brand entity
  │     ├── Gather products → Product + Variant entities
  │     ├── Identify competitors → Competitor entities
  │     └── Understand goals → Goal entities
  │
  ├── Analysis Phase
  │     ├── Pull Business Reports → MetricsSnapshot
  │     ├── Analyze SQP → Market position + share data
  │     ├── Research competitors → Competitor details
  │     ├── Audit listings → Listing quality scores
  │     └── Review advertising → Campaign audit
  │
  ├── Planning Phase
  │     ├── Create Strategy → Strategy entity
  │     ├── Set Goals → Goal entities with targets
  │     ├── Define Actions → ActionItem entities
  │     └── Set up monitoring → Alert rules
  │
  └── Execution Phase
        ├── Implement changes → Action entities
        ├── Log activity → LogEntry entities
        └── Establish cadence → ClientUpdate schedule
```

### Phase 2: Weekly Management Cycle

```
Monday:
  ├── Check Alerts → Alert entities (critical first)
  ├── Pull weekly data → MetricsSnapshot
  ├── Review SQP changes → SQP share analysis
  └── Identify issues → ActionItem entities (new)

Tuesday-Thursday:
  ├── Execute actions → Action entities
  │     ├── Bid adjustments (Search Term Report review)
  │     ├── Keyword management (negate, harvest, expand)
  │     ├── Listing updates
  │     └── Pricing decisions
  ├── Log activity → LogEntry entities
  └── Track decisions → Decision entities

Friday:
  ├── Prepare client update → ClientUpdate entity
  │     ├── Summarize metrics → MetricsSnapshot
  │     ├── Highlight wins → Insights
  │     ├── Flag concerns → Concerns
  │     └── Propose next steps → ActionItems
  ├── Update memory (if 4+ logs) → BrandMemory
  └── Plan next week → ActionItem priorities
```

### Phase 3: Decision → Action → Measurement Loop

```
Observation (from data or client)
  │
  ├── Log it → LogEntry (type: observation)
  │
  ├── Diagnose → Apply Diagnostic Model (Section 3.4)
  │              Use KnowledgeDocuments for frameworks
  │              Cross-reference Amazon reports (Section 3.8)
  │
  ├── Decide → Decision entity
  │     ├── options considered
  │     ├── chosen approach + rationale
  │     ├── expected outcome (MetricImpact)
  │     └── review date set
  │
  ├── Plan → ActionItem entities
  │     ├── specific changes to make
  │     ├── assigned to AM
  │     └── due dates
  │
  ├── Execute → Action entities (from ActionItems)
  │     ├── what was done
  │     ├── before/after values
  │     └── expected impacts attached
  │
  ├── Monitor → wait for measurement period (Section 3.6)
  │     ├── MetricsSnapshot comparison
  │     └── Alert if threshold breached
  │
  └── Measure → MeasuredOutcome
        ├── compare before/after
        ├── attribute to action
        ├── validate/revise decision
        └── log learnings → BrandMemory patterns
```

### Phase 4: Client Communication

```
Reporting Cadence:
  │
  ├── Weekly Email → ClientUpdate (type: weekly_email)
  │     ├── 3-5 bullet points
  │     ├── Key metrics table
  │     ├── Actions taken this week
  │     └── Next week plan
  │
  ├── Monthly Report → ClientUpdate (type: monthly_report) + Report
  │     ├── Full metrics dashboard
  │     ├── Trend analysis (MoM, YoY)
  │     ├── Goal progress (Goal entities)
  │     ├── SQP share analysis (market position)
  │     ├── Strategic recommendations
  │     └── Competitor landscape update
  │
  └── Quarterly Review → ClientUpdate (type: quarterly_review) + Strategy
        ├── Strategy performance review
        ├── Goal achievement assessment
        ├── Market changes + SQP trends
        ├── Next quarter strategy proposal
        └── Budget allocation recommendation
```

---

## Part 8: Listing Optimization Domain

The listing is a **conversion machine**. Its job is to convert traffic into purchases. This section models the entities, relationships, and processes unique to listing optimization.

### 8.1 The Conversion Funnel on the Detail Page

```
Search Results Page (pre-click)          Detail Page (post-click)
┌─────────────────────────┐              ┌─────────────────────────────┐
│ What drives the CLICK:  │              │ What drives the PURCHASE:   │
│                         │              │                             │
│ • Main image            │───click───▶  │ ABOVE THE FOLD (no scroll)  │
│ • Title (first ~80 ch)  │              │ • All images + video        │
│ • Price                 │              │ • Full title                │
│ • Star rating           │              │ • Price + deals/coupons     │
│ • Review count          │              │ • Prime badge               │
│ • Prime badge           │              │ • Buy Box                   │
│ • Coupon badge          │              │ • Star rating + review count│
│ • Delivery speed        │              │ • Bullets 1-2 (desktop)     │
│                         │              │                             │
│ METRIC: CTR             │              │ BELOW THE FOLD (scrolling)  │
└─────────────────────────┘              │ • Remaining bullets         │
                                         │ • A+ Content                │
                                         │ • Reviews section           │
                                         │ • Q&A section               │
                                         │ • Related products          │
                                         │                             │
                                         │ METRIC: CVR                 │
                                         └─────────────────────────────┘
```

### 8.2 Customer Segment Model

Different customers arrive with different needs. The listing must serve ALL of them.

```
CustomerSegment {
  id
  name: string                    # e.g., "Health-Conscious Parent"
  description: string
  product: Product                # which product this segment is for

  # Demographics
  ageRange: string | null         # e.g., "25-40"
  gender: string | null
  incomeLevel: string | null

  # Purchase behavior
  intentType: "branded" | "category" | "problem_solving" | "comparison" | "impulse"
  pricesSensitivity: "high" | "medium" | "low"
  researchDepth: "quick_decider" | "moderate" | "deep_researcher"

  # What they search for
  primaryKeywords: [string]       # the searches that bring them here
  searchIntent: string            # what they're really looking for

  # What they need to see to convert
  decisionDrivers: [{
    factor: string                # e.g., "organic ingredients", "durability", "fast shipping"
    importance: "critical" | "important" | "nice_to_have"
    whereToAddress: "title" | "main_image" | "bullet" | "a_plus" | "images" | "reviews"
  }]

  # Objections to overcome
  objections: [{
    concern: string               # e.g., "Is it safe for kids?"
    whereToAddress: string        # which listing element answers this
    howToAddress: string          # specific content recommendation
  }]

  # What they DON'T search for but need for conversion
  unstatedNeeds: [{
    need: string                  # e.g., "BPA-free", "dishwasher safe"
    importance: "critical" | "important"
    # These are the things that close the deal but aren't in the search query
  }]

  # Estimated segment size
  estimatedShareOfTraffic: Percentage
}
```

**Example:**

```
Product: Maurice's BBQ Sauce (B07P15CB6X)

Segment 1: "BBQ Enthusiast"
  Intent: category search ("bbq sauce", "mustard bbq sauce")
  Decision drivers: flavor uniqueness, authenticity, ingredients
  Objections: "Is it too vinegary?", "How does it compare to Sweet Baby Ray's?"
  Unstated needs: shelf life, bottle size for value

Segment 2: "South Carolina Nostalgic"
  Intent: branded/regional ("maurice's bbq", "south carolina bbq sauce")
  Decision drivers: restaurant heritage, authentic regional recipe
  Objections: "Is it the same as in the restaurant?"
  Unstated needs: emotional connection to SC culture

Segment 3: "Gift Buyer"
  Intent: problem-solving ("bbq sauce gift set", "bbq lover gift")
  Decision drivers: packaging quality, variety, giftability
  Objections: "Will it arrive in nice packaging?"
  Unstated needs: gift-worthiness, presentation

Segment 4: "Health-Conscious Cook"
  Intent: category + qualifier ("low sugar bbq sauce", "gluten free bbq sauce")
  Decision drivers: ingredient list, nutritional info, no artificial additives
  Objections: "How much sugar per serving?"
  Unstated needs: dietary compatibility, clean label
```

### 8.3 Keyword Intent Model

Every keyword has an **intent** that maps to how the listing should serve that searcher.

```
KeywordIntent {
  id
  keyword: string
  product: Product

  # Classification
  intentType: "branded" | "non_branded_category" | "non_branded_problem"
             | "comparison" | "long_tail_specific" | "competitor_branded"
  brandedness: "branded" | "non_branded" | "competitor_branded"
  funnelStage: "awareness" | "consideration" | "decision" | "purchase"
  searchVolume: "high" | "medium" | "low"
  conversionLikelihood: "high" | "medium" | "low"

  # Which customer segment uses this keyword
  primarySegment: CustomerSegment

  # Where this keyword should appear in the listing
  listingPlacement: {
    title: boolean                # should it be in the title?
    titlePosition: "front" | "middle" | "end" | null
    bullets: boolean
    bulletPosition: number | null # which bullet (1-5)
    backend: boolean
    aPlusContent: boolean
  }

  # How the listing should address this keyword's intent
  contentStrategy: string         # e.g., "Lead with flavor uniqueness and restaurant heritage"

  # Advertising strategy for this keyword
  adStrategy: {
    matchTypes: ["exact", "phrase", "broad"]
    targetACOS: Percentage
    bidStrategy: string           # e.g., "Aggressive exact match, defensive broad"
    campaignGoal: "ranking" | "profit" | "defense" | "conquest" | "discovery"
  }
}
```

**Keyword Intent Classification Framework:**

```
┌─────────────────────────┬──────────────┬───────────────┬──────────────────────┐
│ Intent Type             │ Example      │ Funnel Stage  │ Listing Priority     │
├─────────────────────────┼──────────────┼───────────────┼──────────────────────┤
│ Branded                 │ "maurice's   │ Decision      │ Title (brand first), │
│                         │  bbq sauce"  │               │ Brand Store, SB ads  │
├─────────────────────────┼──────────────┼───────────────┼──────────────────────┤
│ Non-branded Category    │ "bbq sauce"  │ Consideration │ Title, Bullets 1-2,  │
│                         │              │               │ Main image, Backend  │
├─────────────────────────┼──────────────┼───────────────┼──────────────────────┤
│ Non-branded Problem     │ "sauce for   │ Awareness/    │ Bullets 3-5, A+,     │
│                         │  pulled pork"│ Consideration │ Backend, Images      │
├─────────────────────────┼──────────────┼───────────────┼──────────────────────┤
│ Comparison              │ "bbq sauce   │ Consideration │ A+ comparison chart, │
│                         │  vs ketchup" │               │ Infographics         │
├─────────────────────────┼──────────────┼───────────────┼──────────────────────┤
│ Long-tail Specific      │ "mustard     │ Decision      │ Backend keywords,    │
│                         │  based bbq   │               │ Bullet that matches  │
│                         │  sauce SC"   │               │ exact use case       │
├─────────────────────────┼──────────────┼───────────────┼──────────────────────┤
│ Competitor Branded      │ "sweet baby  │ Consideration │ Product targeting    │
│                         │  ray's       │               │ ads, comparison      │
│                         │  alternative"│               │ in A+ content        │
└─────────────────────────┴──────────────┴───────────────┴──────────────────────┘
```

### 8.4 Listing Element Model

Each element of the listing is a first-class entity with its own optimization attributes.

```
ListingElement {
  id
  listing: Listing
  type: "title" | "main_image" | "image_2" | "image_3" | "image_4"
       | "image_5" | "image_6" | "image_7" | "video"
       | "bullet_1" | "bullet_2" | "bullet_3" | "bullet_4" | "bullet_5"
       | "a_plus_module_1" | ... | "a_plus_module_7"
       | "backend_keywords" | "description"

  # Content
  content: string | URL           # text content or image URL

  # Optimization metadata
  targetSegments: [CustomerSegment]  # which segments this element primarily serves
  targetKeywords: [KeywordIntent]    # which keywords this element addresses
  primaryMetricImpact: "ctr" | "cvr" | "both"

  # For images
  imageType: "product_only" | "lifestyle" | "infographic" | "comparison"
            | "dimensions" | "ingredients" | "how_to_use" | null

  # For A+ modules
  aPlusModuleType: "comparison_chart" | "four_image_text" | "tech_specs"
                  | "lifestyle_hero" | "feature_highlights" | "qa_module" | null

  # Quality assessment
  qualityScore: number | null     # 0-10 per element
  lastAuditDate: Date | null
  auditNotes: string | null

  # A/B testing
  variants: [{                    # alternative versions being tested
    variantId: string
    content: string | URL
    testStartDate: Date
    testEndDate: Date | null
    ctrResult: Percentage | null
    cvrResult: Percentage | null
    winner: boolean | null
  }] | null
}
```

### 8.5 Design Brief Model (for A+ Content and Images)

When the AM needs to brief a designer, this captures what's needed:

```
DesignBrief {
  id
  listing: Listing
  type: "a_plus_content" | "main_image" | "infographic" | "lifestyle_image" | "video"
  status: "draft" | "approved" | "in_production" | "delivered" | "live"

  # Strategic context
  targetSegments: [CustomerSegment]
  keyMessages: [{
    message: string               # e.g., "50+ years of restaurant heritage"
    priority: number              # 1 = most important
    supportingEvidence: string    # what backs this up
  }]
  keywordsToIncorporate: [string] # keywords that should appear in text overlays
  objectionsToAddress: [string]   # customer concerns to overcome visually
  competitorDifferentiators: [string]  # what makes us different

  # Visual requirements
  modules: [{                     # for A+ content
    moduleType: string            # comparison_chart, four_image_text, etc.
    purpose: string               # what this module should accomplish
    textContent: string           # copy for this module
    imageDirection: string        # what the image should show
    mobileConsiderations: string  # how it should look on mobile
  }] | null

  imageDirection: {               # for individual images
    style: "product_on_white" | "lifestyle" | "infographic" | "hybrid"
    setting: string | null        # e.g., "outdoor BBQ scene"
    mood: string | null           # e.g., "warm, family, summer"
    mustInclude: [string]         # elements that must be in the image
    mustAvoid: [string]           # things to avoid
    textOverlays: [{
      text: string
      placement: string
      emphasis: "primary" | "secondary"
    }] | null
    technicalSpecs: {
      minResolution: string
      format: "jpg" | "png"
      maxFileSize: string
      backgroundRequirement: string  # "pure white" for main image, "any" for others
    }
  } | null

  # Approval
  createdBy: AccountManager
  approvedBy: AccountManager | null
  approvedDate: Date | null
  revisionNotes: [string]
}
```

### 8.6 Listing Quality Score (LQS) Model

A structured audit framework for scoring listings:

```
ListingAudit {
  id
  listing: Listing
  auditDate: Date
  auditor: AccountManager
  overallScore: number            # 0-100

  # 10 scoring dimensions (each 0-10)
  scores: {
    titleOptimization: {
      score: number
      notes: string
      issues: [string]            # e.g., "Missing primary keyword in first 80 chars"
    }
    bulletPointQuality: {
      score: number
      notes: string
      issues: [string]
    }
    mainImageEffectiveness: {
      score: number
      notes: string
      issues: [string]
    }
    secondaryImages: {
      score: number
      notes: string
      issues: [string]            # e.g., "Only 4 of 7 image slots used"
    }
    aPlusContent: {
      score: number
      notes: string
      issues: [string]
    }
    backendKeywords: {
      score: number
      notes: string
      issues: [string]
    }
    pricingCompetitiveness: {
      score: number
      notes: string
      issues: [string]
    }
    reviewHealth: {
      score: number
      notes: string
      issues: [string]            # e.g., "Star rating 3.8, below category avg 4.2"
    }
    categoryAccuracy: {
      score: number
      notes: string
      issues: [string]
    }
    mobileExperience: {
      score: number
      notes: string
      issues: [string]            # e.g., "Title too long, truncated on mobile"
    }
  }

  # Prioritized action items from audit
  recommendations: [{
    element: string               # which listing element
    issue: string
    impact: "high" | "medium" | "low"
    effort: "low" | "medium" | "high"
    recommendation: string
    expectedImpact: MetricImpact
  }]

  # Comparison
  priorAudit: ListingAudit | null
  scoreChange: number | null      # vs prior audit
}
```

### 8.7 The "Unstated Needs" Concept

This is a key insight: **people search for one thing but need to see other things to convert.**

```
Search: "bbq sauce"
  → They typed: flavor category
  → They ALSO need to see (but didn't search for):
    • Ingredients (clean label? allergens?)
    • Size/quantity (value for money?)
    • Versatility (what can I use it on?)
    • Shipping (Prime? arrival time?)
    • Social proof (do others like it?)
    • Origin story (why is this special?)
    • Comparison (how is this different from what I use now?)

The listing must serve the STATED intent (keywords)
AND the UNSTATED needs (conversion drivers)
```

This is modeled in the `CustomerSegment.unstatedNeeds` and `CustomerSegment.decisionDrivers` fields.

---

## Part 9: Advanced Advertising Operations

This section models the tactical advertising patterns that go beyond simple bid/keyword management.

### 9.1 Keyword Graduation Pipeline

Keywords have a **lifecycle** from discovery to optimization:

```
KeywordLifecycle {
  id
  keyword: string
  product: Product
  brand: Brand

  # Current state
  currentStage: "undiscovered" | "discovered" | "testing_broad" | "testing_phrase"
               | "proven_exact" | "skag" | "graduated" | "negated" | "paused"

  # Stage history
  history: [{
    stage: string
    enteredDate: Date
    exitedDate: Date | null
    campaign: Campaign
    matchType: string
    metrics: {
      impressions: number
      clicks: number
      spend: Money
      sales: Money
      acos: Percentage
      orders: number
    }
    promotionReason: string | null  # why it was promoted/demoted
  }]

  # Graduation criteria
  graduationThresholds: {
    autoToBroad: { minSales: 2, maxACOS: Percentage }
    broadToPhrase: { minSales: 3, maxACOS: Percentage }
    phraseToExact: { minSales: 5, maxACOS: Percentage }
    exactToSKAG: { minWeeklySpend: Money, minROAS: number }
  }

  # Negation tracking
  negatedIn: [Campaign]           # campaigns where this is negated
  negationType: "exact" | "phrase" | null
}
```

**The Pipeline:**

```
┌──────────────┐   harvest    ┌──────────────┐   promote    ┌──────────────┐
│ AUTO Campaign│──────────────▶│ BROAD Match  │──────────────▶│ PHRASE Match │
│ (Discovery)  │              │ Campaign     │              │ Campaign     │
│              │              │              │              │              │
│ Low bids     │              │ Medium bids  │              │ Higher bids  │
│ All targeting│              │ Wider reach  │              │ More control │
└──────────────┘              └──────────────┘              └──────────────┘
                                                                   │
       negate in source ◄─────── negate in source ◄────────────────┘
                                                                   │ promote
                                                                   ▼
                              ┌──────────────┐   top performers ┌──────────────┐
                              │ SKAG         │◀─────────────────│ EXACT Match  │
                              │ (Single KW)  │                  │ Campaign     │
                              │              │                  │              │
                              │ Highest bids │                  │ High bids    │
                              │ Own budget   │                  │ Best control │
                              └──────────────┘                  └──────────────┘

At each stage:
  1. Keyword enters new campaign/match type
  2. Keyword is negated (exact negative) in the SOURCE campaign
  3. Bid is set higher than the prior stage
  4. Performance is monitored for graduation or demotion
```

### 9.2 Campaign Structure Strategy

```
CampaignStructure {
  id
  brand: Brand
  strategy: Strategy

  # Portfolio allocation
  portfolios: [{
    name: string
    goal: "brand_defense" | "conquest" | "product_launch" | "profit" | "discovery"
    budgetAllocationPct: Percentage
    targetACOS: Percentage
    campaigns: [Campaign]
  }]

  # Naming convention
  namingConvention: {
    format: "[Product] - [ASIN] - [Type] - [Match] - [Goal]"
    examples: [string]
  }

  # Spend distribution rules
  spendRules: [{
    condition: string             # e.g., "keyword has >$5 spend and 0 sales in 7 days"
    action: string                # e.g., "negate and add to negative list"
  }]
}
```

**Portfolio Templates:**

```
Portfolio: Brand Defense (15-25% of budget)
├── SP - Exact - Brand Terms         Target ACOS: 15-25%
├── SP - Phrase - Brand Variations   Target ACOS: 20-30%
├── SB - Brand Keywords              Target ACOS: 20-30%
└── SD - Retargeting Branded         Target ACOS: 25-35%

Portfolio: Conquest (20-30% of budget)
├── SP - Exact - Competitor Terms    Target ACOS: 35-50%
├── SP - ASIN - Competitor Products  Target ACOS: 35-50%
└── SD - Competitor ASIN Targeting   Target ACOS: 40-60%

Portfolio: Profit Harvesting (30-40% of budget)
├── SP - Exact - Proven Winners      Target ACOS: 15-30%
├── SP - SKAGs - Top 10 Keywords     Target ACOS: 10-25%
└── SP - Phrase - Established KWs    Target ACOS: 20-35%

Portfolio: Discovery (10-20% of budget)
├── SP - Auto - All Targeting        Target ACOS: 50-80%
├── SP - Broad - Category Terms      Target ACOS: 40-70%
└── SP - PAT - Product Attributes    Target ACOS: 40-70%

Portfolio: Product Launch (aggressive, temporary)
├── SP - Auto - All Targeting        ACOS: 60-100% acceptable
├── SP - Broad - Core Keywords       ACOS: 50-80% acceptable
├── SP - ASIN - Competitor Top       ACOS: 50-80% acceptable
├── SB - Top 5 Keywords              ACOS: 40-70%
└── SD - Category Targeting          ACOS: 50-80%
```

### 9.3 Spend Distribution Monitoring

Amazon doesn't spend evenly. This workflow detects and fixes imbalances:

```
SpendDistributionAudit {
  id
  campaign: Campaign
  auditDate: Date
  period: DateRange

  # Per-target analysis
  targets: [{
    target: Target                # keyword or product target
    impressions: number
    clicks: number
    spend: Money
    sales: Money
    budgetSharePct: Percentage    # what % of campaign spend went here
    impressionSharePct: Percentage

    # Status
    status: "healthy" | "under_served" | "over_served" | "zero_spend" | "wasted"

    # Diagnosis
    diagnosis: string | null
    # "zero_spend": Amazon isn't spending on this target at all
    # "under_served": Getting <5% of spend despite high relevance
    # "over_served": Consuming >40% of budget, may need isolation
    # "wasted": Spending but 0 sales, needs negation
  }]

  # Recommended actions
  recommendations: [{
    target: Target
    action: "isolate_to_new_campaign" | "increase_bid" | "negate" | "monitor"
    reason: string
    priority: "high" | "medium" | "low"
  }]
}
```

### 9.4 Negative Keyword Strategy

```
NegativeKeywordAction {
  id
  brand: Brand
  campaign: Campaign | null       # null = account-level
  adGroup: AdGroup | null

  keyword: string
  negativeType: "exact" | "phrase"
  targetType: "keyword" | "product_asin"

  # Source
  reason: "zero_sales" | "high_acos" | "irrelevant" | "cannibalization" | "graduation"
  sourceData: {
    clicks: number
    spend: Money
    sales: Money
    acos: Percentage | null
    period: DateRange
  }

  addedDate: Date
  addedBy: AccountManager
}
```

**Decision Framework:**

```
Search Term Analysis:
│
├── Clicks > 5, Sales = 0?
│   └── YES → Add as Exact Negative (immediate)
│
├── ACOS > 2x target, Clicks > 10?
│   └── YES → Add as Exact Negative (efficiency)
│
├── Irrelevant to product?
│   └── YES → Add as Phrase Negative (blocks variations too)
│
├── Keyword graduated to next match type?
│   └── YES → Add as Exact Negative in source campaign (prevent cannibalization)
│
└── Product ASIN target with 0 sales, $10+ spend?
    └── YES → Add as Negative Product Target
```

### 9.5 Bidding Strategy Selection Model

```
BiddingStrategyDecision {
  campaign: Campaign

  # Decision inputs
  campaignMaturity: "new" | "learning" | "mature"
  dataPoints: number              # how many clicks/conversions
  currentACOS: Percentage
  targetACOS: Percentage
  acosGap: Percentage             # how far from target
  productMargin: Percentage
  priority: "visibility" | "efficiency" | "balanced"

  # Selected strategy
  strategy: "dynamic_up_and_down" | "dynamic_down_only" | "fixed"

  # Placement modifiers
  topOfSearchModifier: Percentage | null      # e.g., +50%
  productPagesModifier: Percentage | null
  restOfSearchModifier: Percentage | null
}
```

**Decision Tree:**

```
Bidding Strategy Selection:
│
├── New campaign / product launch?
│   ├── Priority: max visibility → FIXED (high bids)
│   └── Priority: cost control → DYNAMIC DOWN ONLY
│
├── Mature campaign, ACOS well below target?
│   └── DYNAMIC UP AND DOWN (let Amazon bid more on high-intent)
│
├── Mature campaign, ACOS at or near target?
│   └── DYNAMIC DOWN ONLY (protect efficiency)
│
├── ACOS above target?
│   └── DYNAMIC DOWN ONLY + reduce bids
│
└── Brand defense campaign?
    └── DYNAMIC UP AND DOWN (must win branded terms)

Placement Modifiers:
│
├── Top of Search has best CVR?
│   └── Set TOS modifier +30-100%
│
├── Product Pages converting well?
│   └── Set Product Pages modifier +20-50%
│
└── Discovery/research campaign?
    └── No modifiers (let data accumulate first)
```

### 9.6 Campaign Operations Workflow

The weekly advertising operations cycle:

```
AdOperationsWeeklyCycle {

  monday_harvesting: {
    # Pull 30-day Search Term Reports
    actions: [
      "Filter: sales > 0, ACOS < target → HARVEST (promote to next match type)",
      "Filter: clicks > 5, sales = 0 → NEGATE (add exact negative)",
      "Filter: spend > $10, ACOS > 2x target → REVIEW (negate or reduce bid)",
      "For each harvested term: add to next match type + negate in source"
    ]
  }

  tuesday_optimization: {
    # Bid and budget adjustments
    actions: [
      "Review top 20 keywords by spend → adjust bids to hit target ACOS",
      "Check budget utilization → increase budget on capped profitable campaigns",
      "Review placement performance → adjust TOS/PP modifiers",
      "Check spend distribution → isolate under-served high-value targets"
    ]
  }

  wednesday_analysis: {
    # Performance review
    actions: [
      "Portfolio-level ACOS/ROAS review vs targets",
      "Identify campaigns trending above target ACOS",
      "SQP impression share check for top keywords",
      "Competitor ASIN targeting performance review"
    ]
  }

  thursday_expansion: {
    # Growth and testing
    actions: [
      "Launch new keyword tests from SQP/research",
      "Add new product targets based on Market Basket data",
      "Review auto campaign for new discovery opportunities",
      "Test new ad copy for Sponsored Brands"
    ]
  }

  friday_reporting: {
    # Wrap-up
    actions: [
      "Update campaign performance tracker",
      "Flag any critical issues for Monday",
      "Prepare weekly ad performance summary for AM",
      "Note any strategic decisions needed for next week"
    ]
  }
}
```

---

## Part 10: Client & Agency Management

A **Client** is the paying entity. A client can own multiple brands. This section models the relationship between the agency, its clients, and the communication rhythms that keep accounts healthy.

### 10.1 Core Entities

```
Client {
  id
  name: string                    # "Maurice's Gourmet BBQ LLC"
  contactName: string             # Primary point of contact
  contactEmail: string
  contactPhone: string | null

  # Relationship
  brands: [Brand]                 # one client → many brands
  primaryAM: AccountManager       # main relationship owner
  supportAMs: [AccountManager]    # additional AMs who contribute
  startDate: Date                 # when they became a client
  contractType: "monthly" | "quarterly" | "annual" | "project"

  # Communication preferences
  preferredChannel: "email" | "slack" | "phone" | "zoom"
  timezone: string                # for scheduling
  communicationCadence: CommunicationCadence

  # Status
  status: "prospect" | "onboarding" | "active" | "paused" | "churned"
  healthIndicator: "green" | "yellow" | "red"  # simple RAG status
  # green = on track, responsive, growing
  # yellow = some concerns (missed calls, flat performance, tension)
  # red = at risk (unresponsive, declining, unhappy)
}
```

### 10.2 Communication Cadence

```
CommunicationCadence {
  id
  client: Client

  # Recurring touchpoints
  touchpoints: [{
    type: "weekly_update" | "monthly_review" | "quarterly_strategy" | "ad_hoc"
    frequency: "weekly" | "biweekly" | "monthly" | "quarterly"
    dayOfWeek: string | null      # "Monday", "Thursday", etc.
    format: "email" | "call" | "video" | "slack_message" | "report"
    owner: AccountManager

    # What's covered
    agenda: [string]              # e.g., ["Performance summary", "Action items", "Questions"]
    includesReport: boolean       # does this touchpoint include a data report?
    reportType: string | null     # e.g., "weekly_performance", "monthly_deep_dive"
  }]

  # Tracking
  lastTouchpoint: Date
  nextScheduled: Date
  missedCount: number             # consecutive missed touchpoints (triggers yellow/red)
}
```

### 10.3 Communication Record

```
CommunicationRecord {
  id
  client: Client
  brand: Brand | null             # null = account-level communication
  date: Date
  type: "weekly_update" | "monthly_review" | "quarterly_strategy" | "ad_hoc" | "client_request"
  format: "email" | "call" | "video" | "slack_message" | "report"

  # Content
  summary: string                 # what was discussed
  sentBy: AccountManager

  # Outcomes
  clientRequests: [{
    request: string               # what the client asked for
    priority: "high" | "medium" | "low"
    linkedAction: ActionItem | null  # did this become an action item?
    status: "pending" | "in_progress" | "completed" | "declined"
    declinedReason: string | null    # if we declined, why?
  }]

  # Commitments
  amCommitments: [{
    commitment: string            # what we promised
    dueDate: Date | null
    completed: boolean
  }]

  # Follow-up
  nextSteps: [string]
  nextTouchpointDate: Date | null
}
```

### 10.4 Client ↔ Brand ↔ AM Relationship

```
                    ┌──────────┐
                    │  Client  │
                    │ (paying  │
                    │  entity) │
                    └────┬─────┘
                         │ owns 1..n
              ┌──────────┼──────────┐
              ▼          ▼          ▼
         ┌─────────┐ ┌─────────┐ ┌─────────┐
         │ Brand A │ │ Brand B │ │ Brand C │
         └────┬────┘ └────┬────┘ └────┬────┘
              │           │           │
              ▼           ▼           ▼
         BrandAssignment (primary AM + support AMs)
              │
              ▼
         AccountManager
         - manages multiple brands across multiple clients
         - one primary per brand, can support others
```

**Rule:** Communication cadence is set at the **Client** level (not per brand), because the client is who we meet with. Brand-level updates flow into client-level reports.

---

## Part 11: Financial & Pricing Domain

This section models the financial layer: pricing decisions, margin tracking, budget allocation, and — critically — the **statistical minimums** that govern when you have enough data to act.

### 11.1 Pricing Strategy

```
PricingStrategy {
  id
  product: Product
  brand: Brand

  # Current pricing
  currentPrice: Money
  currentMAP: Money | null        # Minimum Advertised Price (if applicable)
  cogs: Money                     # Cost of Goods Sold per unit
  amazonFees: Money               # referral fee + FBA fee per unit
  estimatedAdCostPerUnit: Money   # avg ad spend to sell one unit

  # Margins
  grossMargin: Percentage         # (price - cogs - amazonFees) / price
  netMargin: Percentage           # (price - cogs - amazonFees - adCost) / price
  breakEvenACOS: Percentage       # the ACOS at which you make $0 profit
  # breakEvenACOS = grossMargin (before ad costs)
  # e.g., price $30, COGS $8, fees $7 → gross margin = $15/$30 = 50% → break-even ACOS = 50%

  targetACOS: Percentage          # must be BELOW breakEvenACOS for profit
  targetMargin: Percentage        # desired net margin after all costs

  # Competitive positioning
  pricePosition: "premium" | "mid_range" | "value" | "economy"
  competitorPriceRange: { min: Money, max: Money, median: Money }
  priceElasticity: "elastic" | "moderate" | "inelastic" | "unknown"
  # elastic = small price change → big demand change
  # inelastic = price change → little demand change (brand loyalty, niche)

  # Price change rules
  priceFloor: Money               # never go below this
  priceCeiling: Money             # never go above this
  rules: [{
    trigger: string               # e.g., "competitor drops below $25"
    action: string                # e.g., "match if margin stays above 20%"
    constraint: string | null     # e.g., "never below MAP"
  }]
}
```

**Break-Even ACOS Calculation:**

```
Break-Even ACOS = (Selling Price - COGS - Amazon Fees) / Selling Price × 100

Example:
  Selling Price:     $30.00
  COGS:              $ 8.00
  Referral Fee (15%):$ 4.50
  FBA Fee:           $ 5.00
  ─────────────────────────
  Gross Profit:      $12.50
  Break-Even ACOS:   $12.50 / $30.00 = 41.7%

  If Target Net Margin = 15%:
    Target ACOS = 41.7% - 15% = 26.7%
```

### 11.2 Budget Allocation

```
BudgetAllocation {
  id
  brand: Brand
  period: DateRange               # monthly allocation

  # Total budget
  totalAdBudget: Money            # total monthly ad spend
  dailyBudgetTarget: Money        # totalAdBudget / days in month

  # Allocation by portfolio goal
  portfolioAllocations: [{
    portfolio: Portfolio
    goal: "brand_defense" | "conquest" | "profit" | "discovery" | "product_launch"
    allocationPct: Percentage
    amount: Money
    targetACOS: Percentage

    # Actual vs planned
    actualSpend: Money | null
    pacing: "on_track" | "under_spending" | "over_spending" | null
  }]

  # Allocation by product
  productAllocations: [{
    product: Product
    allocationPct: Percentage
    amount: Money
    rationale: string             # why this product gets this share
    # e.g., "highest margin", "launch phase", "defending #1 BSR"
  }]

  # Budget tier (determines strategy complexity)
  budgetTier: "micro" | "small" | "medium" | "large"
  # micro:  < $500/mo   → single product, exact match only
  # small:  $500-2000/mo → 2-3 products, limited campaign types
  # medium: $2000-5000/mo → full framework, 3-5 products
  # large:  > $5000/mo   → all campaign types, all products
}
```

### 11.3 Budget Constraint Strategy

When budget is limited, you can't run the ideal campaign structure. This model defines what to do at each budget tier.

```
BudgetConstraintStrategy {
  tier: "micro" | "small" | "medium" | "large"

  # What you CAN run at this tier
  maxProducts: number             # how many products to advertise
  maxCampaigns: number            # total campaigns across all products
  allowedCampaignTypes: [string]  # which campaign types are viable
  matchTypeStrategy: string       # which match types to use

  # Budget split
  profitPct: Percentage           # % to proven winners
  discoveryPct: Percentage        # % to finding new terms
  defensePct: Percentage          # % to brand defense
}
```

**Tier Definitions:**

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│ MICRO BUDGET (< $500/month, < $17/day)                                        │
├─────────────────────────────────────────────────────────────────────────────────┤
│ Products:      1 (highest margin or most strategic)                            │
│ Campaigns:     2-3 max                                                        │
│                  1× Auto (discovery, low bids)                                │
│                  1× Exact Match (proven winners only)                          │
│                  1× Brand Defense (if brand has search volume)                 │
│ Match Types:   Exact only for manual (highest control, best CVR)              │
│ Split:         60% Profit / 30% Discovery / 10% Defense                       │
│ Keywords:      10-15 max across all campaigns                                 │
│ Bid Strategy:  Start at 50% of Amazon suggested bid                           │
│ Key Rule:      Aggressive negative keywords — every dollar counts             │
│ Decision Pace: Bi-weekly (data accumulates slowly)                            │
├─────────────────────────────────────────────────────────────────────────────────┤
│ SMALL BUDGET ($500-2,000/month, $17-67/day)                                   │
├─────────────────────────────────────────────────────────────────────────────────┤
│ Products:      2-3                                                            │
│ Campaigns:     4-8                                                            │
│                  1× Auto per product                                          │
│                  1× Exact per product                                          │
│                  1× Brand Defense (shared)                                     │
│ Match Types:   Exact + Phrase (add phrase for proven terms)                    │
│ Split:         50% Profit / 30% Discovery / 20% Defense                       │
│ Keywords:      15-25 per campaign                                             │
│ Bid Strategy:  Moderate, weekly adjustments                                   │
│ Key Rule:      Graduate keywords from auto → exact weekly                     │
│ Decision Pace: Weekly                                                         │
├─────────────────────────────────────────────────────────────────────────────────┤
│ MEDIUM BUDGET ($2,000-5,000/month, $67-167/day)                               │
├─────────────────────────────────────────────────────────────────────────────────┤
│ Products:      3-5                                                            │
│ Campaigns:     8-20                                                           │
│ Types:         Full framework (Auto, Broad, Phrase, Exact, SKAG for top KWs)  │
│ + SD:          Can add Sponsored Display for retargeting                      │
│ + SB:          Can add Sponsored Brands for top keywords                      │
│ Split:         45% Profit / 25% Discovery / 20% Defense / 10% Conquest        │
│ Key Rule:      Full keyword graduation pipeline active                        │
│ Decision Pace: Weekly, with daily monitoring on top campaigns                 │
├─────────────────────────────────────────────────────────────────────────────────┤
│ LARGE BUDGET (> $5,000/month, > $167/day)                                     │
├─────────────────────────────────────────────────────────────────────────────────┤
│ Products:      All active products                                            │
│ Campaigns:     20+                                                            │
│ Types:         Full SP/SB/SD/SBV stack                                        │
│ + DSP:         Consider Amazon DSP for off-Amazon targeting                   │
│ Split:         40% Profit / 20% Discovery / 20% Defense / 15% Conquest / 5% Test│
│ Key Rule:      Portfolio-level optimization, placement modifiers active        │
│ Decision Pace: Daily bid adjustments, weekly strategy                         │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### 11.4 Statistical Decision Thresholds

This is the most critical operational framework: **how much data do you need before making a decision?**

```
StatisticalDecisionThreshold {
  id
  decisionType: string            # what decision are we making?

  # Minimum data required
  minimumClicks: number | null
  minimumImpressions: number | null
  minimumSpend: Money | null
  minimumConversions: number | null
  minimumTimePeriod: Duration     # never decide faster than this

  # How to calculate dynamically
  dynamicFormula: string | null   # formula that adjusts threshold based on context

  # What to do when insufficient data
  insufficientDataAction: "wait" | "aggregate" | "use_parent_data" | "expert_default"
}
```

**The Decision Threshold Matrix:**

```
┌──────────────────────┬──────────────┬──────────────┬──────────────┬──────────────┐
│ Decision             │ Min Clicks   │ Min Spend    │ Min Time     │ Min Converts │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────┤
│ NEGATE keyword       │ 10-20*       │ 1× product   │ 7 days       │ 0            │
│ (zero sales)         │              │ price        │              │              │
│                      │              │              │              │              │
│ NEGATE keyword       │ 30+          │ 2× product   │ 14 days      │ n/a          │
│ (high ACOS)          │              │ price        │              │              │
│                      │              │              │              │              │
│ PROMOTE keyword      │ n/a          │ n/a          │ 14 days      │ 3-5          │
│ (graduate up)        │              │              │              │              │
│                      │              │              │              │              │
│ ADJUST bid           │ 20+          │ $10+         │ 7 days       │ n/a          │
│                      │              │              │              │              │
│ EVALUATE CTR         │ n/a          │ n/a          │ 7 days       │ n/a          │
│                      │ (1000+ impr) │              │              │              │
│                      │              │              │              │              │
│ EVALUATE CVR         │ 50-100       │ n/a          │ 14 days      │ n/a          │
│                      │              │              │              │              │
│ TRUST ACOS number    │ n/a          │ 2-3× product │ 14 days      │ 5-10         │
│                      │              │ price        │              │              │
│                      │              │              │              │              │
│ PAUSE campaign       │ n/a          │ 3× product   │ 21 days      │ < 2          │
│                      │              │ price        │              │              │
│                      │              │              │              │              │
│ JUDGE placement perf │ 50+          │ n/a          │ 14 days      │ 3+           │
│                      │              │              │              │              │
│ NEGATE (irrelevant)  │ 1-2          │ any          │ any          │ n/a          │
│ (clearly wrong term) │              │              │              │              │
└──────────────────────┴──────────────┴──────────────┴──────────────┴──────────────┘

* Dynamic negation threshold formula:
  Click Threshold = Max Acceptable CPA ÷ Average CPC
  Example: $10 max CPA ÷ $0.50 CPC = 20 clicks before negating

  OR simplified: if product CVR is ~10%, need ~10 clicks to expect 1 sale
                  if product CVR is ~5%, need ~20 clicks to expect 1 sale
```

**Insufficient Data Strategies:**

```
When data is insufficient for a decision:

1. WAIT          → Accumulate more data. Best when budget allows.

2. AGGREGATE     → Combine multiple weeks of data into one window.
                   e.g., 3 clicks/week × 4 weeks = 12 clicks → now decidable.
                   Risk: ignores recency (things change).

3. USE PARENT    → Use parent-ASIN-level data when child-ASIN data is sparse.
                   Or use campaign-level data when keyword-level is sparse.
                   Risk: hides variant-level differences.

4. EXPERT DEFAULT→ Apply rule-of-thumb when data will never be sufficient.
                   e.g., "$50 spent, 0 sales → negate even with only 8 clicks"
                   Risk: may negate future winners.

Priority: WAIT > AGGREGATE > USE PARENT > EXPERT DEFAULT
```

### 11.5 Profitability Analysis

```
ProductProfitability {
  id
  product: Product
  period: DateRange

  # Revenue
  totalRevenue: Money             # from Business Report
  organicRevenue: Money           # totalRevenue - adSales (estimated)
  adAttributedRevenue: Money      # from Advertising Reports

  # Costs
  cogs: Money                     # cost of goods × units sold
  amazonFees: Money               # referral + FBA × units sold
  adSpend: Money                  # total advertising spend

  # Profit
  grossProfit: Money              # revenue - cogs - amazonFees
  netProfit: Money                # grossProfit - adSpend
  grossMarginPct: Percentage
  netMarginPct: Percentage

  # Efficiency
  actualACOS: Percentage          # adSpend / adAttributedRevenue
  actualTACOS: Percentage         # adSpend / totalRevenue
  breakEvenACOS: Percentage       # grossProfit / totalRevenue
  profitPerUnit: Money

  # Trajectory
  marginTrend: "improving" | "stable" | "declining"
  organicShareTrend: "growing" | "stable" | "shrinking"
  # growing organic share = healthier business (less ad-dependent)
}
```

---

## Part 12: Inventory Intelligence

Inventory directly constrains advertising and pricing strategy. Running out of stock kills organic rank. Overstocking ties up capital. This section models the inventory signals that drive AM decisions.

### 12.1 Inventory Snapshot

```
InventorySnapshot {
  id
  product: Product
  variant: Variant
  snapshotDate: Date

  # Current levels
  availableUnits: number          # sellable units in Amazon warehouse
  inboundUnits: number            # units in transit to Amazon
  reservedUnits: number           # units in customer orders, not yet shipped
  unfulfillableUnits: number      # damaged, expired, etc.

  # Velocity
  avgDailySales: number           # trailing 7-day or 30-day average
  salesVelocityTrend: "accelerating" | "stable" | "decelerating"

  # Coverage
  daysOfCover: number             # availableUnits / avgDailySales
  daysOfCoverWithInbound: number  # (available + inbound) / avgDailySales

  # Thresholds
  reorderPoint: number            # units at which to trigger reorder
  safetyStock: number             # minimum units to always maintain

  # Status
  stockStatus: "healthy" | "watch" | "low" | "critical" | "stockout"
  # healthy:  > 60 days of cover
  # watch:    30-60 days of cover
  # low:      14-30 days of cover
  # critical: < 14 days of cover
  # stockout: 0 available units
}
```

### 12.2 Stockout Prediction

```
StockoutPrediction {
  id
  variant: Variant
  predictionDate: Date

  # Projection
  projectedStockoutDate: Date | null   # when will we run out?
  daysUntilStockout: number | null
  confidence: "high" | "medium" | "low"

  # Scenarios
  scenarios: [{
    label: "current_velocity" | "accelerating" | "peak_season" | "with_ad_increase"
    avgDailySales: number
    projectedStockoutDate: Date
    daysUntilStockout: number
  }]

  # Recommendation
  action: "no_action" | "monitor" | "reorder_now" | "reduce_ad_spend" | "pause_ads"
  urgency: "none" | "low" | "medium" | "high" | "critical"
}
```

### 12.3 Inventory ↔ Advertising Strategy Link

This is the critical connection: inventory level directly dictates ad strategy.

```
┌──────────────────┬────────────────────────────────────────────────┐
│ Stock Status     │ Advertising Response                           │
├──────────────────┼────────────────────────────────────────────────┤
│ healthy (>60d)   │ Full budget. Push for ranking. Launch campaigns│
│                  │ acceptable. Aggressive discovery.              │
│                  │                                                │
│ watch (30-60d)   │ Maintain current spend. No new launches.       │
│                  │ Focus on profitable campaigns only.            │
│                  │ Alert: order more inventory.                   │
│                  │                                                │
│ low (14-30d)     │ Reduce discovery spend by 50%.                 │
│                  │ Pause conquest campaigns.                      │
│                  │ Keep brand defense + top performers only.      │
│                  │ Urgent: reorder.                               │
│                  │                                                │
│ critical (<14d)  │ Reduce all bids by 30-50%.                     │
│                  │ Pause all except brand defense.                │
│                  │ Consider raising price to slow velocity.       │
│                  │ Emergency: check restock ETA.                  │
│                  │                                                │
│ stockout (0)     │ PAUSE ALL ADS immediately.                     │
│                  │ Do NOT spend on product that can't be bought.  │
│                  │ Preserve organic rank by minimizing time OOS.  │
│                  │ Plan re-launch strategy for when back in stock.│
└──────────────────┴────────────────────────────────────────────────┘
```

### 12.4 Inventory Alert

```
InventoryAlert {
  id
  variant: Variant
  alertDate: Date

  type: "approaching_reorder" | "below_safety_stock" | "stockout_imminent"
       | "stockout_occurred" | "overstock" | "velocity_spike"

  severity: "info" | "warning" | "critical"

  # Context
  currentDaysOfCover: number
  projectedStockoutDate: Date | null

  # What this means for advertising
  adStrategyImpact: string        # human-readable impact statement
  suggestedAdActions: [{
    action: string
    campaigns: [Campaign] | "all"
  }]

  acknowledged: boolean
  acknowledgedBy: AccountManager | null
  acknowledgedDate: Date | null
}
```

---

## Part 13: Competitive Intelligence (Enhanced)

The original `Competitor` entity (Part 4) captures a point-in-time snapshot. This section adds **temporal tracking** — competitors change, and detecting those changes is critical.

### 13.1 Competitor Snapshot (Temporal)

```
CompetitorSnapshot {
  id
  competitor: Competitor
  snapshotDate: Date

  # Listing state at this point
  price: Money
  starRating: number
  reviewCount: number
  bsr: number | null
  hasAPlusContent: boolean
  hasCoupon: boolean
  couponValue: string | null      # "$2 off" or "10% off"
  hasDeal: boolean
  dealType: string | null         # "Lightning Deal", "Best Deal", etc.
  mainImageUrl: string | null
  titleSnapshot: string
  bulletCount: number
  imageCount: number
  videoCount: number

  # Advertising presence
  sponsoredOnOurKeywords: [string]  # which of OUR keywords they're bidding on
  sponsoredPosition: "top_of_search" | "middle" | "bottom" | "product_page" | null

  # Buy Box
  hasBuyBox: boolean
  buyBoxSeller: string | null     # who has the buy box?

  # Availability
  inStock: boolean
  deliverySpeed: string | null    # "Prime 1-day", "3-5 days", etc.
}
```

### 13.2 Competitor Change Detection

```
CompetitorChange {
  id
  competitor: Competitor
  detectedDate: Date

  changeType: "price_change" | "review_milestone" | "rating_change"
             | "new_coupon" | "deal_launched" | "listing_update"
             | "new_images" | "a_plus_added" | "stock_change"
             | "new_competitor_entered" | "competitor_exited"

  # What changed
  field: string                   # which field changed
  previousValue: string
  newValue: string

  # Impact assessment
  threatLevel: "high" | "medium" | "low" | "opportunity"
  # high: competitor dropped price below us / passed our review count
  # medium: competitor made listing improvements
  # low: minor change
  # opportunity: competitor went OOS / raised price / lost reviews

  impactOnUs: string              # "They're now cheaper by $3"
  suggestedResponse: string | null # "Consider matching or emphasize quality diff"
}
```

### 13.3 Market Share Trend (from SQP)

```
MarketShareTrend {
  id
  brand: Brand
  keyword: string                 # the search query being tracked

  # Time series of our share
  dataPoints: [{
    period: DateRange             # weekly or monthly

    # Our share metrics (from SQP)
    ourImpressionShare: Percentage
    ourClickShare: Percentage
    ourCartAddShare: Percentage
    ourPurchaseShare: Percentage

    # Total market size
    totalSearchVolume: number
    searchFrequencyRank: number | null  # from Top Search Terms report

    # Funnel conversion (our performance at each stage)
    clickThroughRate: Percentage  # our clicks / our impressions
    cartRate: Percentage          # our cart adds / our clicks
    purchaseRate: Percentage      # our purchases / our cart adds
  }]

  # Trends
  impressionShareTrend: "gaining" | "stable" | "losing"
  clickShareTrend: "gaining" | "stable" | "losing"
  purchaseShareTrend: "gaining" | "stable" | "losing"

  # Where in the funnel we're losing (diagnostic)
  funnelWeakPoint: "visibility" | "click_appeal" | "consideration" | "conversion" | null
  # visibility:    impression share low → not showing up (ranking/ad issue)
  # click_appeal:  impressions ok but click share low → listing not compelling in search results
  # consideration: clicks ok but cart adds low → detail page not converting
  # conversion:    cart adds ok but purchase low → price/availability/trust issue
}
```

### 13.4 Competitor Threat Assessment

```
CompetitorThreatAssessment {
  id
  competitor: Competitor
  assessmentDate: Date

  # Scoring (1-10 scale)
  priceThreat: number             # are they cheaper? how much?
  reviewThreat: number            # do they have more/better reviews?
  listingQualityThreat: number    # is their listing better than ours?
  adAggressiveness: number        # how aggressively are they bidding on our terms?
  marketShareThreat: number       # are they gaining share on key queries?

  overallThreatScore: number      # weighted composite (1-10)

  threatTrajectory: "escalating" | "stable" | "diminishing"

  # Recommended response
  responseStrategy: "ignore" | "monitor" | "defend" | "counter_attack" | "differentiate"
  responseActions: [string]       # specific recommended actions
}
```

---

## Part 14: Review & Reputation Management

Reviews are the moat. A 4.5-star product with 1,000 reviews will outperform a 5.0-star product with 10 reviews. This section models the strategic management of reviews.

### 14.1 Review Strategy

```
ReviewStrategy {
  id
  product: Product

  # Current state
  currentRating: number           # e.g., 4.3
  currentReviewCount: number
  ratingDistribution: {           # how many 1-5 star reviews
    fiveStar: number
    fourStar: number
    threeStar: number
    twoStar: number
    oneStar: number
  }

  # Velocity
  reviewVelocity: number          # reviews per month (trailing 30 days)
  velocityBenchmark: number       # category/competitor average
  velocityStatus: "above_average" | "average" | "below_average"

  # Vine enrollment
  vineEligible: boolean           # must have < 30 reviews
  vineEnrolled: boolean
  vineUnitsDistributed: number
  vineReviewsReceived: number
  vineConversionRate: Percentage  # reviews / units distributed (benchmark: 25-50%)

  # Goals
  targetRating: number            # e.g., 4.5
  targetReviewCount: number       # milestone (50, 100, 500, 1000)
  estimatedTimeToTarget: Duration # at current velocity

  # Tactics (approved methods only)
  activeTactics: [
    "vine" |                      # Amazon Vine program
    "request_a_review" |          # Amazon's native "Request a Review" button
    "product_insert" |            # non-incentivized insert card
    "follow_up_email" |           # Buyer-Seller messaging (if allowed)
    "product_improvement"         # fix issues causing negative reviews
  ]
}
```

### 14.2 Review Sentiment Tracking

```
ReviewSentimentAnalysis {
  id
  product: Product
  period: DateRange

  # Aggregate sentiment
  avgRatingThisPeriod: number
  avgRatingPriorPeriod: number
  ratingTrend: "improving" | "stable" | "declining"

  # Theme extraction
  positiveThemes: [{
    theme: string                 # e.g., "great flavor", "fast shipping"
    frequency: number             # how often mentioned
    exampleQuote: string
  }]

  negativeThemes: [{
    theme: string                 # e.g., "bottle leaked", "too spicy"
    frequency: number
    severity: "critical" | "moderate" | "minor"
    # critical: affects product safety/usability → needs product change
    # moderate: affects satisfaction → listing/expectation management
    # minor: taste preference, edge case

    suggestedAction: string       # e.g., "improve packaging" or "set expectations in bullets"
    exampleQuote: string
    addressedInListing: boolean   # are we already addressing this concern?
  }]

  # Questions from reviews (buying signals)
  commonQuestions: [{
    question: string              # e.g., "Is this gluten free?"
    frequency: number
    answeredInListing: boolean    # do our bullets/A+ answer this?
    answeredInQA: boolean         # is it in the Q&A section?
  }]
}
```

### 14.3 Negative Review Response

```
NegativeReviewResponse {
  id
  product: Product
  reviewDate: Date
  reviewRating: number            # 1-3 stars
  reviewText: string

  # Classification
  issueType: "product_defect" | "shipping_damage" | "wrong_expectations"
            | "taste_preference" | "competitor_attack" | "legitimate_complaint"

  # Response
  responded: boolean
  responseDate: Date | null
  responseText: string | null
  responseBy: AccountManager | null

  # Policy: Respond within 24 hours to show commitment
  responseTimeliness: "within_24h" | "within_48h" | "late" | "not_responded"

  # Follow-up
  removalRequested: boolean       # only if violates Amazon policy
  removalGranted: boolean | null

  # Learning
  feedbackToProduct: string | null  # what should change in the product?
  feedbackToListing: string | null  # what should change in the listing?
  linkedAction: ActionItem | null   # did this trigger a specific action?
}
```

### 14.4 Review Health Dashboard

```
Review Health = f(rating, velocity, sentiment trend, competitive position)

┌─────────────────┬──────────────────┬──────────────────────────────┐
│ Signal          │ Healthy          │ Unhealthy                    │
├─────────────────┼──────────────────┼──────────────────────────────┤
│ Rating          │ ≥ 4.3 stars      │ < 4.0 stars                  │
│ Review Count    │ > competitors    │ < 50% of top competitor      │
│ Velocity        │ ≥ category avg   │ < 50% of category avg        │
│ Sentiment Trend │ stable/improving │ declining                    │
│ Negative Theme  │ no critical      │ critical themes unaddressed  │
│ Response Rate   │ 100% within 24h  │ < 80% or > 48h              │
└─────────────────┴──────────────────┴──────────────────────────────┘
```

---

## Part 15: Seasonal & Temporal Modeling

Every Amazon business has seasonal patterns. BBQ sauce peaks in summer. Toys peak in Q4. Ignoring seasonality means misdiagnosing performance changes.

### 15.1 Seasonal Pattern

```
SeasonalPattern {
  id
  product: Product | null         # null = brand-level pattern
  brand: Brand

  # Pattern type
  patternType: "category_seasonal" | "event_driven" | "weather_dependent"
              | "cultural" | "custom"

  # Monthly indices (1.0 = average month)
  monthlyIndex: {
    jan: number   # e.g., 0.6 (40% below average)
    feb: number   # e.g., 0.7
    mar: number   # e.g., 0.8
    apr: number   # e.g., 1.0
    may: number   # e.g., 1.3 (BBQ season starts)
    jun: number   # e.g., 1.5
    jul: number   # e.g., 1.6 (peak)
    aug: number   # e.g., 1.4
    sep: number   # e.g., 1.1
    oct: number   # e.g., 0.9
    nov: number   # e.g., 0.8
    dec: number   # e.g., 1.0 (holiday gift sets)
  }

  # Data source
  basedOn: "historical_sales" | "category_data" | "estimated"
  yearsOfData: number             # how many years inform this pattern

  # Confidence
  reliability: "high" | "medium" | "low"
  # high: 2+ years of consistent pattern
  # medium: 1 year of data
  # low: estimated from category/competitor
}
```

### 15.2 Amazon Events Calendar

```
AmazonEvent {
  id
  name: string                    # "Prime Day", "Black Friday", "Back to School"

  # Timing
  typicalDateRange: { month: number, weekOfMonth: number }  # approximate
  confirmedDates: DateRange | null  # set when Amazon announces

  # Preparation timeline
  prepTimeline: [{
    weeksBeforeEvent: number
    action: string
    category: "inventory" | "listing" | "advertising" | "pricing" | "review"
  }]

  # Impact
  expectedSalesMultiplier: number   # e.g., 2.5 = 150% above normal
  expectedAdCostMultiplier: number  # e.g., 1.8 = CPCs increase 80%
  categoryRelevance: "high" | "medium" | "low" | "none"
  # BBQ sauce on Prime Day = "medium"; on Black Friday = "low"

  # Strategy
  participationDecision: "full" | "limited" | "skip"
  dealType: "lightning_deal" | "best_deal" | "coupon" | "price_drop" | "none"
  budgetIncreasePct: Percentage | null  # how much to increase ad budget
}
```

**Key Amazon Events:**

```
┌────────────────────────┬──────────────┬────────────────┬─────────────────────────┐
│ Event                  │ Typical Time │ Prep Lead Time │ Impact                  │
├────────────────────────┼──────────────┼────────────────┼─────────────────────────┤
│ New Year / Resolution  │ Jan 1-10     │ 4 weeks        │ Category-dependent      │
│ Valentine's Day        │ Feb 1-14     │ 4 weeks        │ Gift categories         │
│ Spring Deals Event     │ March        │ 6 weeks        │ Moderate                │
│ Prime Day (Summer)     │ July         │ 8-10 weeks     │ Major (2-3× sales)      │
│ Back to School         │ Aug-Sep      │ 6 weeks        │ Category-dependent      │
│ Prime Big Deal Days    │ October      │ 8-10 weeks     │ Major (BFCM dress       │
│                        │              │                │ rehearsal)              │
│ Black Friday           │ 4th Fri Nov  │ 10-12 weeks    │ Biggest (3-5× sales)    │
│ Cyber Monday           │ Mon after BF │ (same as BF)   │ Major (2-4× sales)      │
│ Christmas Week         │ Dec 20-26    │ 8 weeks        │ Gift categories         │
│ FBA Inventory Deadline │ ~Oct 19      │ Plan by Aug    │ Hard deadline for Q4    │
└────────────────────────┴──────────────┴────────────────┴─────────────────────────┘
```

### 15.3 Seasonal Strategy

```
SeasonalStrategy {
  id
  brand: Brand
  event: AmazonEvent
  year: number

  # Pre-event
  inventoryPlan: {
    targetStockLevel: number      # units to have before event
    reorderDeadline: Date
    extraUnitsNeeded: number
  }

  listingPrep: [{
    action: string                # "Update main image for holiday theme"
    deadline: Date
    status: "planned" | "in_progress" | "completed"
  }]

  adStrategy: {
    budgetIncreasePct: Percentage
    newCampaignsToLaunch: [string]
    bidsIncreasePct: Percentage
    startDate: Date               # when to ramp up
    endDate: Date                 # when to pull back
  }

  pricingStrategy: {
    dealType: string | null
    discountPct: Percentage | null
    couponValue: Money | null
    priceChangeDate: Date | null
  }

  # Post-event
  postEventPlan: {
    adBudgetReturnDate: Date      # when to reduce back to normal
    priceRestoreDate: Date
    retargetingStrategy: string | null
    dataAnalysisDeadline: Date    # when to analyze results
  }

  # Results (filled after event)
  results: {
    actualSalesMultiplier: number | null
    actualAdSpend: Money | null
    actualROAS: number | null
    lessonsLearned: [string]
  } | null
}
```

### 15.4 Year-over-Year Comparison Framework

```
YoYComparison {
  id
  brand: Brand
  product: Product | null

  currentPeriod: DateRange
  priorYearPeriod: DateRange      # same period last year

  metrics: [{
    metric: MetricType
    currentValue: number
    priorYearValue: number
    changePct: Percentage
    changeDirection: "up" | "down" | "flat"

    # Context: is this change expected?
    seasonalExpectation: "in_line" | "above_expected" | "below_expected"
    # If seasonal index says July should be 1.6× and we're at 1.7×, that's "above_expected"

    explanation: string | null    # what drove this change?
  }]
}
```

---

## Part 16: Variant Strategy

Products on Amazon often exist as parent-child families (a BBQ sauce in 5 flavors, or a tool set in 3 sizes). Managing variants is a strategic domain.

### 16.1 Variant Portfolio

```
VariantPortfolio {
  id
  product: Product                # the parent

  variants: [{
    variant: Variant
    role: "hero" | "gateway" | "upsell" | "niche" | "declining"
    # hero:     highest sales, most promoted, the one you're known for
    # gateway:  cheapest variant, designed to get first purchase
    # upsell:   larger size / bundle, higher margin
    # niche:    specialty variant for a specific segment
    # declining: losing demand, consider sunsetting

    # Performance
    salesSharePct: Percentage     # what % of parent's total sales
    revenueSharePct: Percentage
    marginRank: number            # 1 = highest margin variant
    reviewContribution: number    # reviews on this child vs total

    # Advertising allocation
    adSpendSharePct: Percentage   # what % of product ad spend
    adSpendJustification: string  # why this allocation
  }]

  # Portfolio health
  heroVariantExists: boolean      # is there a clear winner?
  heroConcentration: Percentage   # if hero is 80%+ of sales, too concentrated?
  variantCount: number
  activeVariantCount: number      # variants with sales in last 30 days
}
```

### 16.2 Variant Performance Comparison

```
VariantComparison {
  id
  product: Product
  period: DateRange

  # Per-variant performance
  variants: [{
    variant: Variant

    # Sales
    units: number
    revenue: Money
    avgPrice: Money

    # Traffic
    sessions: number
    pageViews: number

    # Conversion
    cvr: Percentage

    # Advertising
    adSpend: Money
    adSales: Money
    acos: Percentage

    # Profitability
    grossMargin: Percentage
    netMargin: Percentage
    profitPerUnit: Money

    # Rankings
    bsr: number | null
    organicRank: { keyword: string, position: number }[] | null
  }]

  # Insights
  bestConverter: Variant          # highest CVR
  mostProfitable: Variant         # highest net margin per unit
  bestAdEfficiency: Variant       # lowest ACOS
  highestVelocity: Variant        # most units sold

  recommendations: [{
    variant: Variant
    recommendation: string        # "Shift 10% of ad spend from X to Y"
    rationale: string
  }]
}
```

### 16.3 Cannibalization Detection

```
CannibalizationAnalysis {
  id
  product: Product
  period: DateRange

  # Pairs of variants that may cannibalize each other
  cannibalizationPairs: [{
    variantA: Variant
    variantB: Variant

    # Evidence
    sharedKeywords: [string]      # keywords both variants rank/bid on
    keywordOverlapPct: Percentage # how much targeting overlap

    # Behavior patterns
    crossElasticity: "high" | "medium" | "low"
    # high: when A's sales go up, B's go down proportionally
    # medium: some relationship
    # low: independent demand

    evidence: string              # "When we bid on 'bbq sauce' for Flavor A,
                                  #  Flavor B loses sessions"

    # Resolution
    recommendedAction: "differentiate_keywords" | "differentiate_targeting"
                     | "merge_listings" | "sunset_one" | "accept_overlap"
    rationale: string
  }]
}
```

### 16.4 Variant Lifecycle

```
VariantLifecycle {
  id
  variant: Variant

  currentStage: "planned" | "launch" | "growth" | "mature" | "declining" | "sunset"

  stageHistory: [{
    stage: string
    enteredDate: Date
    exitedDate: Date | null
    metrics: {
      avgMonthlySales: Money
      avgMonthlyUnits: number
      reviewCount: number
      bsr: number | null
    }
  }]

  # Stage-specific strategy
  stageStrategy: {
    launch: {
      targetReviewCount: number     # reviews needed before "growth"
      maxAcceptableACOS: Percentage  # launch ACOS can be high
      parentListingMerged: boolean   # is it under the parent yet?
    }
    growth: {
      targetMarketSharePct: Percentage
      keywordExpansionActive: boolean
      adBudgetRampPct: Percentage
    }
    mature: {
      targetACOS: Percentage         # tighten to profitability
      organicSalesTarget: Percentage # target organic share
      defenseKeywords: [string]
    }
    declining: {
      salesDeclinePct: Percentage    # how fast declining?
      sunsettingDecision: "maintain" | "reduce_support" | "discontinue"
      remainingInventoryUnits: number
    }
  }
}
```

---

## Part 17: Listing Experimentation

Amazon's **Manage Your Experiments** (MYE) allows A/B testing of listing elements. This section models systematic experimentation.

### 17.1 Listing Experiment

```
ListingExperiment {
  id
  product: Product

  # What we're testing
  elementTested: "title" | "main_image" | "image" | "bullet_points"
                | "a_plus_content" | "brand_story" | "description"

  hypothesis: string              # "Adding 'South Carolina' to title will increase
                                  #  CTR for regional searchers by 10%"

  # Variants
  control: {
    label: "Control (current)"
    content: string | URL         # current listing content
  }
  treatment: {
    label: string                 # "Variant B: Regional title"
    content: string | URL         # proposed change
    changeDescription: string     # what specifically is different
  }

  # Targeting
  targetSegment: CustomerSegment | null  # which segment should this help?
  targetKeywords: [string] | null        # which keywords should improve?

  # Setup
  status: "planned" | "running" | "completed" | "cancelled"
  startDate: Date | null
  endDate: Date | null
  minimumDuration: Duration       # recommend 8-10 weeks

  # Traffic requirements
  estimatedWeeklyTraffic: number  # sessions per week on this ASIN
  trafficSufficient: boolean      # does this ASIN qualify for MYE?
  # Amazon requires "high-traffic ASINs" — varies by category

  # Results (filled when completed)
  results: ExperimentResult | null
}
```

### 17.2 Experiment Result

```
ExperimentResult {
  id
  experiment: ListingExperiment

  # Sample
  controlSampleSize: number       # logged-in shoppers who saw control
  treatmentSampleSize: number
  totalDuration: Duration

  # Outcomes
  controlMetrics: {
    unitsSold: number
    revenue: Money
    cvr: Percentage               # units per unique visitor
  }
  treatmentMetrics: {
    unitsSold: number
    revenue: Money
    cvr: Percentage
  }

  # Statistical significance
  cvrLift: Percentage             # treatment CVR vs control CVR
  revenueLift: Percentage
  confidenceLevel: Percentage     # Amazon uses Bayesian approach, reports 95% CI
  isSignificant: boolean          # did the test reach significance?

  # Outcome
  winner: "control" | "treatment" | "inconclusive"

  # Decision
  decision: "adopt_treatment" | "keep_control" | "retest" | "iterate"
  decisionRationale: string
  adoptedDate: Date | null        # when was the winning version made permanent?

  # Learning
  learnings: [string]             # what did we learn beyond the result?
  feedsInto: ListingExperiment | null  # does this inspire a follow-up test?
}
```

### 17.3 Experimentation Roadmap

```
ExperimentationRoadmap {
  id
  product: Product

  # Prioritized test queue
  queue: [{
    priority: number              # 1 = run first
    element: string               # what to test
    hypothesis: string
    expectedImpact: "high" | "medium" | "low"
    effort: "low" | "medium" | "high"  # creative effort to create variant

    # Priority score = impact × (1 / effort)
    # Test high-impact, low-effort first

    status: "queued" | "in_experiment" | "completed"
    experiment: ListingExperiment | null
  }]

  # Test velocity
  averageTestDuration: Duration   # how long tests typically run
  testsCompletedThisQuarter: number
  testsPlannedThisQuarter: number
}
```

**Experimentation Priority Framework:**

```
┌─────────────────────┬────────────┬────────────────────────────────────────┐
│ Element             │ Impact     │ Test When...                           │
├─────────────────────┼────────────┼────────────────────────────────────────┤
│ Main Image          │ Highest    │ CTR below category avg, or when new   │
│                     │            │ creative is available                  │
│                     │            │                                        │
│ Title               │ High       │ CTR low on key search terms, or when  │
│                     │            │ keyword strategy changes               │
│                     │            │                                        │
│ A+ Content          │ High       │ CVR below target, or seasonal refresh │
│                     │            │                                        │
│ Bullet Points       │ Medium     │ Review themes suggest unaddressed      │
│                     │            │ concerns, or CVR below target          │
│                     │            │                                        │
│ Secondary Images    │ Medium     │ After main image optimized, or when   │
│                     │            │ new product features to highlight      │
│                     │            │                                        │
│ Brand Story         │ Lower      │ After other elements optimized        │
│                     │            │                                        │
│ Description         │ Lowest     │ Rarely (most shoppers don't read it)  │
└─────────────────────┴────────────┴────────────────────────────────────────┘

Note: Test ONE element at a time unless using MYE multi-attribute tests.
      Run each test for minimum 8 weeks (4 weeks if high traffic).
```

---

## Part 18: Conflict Resolution & Decision Arbitration

Different parts of the system may recommend conflicting actions. The inventory engine says "pause ads" while the ranking engine says "maintain spend for organic rank." The pricing engine says "raise price" while the competitive engine says "match competitor." This section defines how conflicts are detected and resolved.

### 18.1 The Conflict Problem

```
Recommendation sources that can conflict:

  ┌─────────────────────┐
  │ Prediction Engine   │──┐
  │ (Part 3.10)         │  │
  └─────────────────────┘  │
  ┌─────────────────────┐  │    ┌──────────────────────┐
  │ Inventory Engine    │──┼───▶│ CONFLICT DETECTOR    │──▶ Resolution
  │ (Part 12)           │  │    └──────────────────────┘
  └─────────────────────┘  │
  ┌─────────────────────┐  │
  │ Pricing Engine      │──┤
  │ (Part 11)           │  │
  └─────────────────────┘  │
  ┌─────────────────────┐  │
  │ Competitive Engine  │──┤
  │ (Part 13)           │  │
  └─────────────────────┘  │
  ┌─────────────────────┐  │
  │ Budget Constraints  │──┤
  │ (Part 11.3)         │  │
  └─────────────────────┘  │
  ┌─────────────────────┐  │
  │ Seasonal Engine     │──┤
  │ (Part 15)           │  │
  └─────────────────────┘  │
  ┌─────────────────────┐  │
  │ Review Engine       │──┘
  │ (Part 14)           │
  └─────────────────────┘
```

### 18.2 Recommendation Entity

Every engine produces recommendations in a common format:

```
Recommendation {
  id
  source: "prediction" | "inventory" | "pricing" | "competitive"
         | "budget" | "seasonal" | "review" | "advertising" | "listing"

  date: Date
  product: Product | null
  brand: Brand

  # What is recommended
  actionType: ActionType          # from the action taxonomy
  actionDetails: string           # specific parameters

  # Why
  rationale: string               # human-readable explanation
  dataEvidence: string            # what data supports this

  # Impact estimate
  expectedImpact: {
    metric: MetricType
    direction: "increase" | "decrease"
    magnitude: "small" | "moderate" | "large"
    confidence: "high" | "medium" | "low"
    timeToEffect: Duration
  }

  # Urgency
  urgency: "immediate" | "this_week" | "this_month" | "when_convenient"
  expiresAt: Date | null          # recommendation may become stale

  # Constraints this recommendation respects
  constraintsConsidered: [string] # e.g., ["budget limit", "inventory level"]

  # Status
  status: "proposed" | "approved" | "rejected" | "superseded" | "executed"
  conflictsWith: [Recommendation] # other recommendations it conflicts with
}
```

### 18.3 Conflict Detection

```
Conflict {
  id
  detectedDate: Date

  # The conflicting recommendations
  recommendationA: Recommendation
  recommendationB: Recommendation

  # Nature of conflict
  conflictType: "opposite_direction" | "resource_competition" | "timing_clash"
               | "goal_incompatible" | "constraint_violation"

  # opposite_direction: A says "increase bids", B says "decrease bids"
  # resource_competition: both need budget that doesn't exist
  # timing_clash: A says "do now", B says "wait until after event"
  # goal_incompatible: A optimizes for rank, B optimizes for profit
  # constraint_violation: A's action would violate a constraint B relies on

  description: string             # human-readable conflict explanation

  # Resolution
  resolution: ConflictResolution | null
}
```

### 18.4 Conflict Resolution

```
ConflictResolution {
  id
  conflict: Conflict

  resolvedBy: "hierarchy" | "constraint" | "synthesis" | "human"

  # Resolution methods (tried in order):

  # 1. HIERARCHY — fixed priority ordering
  # Some concerns always trump others:
  hierarchyResult: {
    winner: Recommendation
    rule: string
    # Rules (highest to lowest priority):
    # 1. Amazon policy compliance (never violate TOS)
    # 2. Inventory constraint (can't advertise what you can't sell)
    # 3. Budget constraint (can't spend what you don't have)
    # 4. Client directive (explicit client instruction overrides engine)
    # 5. Profitability floor (never go below break-even deliberately)
    # 6. Ranking/growth goals (strategic intent)
    # 7. Optimization preference (engine suggestions)
  } | null

  # 2. CONSTRAINT SATISFACTION — find an action that satisfies both
  constraintResult: {
    synthesizedAction: string     # compromise action
    satisfiesA: Percentage        # how much of A's goal is met
    satisfiesB: Percentage        # how much of B's goal is met
    tradeoff: string              # what we're giving up
  } | null

  # 3. SYNTHESIS — combine both recommendations into a modified plan
  synthesisResult: {
    modifiedAction: string
    explanation: string
    # Example: "Reduce bids by 15% (not 30% as inventory engine suggests,
    #  not 0% as ranking engine suggests) — balances stock conservation
    #  with rank maintenance"
  } | null

  # 4. HUMAN DECISION — escalate to AM when system can't resolve
  humanDecision: {
    escalatedTo: AccountManager
    escalatedDate: Date
    context: string               # what the AM needs to know
    options: [{
      option: string
      prosAndCons: string
    }]
    decision: string | null       # what the AM chose
    decidedDate: Date | null
  } | null

  # Outcome tracking
  chosenRecommendation: Recommendation | null
  chosenAction: string
  outcome: MeasuredOutcome | null  # did the resolution work?
}
```

### 18.5 Common Conflict Patterns

```
┌──────────────────────┬──────────────────────┬──────────────────────────────────┐
│ Engine A Says        │ Engine B Says        │ Resolution Pattern               │
├──────────────────────┼──────────────────────┼──────────────────────────────────┤
│ INVENTORY: Pause ads │ RANKING: Maintain    │ HIERARCHY: Inventory wins.       │
│ (stock < 14 days)    │ spend for rank       │ Can't sell what you don't have.  │
│                      │                      │ Reduce bids 50%, keep brand only │
│                      │                      │                                  │
│ PRICING: Raise price │ COMPETITIVE: Match   │ CONSTRAINT: Raise price only if  │
│ (margin too low)     │ competitor drop      │ still within 10% of competitor.  │
│                      │                      │ Otherwise, hold and differentiate│
│                      │                      │                                  │
│ BUDGET: Cut spend    │ SEASONAL: Increase   │ SYNTHESIS: Reallocate from       │
│ (over monthly budget)│ for Prime Day        │ low-performers to event          │
│                      │                      │ campaigns. Total budget stays.   │
│                      │                      │                                  │
│ PREDICTION: Increase │ BUDGET: Can't afford │ HIERARCHY: Budget wins.          │
│ bids on keyword X    │ more spend           │ Find budget by pausing weaker    │
│                      │                      │ keywords, not by overspending.   │
│                      │                      │                                  │
│ REVIEW: Push for     │ INVENTORY: Low stock │ HIERARCHY: Inventory wins.       │
│ more sales velocity  │ don't accelerate     │ Velocity without stock = worst   │
│ (need more reviews)  │                      │ outcome (stockout + rank loss).  │
│                      │                      │                                  │
│ AD ENGINE: Add broad │ LISTING: Title needs │ TIMING: Do listing first (1 wk),│
│ match for new KWs    │ optimization first   │ then launch broad match. Better  │
│                      │                      │ listing = better ad CVR.         │
│                      │                      │                                  │
│ COMPETITIVE: Launch  │ PROFITABILITY: Our   │ CONSTRAINT: Only if conquest     │
│ conquest campaign    │ margin can't support │ target ACOS stays above margin   │
│ (competitor weak)    │ high ACOS conquest   │ floor. Otherwise, monitor only.  │
└──────────────────────┴──────────────────────┴──────────────────────────────────┘
```

### 18.6 Resolution Priority Hierarchy

When all else fails, this is the pecking order:

```
Priority 1: COMPLIANCE        → Never violate Amazon TOS / policy
Priority 2: INVENTORY         → Can't sell air. Stock constraints are hard limits.
Priority 3: BUDGET            → Can't spend what doesn't exist. Hard financial limit.
Priority 4: CLIENT DIRECTIVE  → Explicit client instructions override engine logic.
Priority 5: PROFITABILITY     → Don't knowingly lose money (below break-even ACOS).
Priority 6: TIMING/SEASONAL   → Event windows are time-limited opportunities.
Priority 7: COMPETITIVE       → React to competitive threats.
Priority 8: GROWTH/RANKING    → Strategic growth goals.
Priority 9: OPTIMIZATION      → Fine-tuning for marginal improvements.

Rule: A higher-priority concern ALWAYS overrides a lower one.
      Within the same priority, use SYNTHESIS to find a middle ground.
      When two concerns are at the same priority and can't be synthesized,
      escalate to HUMAN DECISION.
```

---

## Appendix A: Metric Type Enum

```
MetricType =
  # Traffic
  | "sessions" | "page_views" | "page_views_per_session"
  | "impressions_organic" | "impressions_paid" | "external_traffic"

  # Conversion
  | "ctr_organic" | "ctr_paid" | "cvr" | "add_to_cart_rate"
  | "purchase_rate" | "buy_box_pct"

  # Advertising
  | "ad_spend" | "ad_sales" | "acos" | "roas" | "tacos"
  | "cpc" | "clicks_paid" | "advertised_sku_sales"
  | "other_sku_sales" | "halo_pct" | "new_to_brand_pct"

  # Revenue
  | "total_sales" | "total_units" | "avg_selling_price"
  | "organic_sales" | "organic_share_pct" | "b2b_sales"

  # Ranking
  | "bsr" | "organic_rank" | "impression_share"
  | "click_share" | "conversion_share" | "share_of_voice"

  # Customer
  | "star_rating" | "review_count" | "review_velocity"
  | "repeat_purchase_pct" | "repeat_revenue_pct" | "return_rate"

  # Account Health
  | "order_defect_rate" | "late_shipment_rate"
  | "cancellation_rate" | "ipi_score" | "days_of_cover"
```

## Appendix B: Action Type Enum

```
ActionType =
  # Advertising
  | "increase_bid" | "decrease_bid" | "set_bid_rule"
  | "increase_budget" | "decrease_budget" | "set_budget_cap"
  | "add_keyword" | "negate_keyword" | "pause_keyword" | "change_match_type"
  | "create_campaign" | "pause_campaign" | "archive_campaign" | "change_campaign_setting"
  | "add_product_target" | "add_category_target" | "negate_product_target"
  | "adjust_placement_modifier"

  # Listing
  | "update_title" | "update_main_image" | "add_image" | "add_video"
  | "update_bullet_points" | "update_description"
  | "create_a_plus_content" | "update_a_plus_content"
  | "update_backend_keywords" | "create_brand_store" | "update_brand_store"

  # Pricing
  | "change_price" | "create_coupon" | "create_promotion"
  | "submit_lightning_deal" | "enroll_subscribe_and_save" | "change_map_price"

  # Inventory
  | "restock_fba" | "create_removal_order" | "switch_fulfillment"

  # Reviews
  | "enroll_vine" | "respond_to_review" | "request_review" | "respond_to_question"

  # Account
  | "open_case" | "submit_brand_registry_ticket" | "appeal_suppression"

  # External
  | "drive_external_traffic" | "social_media_campaign" | "influencer_partnership"
```

## Appendix C: Amazon Report Column Reference

### Business Report (Detail Page Sales & Traffic by Child ASIN)
```
Columns:
  - (Child) ASIN
  - (Parent) ASIN
  - Title
  - Sessions
  - Session Percentage
  - Page Views
  - Page Views Percentage
  - Buy Box Percentage
  - Units Ordered
  - Units Ordered - B2B
  - Unit Session Percentage       # This is CVR
  - Ordered Product Sales
  - Ordered Product Sales - B2B
  - Total Order Items
```

### SQP Report (Search Query Performance)
```
Columns:
  - Search Query
  - Search Query Score
  - Search Query Volume
  - Impressions: Total Count
  - Impressions: Brand Count
  - Impressions: Brand Share (%)
  - Clicks: Total Count
  - Clicks: Brand Count
  - Clicks: Brand Share (%)
  - Cart Adds: Total Count
  - Cart Adds: Brand Count
  - Cart Adds: Brand Share (%)
  - Purchases: Total Count
  - Purchases: Brand Count
  - Purchases: Brand Share (%)
  - Brand Price (Median)
  - Price (Median)
  - Same-Day Shipping Speed
  - 1-Day Shipping Speed
  - 2-Day Shipping Speed
```

### Search Term Report (Advertising)
```
Columns:
  - Campaign Name
  - Ad Group Name
  - Targeting
  - Match Type
  - Customer Search Term
  - Impressions
  - Clicks
  - Click-Through Rate (CTR)
  - Cost Per Click (CPC)
  - Spend
  - 7 Day Total Sales
  - Total Advertising Cost of Sales (ACoS)
  - Total Return on Advertising Spend (RoAS)
  - 7 Day Total Orders
  - 7 Day Total Units
  - 7 Day Conversion Rate
  - 7 Day Advertised SKU Units
  - 7 Day Other SKU Units
  - 7 Day Advertised SKU Sales
  - 7 Day Other SKU Sales
```

### Placement Report
```
Columns:
  - Campaign Name
  - Placement Type                 # "Top of Search", "Rest of Search", "Product Pages"
  - Impressions
  - Clicks
  - Click-Through Rate (CTR)
  - Cost Per Click (CPC)
  - Spend
  - 7 Day Total Sales
  - Total ACoS
  - Total RoAS
  - 7 Day Total Orders
  - 7 Day Total Units
  - 7 Day Conversion Rate
```

### Purchased Product Report
```
Columns:
  - Campaign Name
  - Ad Group Name
  - Advertised ASIN
  - Advertised SKU
  - Purchased ASIN
  - Purchased SKU
  - Purchased Product Title
  - 7 Day Purchased Product Sales
  - 7 Day Purchased Product Units
```

### Top Search Terms Report
```
Columns:
  - Search Term
  - Search Frequency Rank
  - #1 Clicked ASIN
  - #1 Product Title
  - #1 Click Share
  - #1 Conversion Share
  - #2 Clicked ASIN
  - #2 Product Title
  - #2 Click Share
  - #2 Conversion Share
  - #3 Clicked ASIN
  - #3 Product Title
  - #3 Click Share
  - #3 Conversion Share
```

### Market Basket Analysis Report
```
Columns:
  - ASIN
  - Title
  - #1 Purchased ASIN
  - #1 Purchased Title
  - #1 Combination (%)
  - #2 Purchased ASIN
  - #2 Purchased Title
  - #2 Combination (%)
  - #3 Purchased ASIN
  - #3 Purchased Title
  - #3 Combination (%)
```

### Repeat Purchase Behavior Report
```
Columns:
  - ASIN
  - Title
  - Orders
  - Unique Customers
  - Repeat Customer Share (%)
  - Repeat Purchase Ordered Product Sales ($)
  - Repeat Purchase Ordered Product Sales (%)
  - Repeat Ordered Units
  - Repeat Ordered Units (%)
```
