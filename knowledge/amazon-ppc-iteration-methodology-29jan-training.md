# Amazon PPC Iteration Methodology

**Source:** Training Documentation
**Date:** 2026-01-29
**Tags:** `PPC`, `advertising`, `campaigns`, `iteration`, `optimization`, `search-terms`, `negatives`, `placements`, `structure`, `CTR`, `CVR`

---

## Document Purpose

Strategic framework for Amazon PPC management based on visibility, data-driven iteration, and strict account structure. The core philosophy is that advertising provides visibility, but listing quality determines conversion.

---

## 1. Core Philosophy

### The Goal of Amazon Ads

**Identify and capture every single search term and product page** where your target audience exists.

**Key Principle:** Advertising provides visibility; however, the listing's quality (images, reviews, price) determines whether that visibility converts into sales.

**Implication:** Before scaling ads, ensure the listing is optimized for conversion. Ads amplify what already exists—good or bad.

---

## 2. Account Hygiene and Structural Organisation

### 2.1 Portfolio Management

**Purpose:** Organize campaigns for easy filtering and accurate performance analysis.

**Rule:** Group all campaigns for a single **parent ASIN** into one portfolio.

**Why This Matters:**
- Prevents "delusional" data analysis
- Stops misattribution of sales from one variation to another
- Enables accurate product-level performance tracking
- Creates clean aggregated views

### 2.2 Naming Conventions

**Purpose:** Enable dashboard filtering and quick performance analysis.

**Recommended Format:**
```
[Product Code] - [Ad Type] - [Ad Subtype] - [Special Identifiers]
```

**Example:**
| Product | Code | Campaign Name Example |
|---------|------|----------------------|
| Hardwork Unflavored | HU | HU - SP - Exact - Brand Terms |
| Hardwork Mango Pineapple | HMP | HMP - SP - Auto - Discovery |

**Benefit:** Filtering by product code (e.g., "HU") instantly shows aggregated performance for that specific product line.

---

## 3. Campaign Architecture and Control

### 3.1 The One-Campaign-One-Ad-Group Structure

**Rule:** Use a **"one campaign, one ad group"** structure with **no more than five keywords** per campaign.

**Why:**
- Budget flows directly to intended keywords
- Prevents one high-volume term from "stealing" entire budget
- Enables precise performance tracking
- Creates clear cause-effect relationships

**This aligns with the 1-1-1 Rule:** 1 Portfolio → 1 Campaign → 1 Ad Group → 1 Parent ASIN

### 3.2 Budget vs. Bid Control

**Critical Distinction:** Control spend at the **keyword/bid level**, NOT the budget level.

**Process:**
1. Set high daily budgets to avoid artificial ceilings
2. Control spend by starting with very low bids (e.g., £0.01)
3. "Inch" bids up gradually until impressions begin
4. Let bid levels determine actual spend

**Why:** Budget caps can prevent winning placements during peak hours. Bid control gives precision without artificial constraints.

### 3.3 Match Type Separation

**Rule:** Use all match types (Broad, Phrase, Exact) simultaneously, but keep them in **separate campaigns**.

**Rationale:**
- Different match types perform differently
- Each captures different search segments
- Prevents broad terms from consuming entire budget
- Enables match-type-specific optimization

**Structure Example:**
```
[Product] - SP - Broad - Category Terms
[Product] - SP - Phrase - Category Terms
[Product] - SP - Exact - Category Terms
```

---

## 4. The Iteration Thought Process

### 4.1 The Core Concept

**Iteration = Continuous cycle of adjustment**

Optimization is like adjusting a shower handle—continually tweaking until the "temperature" (performance) is right.

**Key Mindset:**
- No "set and forget"
- Expect constant adjustment
- Small changes, measured results
- Learn from each iteration

### 4.2 The Bulking Phase

**Purpose:** Scale and discover.

**Actions:**
- Increase bids across campaigns
- Increase budgets
- Launch new keywords
- Expand targeting

**Expected Outcomes:**
- Ad spend increases
- Revenue increases
- Profit may temporarily decrease
- Data volume increases

**When to Bulk:**
- Testing new products/keywords
- Seasonal opportunities
- Gaining market share
- Building ranking momentum

### 4.3 The Cutting Phase

**Purpose:** Lean out and increase profit.

**Actions:**
- Lower bids on non-performers
- Add negative keywords
- Pause underperforming campaigns
- Eliminate wasted spend

**Expected Outcomes:**
- Ad spend decreases
- Revenue may decrease slightly
- Profit increases
- ACOS improves

**When to Cut:**
- After sufficient data gathered
- When ACOS exceeds targets
- When scaling shows diminishing returns
- To improve profitability

### 4.4 The Cycle

```
Bulk → Gather Data → Analyze → Cut Non-Performers → Stabilize → Bulk Again
```

This is not linear—expect to move between phases based on data.

---

## 5. Data-Driven Optimization: Search Term Audits

### 5.1 The Binary Decision Framework

Use Search Term Reports (STR) and Bulk Sheets to make **binary decisions** based on performance thresholds.

### 5.2 Wasted Spend Rule (Adding Negatives)

**Threshold Rule:** If a search term reaches **75% of the product price** with **zero sales**, add it as a negative keyword.

**Examples by Price Point:**

