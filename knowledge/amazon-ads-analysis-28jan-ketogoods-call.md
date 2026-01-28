# Amazon Advertising Analysis Intelligence

**Source:** Keto Goods Analysis Session
**Date:** 2026-01-28
**Tags:** `ads`, `analysis`, `ACOS`, `CTR`, `hypothesis`, `placements`, `campaigns`, `search-terms`

---

## Document Purpose
This document captures expert-level Amazon advertising analysis methodology, data interpretation rules, and decision frameworks for account managers to reference when analyzing Amazon ad accounts.

---

## 1. CTR ANALYSIS METHODOLOGY

### Multi-Level CTR Investigation Framework

When a CTR issue is identified, NEVER conclude immediately. Follow this investigation sequence:

#### Level 1: Benchmark Verification
- **Question**: Is this CTR actually low for this industry/product category?
- **Action**: Compare against industry benchmarks
- **Rationale**: Different product categories have inherently different CTR ranges (e.g., t-shirts vs BBQ sauce)
- **Why it matters**: Products requiring more research naturally have lower CTR than impulse purchases

#### Level 2: Account-Level Analysis
- Check overall account CTR trends
- Compare organic vs paid traffic CTR
- If 80% of orders are organic and only 20% are paid, don't jump to conclusions about creative issues

#### Level 3: Branded vs Non-Branded
- Separate branded search performance from non-branded
- Branded searches naturally have higher CTR
- If branded volume is low and non-branded is high, that's valuable context

#### Level 4: Placement-Specific Analysis
- **Critical Understanding**: SQP (Search Query Performance) and Ads reports show different data
- Top of Search CTR
- Rest of Search CTR
- Product Pages CTR
- Each placement has different expected CTR ranges

#### Level 5: Campaign-Type Analysis
- Auto campaigns often show product page placements
- Manual campaigns may focus on search placements
- Compare campaign types separately

### CTR Benchmarking Context
Before declaring CTR as "the problem," establish:
1. What is the industry average?
2. What is the product category behavior? (high research vs low research purchase)
3. What is the historical performance?
4. What is the placement mix?

---

## 2. CRITICAL DATA INTERPRETATION RULES

### Search Query Performance (SQP) Report

**What SQP Captures:**
- ONLY search-based traffic
- Customer searches a keyword → sees your ad → clicks
- Does NOT include product page impressions
- Does NOT include browse node navigation
- Does NOT include recommendation widgets

**What This Means:**
- If your campaign shows mostly "Product Pages" placement but SQP shows good CTR, there's no contradiction
- SQP is showing the search-based subset only
- The bad CTR on product pages won't fully show in SQP

**When ASINs Appear as Search Terms in SQP:**
1. External traffic (customer comes from outside Amazon with ASIN)
2. Direct product lookup (customer types ASIN in search)
3. Customer service scenarios (customer has ASIN from previous order)
4. Rare edge cases

### Ads Report (Search Term Report)

**What Ads Report Captures:**
- All advertising impressions and clicks
- Includes search-based traffic
- Includes product page placements
- Includes all placement types

**Critical Difference:**
- Ads report impression count ≠ SQP impression count
- Ads report includes product pages, SQP doesn't
- When analyzing CTR discrepancies, this is usually why

### Placement Report Data

**Understanding Placement Data:**
- Campaigns can show in multiple placements simultaneously
- Just because a campaign shows "Rest of Search" dominant doesn't mean product page traffic is zero
- Check placement report for actual distribution
- Product page placement CTR is typically lower than search placement CTR

**Example from Keto Analysis:**
- Auto campaign showed mostly "Rest of Search" placement
- But when filtered to product pages specifically, CTR was abysmal (0.1-0.2%)
- This was pulling down the overall average
- The search placement CTR was actually decent (1.5-2%)

---

## 3. COMMON ANALYTICAL MISTAKES

### Mistake 1: Daily-Level Pivot Analysis

**The Problem:**
```
Pivot Table Setup:
Rows: Targeting
Columns: Date
Filter: Spend > 0, Sales = 0
```

**Why This Is Wrong:**
- Groups by day, which fragments keyword performance
- A keyword might spend $5 with 0 sales on Day 1
- Same keyword might spend $3 with 2 sales on Day 2
- Daily pivot shows both as "problem" days
- Masks that keyword is actually profitable overall

**The Correct Approach:**
- Group by targeting at weekly or monthly level
- Use calculated fields for ROAS instead of separate columns
- Filter by total spend threshold (e.g., >$100 lifetime) before analyzing
- Check if targeting ever performed well historically before concluding it's bad

### Mistake 2: Not Checking Historical Performance

**The Problem:**
- Seeing a campaign with poor current performance
- Immediately pausing or concluding it should be paused
- Not checking if it ever performed well

**Why This Matters:**
- Amazon's metadata for campaigns includes historical performance
- If a campaign performed well before, Amazon "knows" it can work
- Current poor performance might be due to:
  - Budget constraints
  - Bid issues
  - Seasonal changes
  - Recent changes to listing
- Can often be fixed rather than paused

**The Correct Approach:**
1. Download lifetime data for the campaign
2. Check if there were periods of good performance
3. Analyze what was different during good periods
4. Try to recreate those conditions before giving up

### Mistake 3: Not Understanding Data Scope

**Critical Questions to Ask:**
- Is this campaign-level or search-term-level data?
- Is this targeting-level or match-type-level data?
- What time range am I actually looking at? (Amazon's date filters can be tricky)
- Am I comparing branded to non-branded properly?

---

## 4. PLACEMENT ANALYSIS FRAMEWORK

### Understanding Placement Performance

**Three Main Placements:**
1. **Top of Search** - First few results on search results page
2. **Rest of Search** - Lower positions on search results page
3. **Product Pages** - On competitor or related product detail pages

**Expected Performance Patterns:**
- Top of Search: Highest CTR, usually best CVR, highest cost
- Rest of Search: Medium CTR, good CVR, medium cost
- Product Pages: Lowest CTR, variable CVR, lowest cost

### When Product Page Performance Is Bad

**Analysis Steps:**
1. Check what products you're targeting (for product targeting campaigns)
2. Check what search terms trigger product page impressions (for auto campaigns)
3. Evaluate relevance - are you showing on relevant competitor pages?
4. Consider the comparison - if your packaging looks less appealing next to competitor, CTR will suffer
5. Check if product page CVR is actually good despite low CTR (sometimes happens)

**Example Pattern:**
- Campaign shows 40% rest of search, 5% product pages
- Overall CTR is 0.8%
- When you filter to just product pages: CTR is 0.1%
- Product pages are pulling down the average significantly
- But they're only 5% of volume
- Decision: Might be worth keeping if CPCs are low

### Placement Bidding Strategy

**When to Bid Up on Placements:**
- Good CTR and CVR on that placement
- Placement isn't saturating your budget
- ROAS is borderline but acceptable
- You want more volume at current efficiency

**When to Bid Down on Placements:**
- Poor CTR indicates low relevance
- Poor CVR despite good CTR
- ROAS is below target
- Placement is consuming too much budget

---

## 5. CAMPAIGN MANAGEMENT BEST PRACTICES

### Campaign Naming Convention

**Standard Format:**
```
MB-[Campaign Type]-[Keyword/Product]-[Match Type]-[Priority]
```

**Examples:**
- `MB-SP-Keyword-Exact-P0` - Sponsored Product, Keyword targeting, Exact match, Priority 0
- `MB-SP-Auto-P1` - Sponsored Product, Auto campaign, Priority 1
- `MB-SP-Product-Competitor-P0` - Sponsored Product, Product targeting, Competitor targeting

**Why This Matters:**
- Enables quick filtering and searching
- Makes bulk operations easier
- Clear hierarchy and organization
- "MB" prefix identifies Merchant Bot managed campaigns

### Budget Strategy

**Critical Understanding:**
Amazon's algorithm considers budget as a factor in impression allocation, not just as a cap.

**How It Works:**
1. Two advertisers with identical bids, CTR, CVR, and relevance
2. Advertiser A sets $10/day budget
3. Advertiser B sets $100/day budget
4. Amazon will favor Advertiser B because it can maximize spend

**Practical Application:**
- If campaign is performing at target ROAS but not getting volume
- Try increasing budget (even if not fully spending current budget)
- Example: $10 budget spending $5 → increase to $20, might spend $8
- Not proportional, but incremental gains

**Budget Setting Options:**
1. **Campaign-level daily budget** - Standard approach
2. **Portfolio-level budget** - Can set daily or lifetime across multiple campaigns
3. **Account-level daily budget** - Overall cap across all campaigns

**Client Budget Constraints:**
- If client says "don't exceed $25/day total," use account-level budget cap
- Portfolio budgets are risky (one campaign could spend entire budget in one day)
- Better to manage at campaign level with individual caps

### When to Close/Pause Campaigns

**Evaluation Framework:**
1. **Check lifetime performance** - Did it ever work well?
2. **Check spend threshold** - Has it spent enough to be statistically significant? (minimum $100-300)
3. **Check recent changes** - Did something change that caused decline?
4. **Check alternative placements** - Could it work better with different bid adjustments?
5. **Check search term report** - Are there salvageable keywords to move to exact campaigns?

**Don't Close If:**
- Low spend (<$100 lifetime) - not enough data
- Recently performed well (within last 90 days)
- Contains some profitable search terms
- Budget/bid constraints are causing poor performance

**Do Close If:**
- High spend (>$500) with consistently poor ROAS
- No profitable search terms at any time in history
- Targeting is fundamentally misaligned with product
- Better to reallocate budget to working campaigns

### Portfolio Organization

**When to Use Portfolios:**
1. Client manages multiple products, wants separate budgets per product
2. Need to set budget caps across related campaigns
3. Want grouped reporting for specific product lines
4. Testing different strategies on different product groups

**Structure Example:**
```
Portfolio: Allulose Products (Budget: $500/day lifetime)
├── MB-SP-Auto-Allulose
├── MB-SP-Keyword-Exact-P0-Allulose
├── MB-SP-Keyword-Broad-P1-Allulose
└── MB-SP-Product-Competitor-Allulose

Portfolio: Fiber Syrup Products (Budget: $200/day lifetime)
├── MB-SP-Auto-FiberSyrup
└── MB-SP-Keyword-Exact-P0-FiberSyrup
```

---

## 6. SEARCH TERM ANALYSIS

### Multi-Dimensional Analysis Required

When analyzing search term performance, need to split by multiple dimensions:

**Dimension 1: Campaign Type**
- Auto campaigns
- Manual exact campaigns
- Manual phrase campaigns
- Manual broad campaigns
- Product targeting campaigns

**Dimension 2: Targeting**
- Specific keyword or ASIN being targeted
- Match type (for keyword campaigns)

**Dimension 3: Search Term**
- Actual customer search query

**Why This Matters:**
Same search term can perform differently based on:
- Which campaign it came from (auto vs exact)
- Which targeting triggered it (close match vs exact)
- The landing page/ASIN it sent traffic to

**Example:**
- Search term "allulose syrup" in close match targeting: poor performance
- Same search term "allulose syrup" in exact match targeting: good performance
- Don't just pause the search term globally, analyze by targeting type

### Creating Search Term Reports

**Proper Pivot Structure:**
```
Rows: Campaign, Ad Group, Targeting, Search Term
Columns: (Calculated Fields for ROAS)
Values: Impressions, Clicks, Spend, Sales, Orders
Filters: Spend > $X (set threshold based on account size)
```

**Time Range Considerations:**
- For search terms: Maximum 65 days available
- For targeting: Lifetime data available
- Use longest range possible for statistical significance

### Identifying Winners and Losers

**Winner Criteria:**
- ROAS above target (typically 3+ for most accounts)
- Consistent performance over time
- Reasonable volume (at least 10-20 clicks to be meaningful)

**What to Do with Winners:**
1. Add exact match campaigns for the search term (if not already)
2. Increase bids if not reaching top of search
3. Check if running in all relevant campaigns
4. Consider phrase match expansion for related terms

**Loser Criteria:**
- ROAS below 1.0 (losing money)
- High spend (>$50) with poor performance
- No orders despite good CTR (relevance issue)

**What to Do with Losers:**
1. Add as negative match in relevant campaigns
2. Consider match type - exact negative or phrase negative
3. Check if it's a problem in all campaigns or just specific ones
4. Analyze if it's a placement issue rather than search term issue

---

## 7. MATCH TYPE STRATEGY

### When to Use Each Match Type

**Exact Match:**
- Proven performers from other campaigns
- High-intent keywords with clear commercial value
- Brand terms
- When you want maximum control over spend

**Phrase Match:**
- Tested keywords from exact that performed well
- When you want some variation but not too broad
- Category terms where word order matters
- Expanding from successful exacts

**Broad Match:**
- Discovery phase for new products
- Low competition keywords where traffic is limited
- When you need more volume and have good negatives list
- Products where you're confident in conversion regardless of variation

**When NOT to Use Broad/Phrase:**
- New products without conversion data
- High CPC categories where wasted spend is costly
- Products with narrow use cases
- When you don't have time to monitor and add negatives regularly

**Important Note:**
Just because competitors use broad/phrase doesn't mean you should. Consider:
- Their product might have wider appeal
- They might have more budget to waste on discovery
- Their conversion rate might support broader matching
- Each product's ideal match type mix is different

---

## 8. AUTO CAMPAIGN STRATEGY

### What Auto Campaigns Do

**Targeting Types in Auto:**
- Close Match - Keywords closely related to your product
- Loose Match - Keywords loosely related to your product
- Substitutes - Shows on competitor product pages
- Complements - Shows on complementary product pages

### Auto Campaign Analysis

**Key Questions:**
1. Which targeting type is spending most?
2. Which targeting type is most efficient?
3. What search terms are being discovered?
4. What products are we showing on?

**Common Patterns:**
- Close match often performs best for ROAS
- Loose match often provides highest volume
- Substitutes can work well for commodity products
- Complements are hit-or-miss depending on customer behavior

**Optimization Strategy:**
1. Let auto campaign run with moderate budget
2. Harvest winning search terms → move to exact campaigns
3. Add losing search terms as negatives
4. Adjust bids by targeting type based on performance
5. Keep running for ongoing discovery

### Auto Campaign Bid Adjustments

**By Targeting Type:**
You can bid up or down by targeting type:
- Close match: Often bid up 20-50%
- Loose match: Often bid down 20-50% or keep neutral
- Substitutes: Evaluate based on competitive landscape
- Complements: Usually bid down unless performing exceptionally

**By Placement:**
Combine targeting type bids with placement bids:
- If close match on top of search is working: bid up both
- If loose match on product pages is wasting spend: bid down both

---

## 9. PRODUCT TARGETING STRATEGY

### When to Use Product Targeting

**Good Use Cases:**
1. Clear competitor products that customers would cross-shop
2. Complementary products (buy X, might also need Y)
3. Alternative versions of your product (different size/flavor/brand)
4. Category-leading products where your product is a better alternative

**Bad Use Cases:**
1. Unrelated products hoping for impulse buys
2. Products significantly different in price point
3. Products in different categories/use cases
4. When you're not confident in your listing's competitive positioning

### Analyzing Product Targeting Performance

**Key Metrics:**
- ASIN-level ROAS
- CTR by ASIN (indicates how appealing your product looks in comparison)
- CVR by ASIN (indicates relevance of traffic)

**Common Issues:**
- Low CTR: Your product looks less appealing (image, price, reviews)
- High CTR but low CVR: Misleading click but not relevant
- High spend on one ASIN: May need to break out into separate campaign for bid control

**Example Analysis:**
If targeting competitor "FiberSyrup" product:
- Check: Is FiberSyrup actually a substitute or complement?
- Check: Do customers looking for FiberSyrup also want Allulose?
- Check: Is our product positioned correctly for this audience?
- Decision: Even if some sales, might not be worth it if ROAS is low

---

## 10. TECHNICAL NUANCES

### Amazon Reporting Limitations

**Date Range Limits:**
- Search Term Report: 65 days maximum
- Targeting Report: Lifetime available
- Placement Report: 65 days maximum
- Campaign Report: Lifetime available

**Data Export Issues:**
- Selecting date ranges can be tricky (Amazon's UI is inconsistent)
- "Last 65 days" might not actually be 65 days
- Always verify date range in downloaded file
- Gap in data is common (Amazon processing delays)

### Metabase Query Optimization

**When querying ad data:**
1. Always filter by time range first (performance)
2. Filter by campaign/product before aggregating
3. Use portfolio/campaign ID filters when possible
4. Be aware that daily data creates large result sets

**Best Practices:**
- Download and work in Excel/Sheets for pivot analysis
- Metabase for quick checks and filtering
- Excel for detailed multi-dimensional analysis
- Always verify your date range matches what you think it is

### Cross-Campaign Analysis

**What to Look For:**
1. Same search term performing differently across campaigns
2. Budget constraints preventing scaling of winners
3. Cannibalization between campaigns
4. Overlap in targeting causing bid competition with yourself

**Example:**
- "allulose" in exact match campaign: $2 CPC, good ROAS
- "allulose" in auto close match: $3 CPC, poor ROAS
- Action: Add "allulose" as exact negative in auto campaign
- Result: Exact campaign gets all traffic at lower CPC

---

## 11. CLIENT COMMUNICATION

### Setting Expectations

**Be Honest About Uncertainties:**
- "This is my hypothesis, but we need to test to confirm"
- "The data suggests X, but there could be other factors"
- "I don't have enough data to make a definitive conclusion yet"

**Don't Make Definitive Statements Without Evidence:**
- "Your CTR is too low, that's the problem"
- "Your CTR on product pages is lower than average, which could be one contributing factor"

**Provide Context:**
- Compare to industry benchmarks when available
- Explain why certain metrics might differ by product type
- Show historical trends, not just current snapshot

### Presenting Analysis

**Structure:**
1. **What I looked at**: Data sources, time ranges, filters
2. **What I found**: Key patterns and metrics
3. **What it means**: Interpretation with caveats
4. **What I recommend**: Specific actions with expected outcomes
5. **What I'm not sure about**: Open questions and limitations

**Example:**
> "I analyzed the last 60 days of campaign data, focusing on CTR across different placements. I found that your overall CTR of 0.8% is being pulled down by product page placements (0.1% CTR), while your search placements are performing at 1.7% CTR which is closer to industry average.
>
> This suggests the issue isn't with your creative or listing content, but rather with where your ads are being shown. Product page placements are lower intent by nature.
>
> I recommend we adjust placement bids to reduce product page exposure and focus budget on search placements. We should also review which specific products we're showing on to ensure relevance.
>
> What I'm still investigating: Why the auto campaign is getting so much product page traffic compared to manual campaigns, and whether those specific product targets are valuable despite low CTR."

---

## 12. DECISION FRAMEWORKS

### When Analyzing a New Account

**Step 1: Get the Lay of the Land (15-30 min)**
- How many products?
- How many campaigns?
- What's the overall spend and ROAS?
- What's the organic vs paid split?
- Any obvious issues? (campaigns paused, out of budget, etc.)

**Step 2: Identify Top Performers (15 min)**
- Which campaigns drive most sales?
- Which campaigns have best ROAS?
- Which search terms/products drive most volume?
- Are there any clear winners to scale?

**Step 3: Identify Problem Areas (30 min)**
- Which campaigns are losing money?
- Where is budget being wasted?
- What's not getting volume despite good ROAS?
- Any campaigns that haven't spent at all?

**Step 4: Form Hypotheses (15 min)**
- Why are winners winning?
- Why are losers losing?
- What are the quick wins?
- What needs deeper investigation?

**Step 5: Prioritize Actions (15 min)**
- What will have biggest impact?
- What can be done immediately?
- What needs client approval?
- What needs more data/time?

### When Deciding to Scale or Cut

**Scale If:**
- Consistently good ROAS over 30+ days
- Reasonable volume (not just 1-2 lucky orders)
- Budget isn't fully utilized (room to grow)
- Not cannibalizing other campaigns
- Client has capacity for more orders

**Cut If:**
- Consistently poor ROAS over 30+ days
- High spend (sufficient data to judge)
- No improving trend
- Budget needed for better performers
- Can't identify fixable issue

**Hold/Optimize If:**
- Recent changes need time to show effect
- Borderline performance that could be improved
- Seasonal considerations
- Testing in progress
- Need more data

---

## 13. COMMON PITFALLS TO AVOID

### Pitfall 1: Over-Optimization
- Making too many changes at once
- Not giving changes time to show effect
- Optimizing campaigns with insufficient data
- Chasing daily fluctuations

### Pitfall 2: Analysis Paralysis
- Spending too much time analyzing, not enough testing
- Waiting for "perfect" data before making decisions
- Over-complicating what should be simple decisions

### Pitfall 3: Following "Best Practices" Blindly
- Every account is different
- What works for one product/category might not work for another
- Test and validate rather than assume

### Pitfall 4: Ignoring the Product
- Bad listings can't be fixed with ad optimization alone
- If organic sales are strong but paid is weak, it's often a bidding/placement issue
- If both organic and paid are weak, it's likely a listing issue

### Pitfall 5: Not Checking Your Work
- Always verify your pivot table groupings
- Always verify your date ranges
- Always verify your filters
- Spot check a few data points manually to ensure calculations are correct

---

## 14. FINAL PRINCIPLES

1. **Data tells a story, but you need to know how to read it**
   - Same metric can mean different things in different contexts
   - Always dig multiple levels deep before concluding

2. **Amazon's platform has quirks**
   - Reports don't always align perfectly
   - Date ranges can be misleading
   - Different reports capture different things
   - Learn these quirks to avoid misinterpretation

3. **Every product is unique**
   - Industry benchmarks are guidelines, not rules
   - Customer behavior varies by category
   - What works for commodity products might not work for niche products

4. **Optimization is iterative**
   - Test, measure, learn, repeat
   - Don't expect perfect results immediately
   - Small improvements compound over time

5. **Communication is as important as analysis**
   - Clients need to understand your reasoning
   - Be honest about uncertainties
   - Set realistic expectations
   - Provide context with every recommendation

---

## APPENDIX: Quick Reference Checklists

### CTR Issue Investigation Checklist
- [ ] Check industry benchmark
- [ ] Check account-level CTR
- [ ] Check branded vs non-branded split
- [ ] Check placement-specific CTR
- [ ] Check SQP vs Ads report CTR
- [ ] Check campaign-type specific CTR
- [ ] Check historical trends
- [ ] Form hypothesis with evidence

### Campaign Performance Review Checklist
- [ ] Lifetime data reviewed
- [ ] Placement distribution checked
- [ ] Search term performance analyzed
- [ ] Budget utilization checked
- [ ] Historical performance reviewed
- [ ] Comparison to similar campaigns
- [ ] ROAS trend over time
- [ ] Specific issues identified with evidence

### New Campaign Setup Checklist
- [ ] Naming convention followed (MB-XX-XX-XX)
- [ ] Portfolio assigned (if applicable)
- [ ] Budget set appropriately
- [ ] Targeting set up correctly
- [ ] Bids set at reasonable starting point
- [ ] Negative keywords added (if applicable)
- [ ] Tracking/monitoring plan in place

### Data Analysis Checklist
- [ ] Date range verified
- [ ] Grouping levels appropriate for question
- [ ] Filters documented
- [ ] Calculations verified with spot checks
- [ ] Edge cases considered
- [ ] Statistical significance considered
- [ ] Alternative explanations explored
- [ ] Conclusions supported by evidence

---

**Document Version:** 1.0
**Last Updated:** 2026-01-28
**Source:** Keto Goods Analysis Session
**Usage:** Reference document for Amazon advertising account management and analysis