| Product Price | Spend Threshold for Negative | Logic |
|---------------|------------------------------|-------|
| £20 | £15 with zero sales | 75% of price |
| £40 | £20-£30 with zero sales | 75% of price |
| £100 | £75 with zero sales | 75% of price |

**Implementation:**
1. Export Search Term Report
2. Filter for terms with spend > threshold AND sales = 0
3. Add as negative keywords (exact match)
4. Document for future reference

### 5.3 Profitable Harvesting Rule

**Process:** Identify terms in Broad, Phrase, or Auto campaigns that convert at target ACOS (e.g., under 35%) and "graduate" them to their own Exact match campaigns.

**Steps:**
1. Export Search Term Report
2. Filter for terms with ACOS < target threshold
3. Create dedicated Exact match campaign
4. Set appropriate bid based on historical CPC

**CRITICAL WARNING:**
> **Do NOT negative a term in the original discovery campaign just because you moved it to an Exact campaign.**

**Why:** This can kill existing performance that is not guaranteed to replicate in the new setup. Let both run until the Exact campaign proves it can perform.

---

## 6. Pre-Advertising Preparation: The Seven CTR/CVR Factors

Before scaling ads, optimize these seven factors that determine Click-Through Rate and Conversion Rate:

### Factor 1: Main Image
- Must be clean and attractive
- Consider 3D renders for professional appearance
- Stand out in search results
- Show product clearly

### Factor 2: Title
- Must be clear and satisfy search intent
- Include primary keywords
- Communicate key benefits
- Front-load important information

### Factor 3: Star Rating / Review Count
- Critical conversion factor
- Jump from 4.2 to 4.3 stars can drastically increase revenue
- Low review count (<100) hurts trust in competitive categories
- Prioritize review acquisition strategies

### Factor 4: Coupons
- Green/blue coupon badge attracts attention
- Increases CTR from search results
- Test different discount levels
- Consider impact on margins

### Factor 5: Badges
- "Bestseller" badge significantly increases CTR
- "Amazon's Choice" badge builds trust
- Earned through performance + relevance
- Worth optimizing toward

### Factor 6: Price
- Test different price points
- Find the "bell curve" where profit and CVR are balanced
- Too high = low CVR
- Too low = low profit despite high CVR
- Category-relative positioning matters

### Factor 7: Shipping Speed
- FBA (Fulfilled by Amazon) strongly preferred
- Customers rarely choose items that take a week over overnight alternatives
- Prime badge essential in most categories
- Fast shipping = higher CVR

---

## 7. Advanced Scaling: Placement Optimization

### 7.1 Analyzing Placement Performance

**Process:**
1. Check campaign performance data by placement
2. Compare **Top of Search** vs. **Product Pages** vs. **Rest of Search**
3. Identify where ROAS is highest

**Common Pattern:** Top of Search often has significantly higher ROAS than other placements.

### 7.2 Placement Bid Adjustments

**When Top of Search outperforms:**
- Apply percentage bid adjustment (e.g., +30% to +100%)
- Ensures ads appear more often in high-performing placement
- Start conservative, increase based on results

**When Product Pages underperform:**
- Consider negative adjustment or accept lower bids
- Different intent on product pages vs. search
- May need different strategy for each

### 7.3 Product-Market Fit and Premium Pricing

**Key Insight:** A product can charge a premium price even with fewer reviews if it solves a specific problem better than competitors.

**Example:**
- Electronic drum set charging premium price
- Fewer reviews than competitors
- BUT: Significantly quieter (18dB vs 60dB)
- Solves specific problem for target audience (parents)
- Premium justified by unique value proposition

**Implication:** Understand your product's unique value and target the audience that values it.

---

## 8. Implementation Checklist

### Before Launching Campaigns
- [ ] Listing optimized for all 7 CTR/CVR factors
- [ ] Portfolio structure planned
- [ ] Naming convention established
- [ ] Initial keyword list researched
- [ ] Target ACOS defined

### Campaign Setup
- [ ] One campaign per match type
- [ ] Maximum 5 keywords per campaign
- [ ] Low starting bids set
- [ ] High daily budgets set
- [ ] Negatives from research added

### Weekly Optimization
- [ ] Search Term Report exported
- [ ] Wasted spend terms identified
- [ ] Negative keywords added
- [ ] Profitable terms identified for graduation
- [ ] Bids adjusted based on performance
- [ ] Placement performance checked

### Monthly Review
- [ ] Bulking vs. Cutting phase decision
- [ ] Overall ACOS vs. target
- [ ] Campaign structure cleanup
- [ ] New keyword opportunities identified
- [ ] Listing optimization needs assessed

---

## 9. Key Principles Summary

| Principle | Application |
|-----------|-------------|
| Visibility ≠ Sales | Ads provide visibility; listing quality converts |
| Control via Bids | Use bids, not budgets, to control spend |
| Structure for Analysis | Portfolios + naming = accurate data |
| Match Type Separation | Prevent budget cannibalization |
| Iterate Continuously | Bulk → Analyze → Cut → Repeat |
| Data-Driven Negatives | 75% of price with zero sales = negative |
| Don't Pre-Negative Graduates | Let new Exact campaigns prove themselves |
| Optimize Before Scaling | Fix 7 factors before increasing spend |
| Placement Matters | Adjust bids based on placement performance |

---

**Document Version:** 1.0
**Last Updated:** 2026-01-29
**Usage:** Strategic framework for Amazon PPC campaign management and optimization
