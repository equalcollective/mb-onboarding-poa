# Common Knowledge Base

> Distilled wisdom synthesized from all knowledge sources.
> This is the "always reference" document for core principles.

**Last Synthesized:** 2026-01-29
**Source Documents:** 6

---

## Amazon Platform Fundamentals

### Amazon Hierarchy Structure
```
Brand
  └── Parent ASIN (backend only, not visible to customers)
      └── Child ASIN (individual product page with reviews, images, price)
          └── SKU (seller's internal inventory identifier)
```

**Key Points:**
- **Child ASIN** = The product page customers see (Amazon controls)
- **Parent ASIN** = Backend grouping for variants (colors, sizes, flavors)
- **SKU** = Seller-controlled internal tracking
- Multiple sellers can sell the same ASIN
- Only brand owners have access to Brand Analytics

**Sources:** [amazon-marketing-fundamentals-29jan-training](./amazon-marketing-fundamentals-29jan-training.md)

### Buy Box Essentials
The buy box ("Add to Cart" button) determines who gets the sale when multiple sellers offer the same product.

**Priority Factors:**
1. Price (competitive, not necessarily lowest)
2. Fulfillment speed (FBA strongly favored)
3. Seller rating
4. Inventory availability

**Critical Rule:** Cannot run sponsored product ads without buy box. "Ineligible" often means buy box issue.

**Sources:** [amazon-marketing-fundamentals-29jan-training](./amazon-marketing-fundamentals-29jan-training.md)

---

## Universal Principles

### 1. Never Conclude Without Context
Same metric can mean different things in different contexts. Always establish benchmarks, historical trends, and segmentation before diagnosing.

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### 2. Data Sources Capture Different Things
Different Amazon reports (SQP, Ads, Business) capture different slices of data. Understanding what each report includes/excludes is critical to avoid misinterpretation.

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md), [amazon-marketing-fundamentals-29jan-training](./amazon-marketing-fundamentals-29jan-training.md)

### 3. Historical Performance Matters
Before pausing or cutting anything, check if it ever performed well. Amazon's algorithm remembers past performance.

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### 4. Optimization is Iterative
Test, measure, learn, repeat. Small improvements compound over time. Don't expect perfect results immediately.

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### 5. Ad Algorithm: Relevance → Performance → Bid
Amazon's ad algorithm prioritizes in this order:
1. **Relevance matching** - Does product match user intent?
2. **Performance factors** - Conversion rate, reviews, price competitiveness
3. **Bid amount** - Among similar performers, higher bid wins

**Critical Rule:** High bids cannot overcome poor conversion. Algorithm protects customer experience.

**Sources:** [amazon-marketing-fundamentals-29jan-training](./amazon-marketing-fundamentals-29jan-training.md)

### 6. Fix Conversion Before Increasing Traffic
Work backwards through the funnel: Conversion → CTR → Impressions.

**Critical Logic:**
- If CVR is low, getting more clicks will only INCREASE losses
- Fix conversion (listing, reviews, positioning) FIRST
- THEN work on getting more traffic

**Exception:** If you don't have enough data, you may need more clicks first - but only LEGITIMATE clicks from relevant, high-intent keywords.

**Sources:** [amazon-account-analysis-framework-29jan-training](./amazon-account-analysis-framework-29jan-training.md)

---

## Core Frameworks

### CTR Diagnosis Framework
**Use when:** CTR appears low and you need to determine root cause

1. Check industry/category benchmark
2. Check account-level CTR trends
3. Check branded vs non-branded split
4. Check placement-specific CTR (search vs product pages)
5. Check campaign-type breakdown
6. Check historical trends
7. THEN evaluate creative if still unexplained

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### New Account Analysis Framework
**Use when:** First looking at any new account

1. **Lay of the Land (15-30 min):** Products, campaigns, spend, ROAS, organic/paid split
2. **Top Performers (15 min):** Best campaigns, best ROAS, top search terms
3. **Problem Areas (30 min):** Losing money, wasted budget, low volume
4. **Form Hypotheses (15 min):** Why winners win, why losers lose
5. **Prioritize Actions (15 min):** Biggest impact, immediate vs needs approval

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### Scale or Cut Decision Framework
**Use when:** Deciding whether to increase budget or pause a campaign

**Scale if:** Good ROAS 30+ days, reasonable volume, budget room, no cannibalization
**Cut if:** Poor ROAS 30+ days, high spend, no improving trend, no fixable issue
**Hold if:** Recent changes need time, borderline performance, testing in progress

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### Campaign Structure: 1-1-1 Rule
**Use when:** Setting up or restructuring campaigns

**Recommended Setup:**
```
1 Portfolio → 1 Campaign → 1 Ad Group → 1 Parent ASIN
```

**Benefits:**
- Independent budget per product
- Isolated performance tracking
- Clear cause-effect relationships

**Why Not Multiple Products Per Campaign:**
- Amazon picks favorites
- Some products never get tested
- Can't test different products fairly
- Lose granular control

**Targeting Best Practice:** Maximum 5 keywords per campaign (fewer is better)

**Sources:** [amazon-marketing-fundamentals-29jan-training](./amazon-marketing-fundamentals-29jan-training.md)

### Multi-Layer CVR Analysis
**Use when:** Diagnosing conversion rate issues

Analyze at multiple levels to find the bottleneck:
1. **Overall Account CVR** - Baseline performance
2. **Ad-Level CVR** - Performance when ads drive traffic
3. **Branded vs Non-Branded CVR** - Brand strength vs. category competitiveness
4. **Campaign-Specific CVR** - Performance by product/category

**Key Diagnostic:** If branded CVR is acceptable but non-branded is poor → listing/positioning issue, not traffic quality

**Decision Tree:**
```
If overall CVR strong but ad CVR weak:
  → Traffic quality issue

If branded CVR strong but non-branded weak:
  → Product differentiation or listing issue

If all CVRs are low:
  → Fundamental product-market fit problem
```

**Sources:** [amazon-account-analysis-framework-29jan-training](./amazon-account-analysis-framework-29jan-training.md), [amazon-account-analysis-methodology-29jan-call](./amazon-account-analysis-methodology-29jan-call.md)

### Test Budget Calculation
**Use when:** Planning budget for testing a product

**Click Volume by Price Point:**

| Product Price | Clicks Needed |
|---------------|---------------|
| <$15 | 50-100 clicks |
| $15-$50 | 100-200 clicks |
| $50-$80 | 200-300 clicks |
| >$80 | 300-500 clicks |

**Formula:**
```
Test Budget = Required Clicks × Average CPC
If (Expected Orders × Product Price) < Test Budget → You'll lose money
```

**Sources:** [amazon-account-analysis-framework-29jan-training](./amazon-account-analysis-framework-29jan-training.md)

### Quick Profitability Check
**Use when:** Need to quickly assess if account is profitable

**Formula:**
```
ACOS = (Ad Spend / Ad Sales) × 100
Revenue after COGS = Sales × (1 - COGS%)
Revenue after COGS and Fees = Sales × (1 - COGS% - 15%)
Profit = Revenue after COGS and Fees - Ad Spend
```

**Why It Matters:**
- Profitable → Room for testing and experimentation
- Unprofitable → Need quick wins, less room for tests
- Determines client patience level

**Sources:** [amazon-account-analysis-methodology-29jan-call](./amazon-account-analysis-methodology-29jan-call.md)

### Portfolio Budget Allocation
**Use when:** Deciding which products should get budget priority

**For each product, assess:**
1. Current profitability
2. Market size/opportunity
3. Competitive intensity
4. Unique differentiation

**Allocation Priority:**
1. **Profitable + unique products** → Defend and scale
2. **Large opportunity + fixable issues** → Invest to fix
3. **Small opportunity or unfixable issues** → Maintain or sunset

**Sources:** [amazon-account-analysis-methodology-29jan-call](./amazon-account-analysis-methodology-29jan-call.md)

### ICAP Funnel Analysis
**Use when:** Analyzing keyword-level conversion performance using SQP data

**The ICAP Funnel:**
```
Impressions → Clicks → Add to Carts → Purchases
```

**The Primary Signal:**
Compare **Click Share %** to **Impression Share %** for each keyword.

| Comparison | Meaning | Action |
|------------|---------|--------|
| Click Share > Impression Share | Product MORE relevant than competitors | Invest more, scale ads |
| Click Share < Impression Share | Product LESS appealing when shown | Fix CTR (image, title, price) |
| Click Share ≈ Impression Share | At market average | Check ATC and Purchase rates |

**Bottleneck Fixes:**

| Stage | Problem | Fix |
|-------|---------|-----|
| Impressions | Low visibility | Improve indexing, increase ad spend |
| CTR | Low clicks | Optimize main image, title, price |
| Add to Cart | Low ATC | Improve secondary images, create urgency |
| Purchase | Low conversion | Add social proof, clarify details |

**Inverted Funnel Opportunity:**
When ATC or Purchase rate EXCEEDS CTR → product is highly desirable once discovered. Priority: fix CTR to unlock scale.

**Sources:** [icap-funnel-methodology-29jan-training](./icap-funnel-methodology-29jan-training.md)

### PPC Iteration Methodology (Bulking & Cutting)
**Use when:** Managing ongoing PPC optimization cycles

**Core Concept:** Optimization is like adjusting a shower handle—continually tweaking until performance is right.

**The Bulking Phase (Scale & Discover):**
- Increase bids and budgets
- Launch new keywords
- Expect: spend ↑, revenue ↑, profit may temporarily ↓

**The Cutting Phase (Lean Out & Profit):**
- Lower bids on non-performers
- Add negative keywords
- Expect: spend ↓, revenue may ↓ slightly, profit ↑

**The Cycle:**
```
Bulk → Gather Data → Analyze → Cut Non-Performers → Stabilize → Bulk Again
```

**Sources:** [amazon-ppc-iteration-methodology-29jan-training](./amazon-ppc-iteration-methodology-29jan-training.md)

### Wasted Spend Threshold (Negative Keywords)
**Use when:** Deciding whether to add a search term as negative

**Rule:** If a search term reaches **75% of product price** with **zero sales**, add it as a negative keyword.

| Product Price | Negative Threshold |
|---------------|-------------------|
| £20 | £15 with zero sales |
| £40 | £20-£30 with zero sales |
| £100 | £75 with zero sales |

**Critical Warning:** Do NOT negative a term in discovery campaigns just because you moved it to Exact—let both run until new campaign proves performance.

**Sources:** [amazon-ppc-iteration-methodology-29jan-training](./amazon-ppc-iteration-methodology-29jan-training.md)

### Seven CTR/CVR Factors
**Use when:** Preparing a listing before scaling ads

Before scaling ad spend, optimize these factors that determine Click-Through Rate and Conversion Rate:

| Factor | Impact |
|--------|--------|
| 1. Main Image | Clean, attractive, stands out in search |
| 2. Title | Clear, satisfies search intent, front-loaded |
| 3. Star Rating / Reviews | 4.2→4.3 jump can drastically increase revenue |
| 4. Coupons | Green/blue badge attracts attention |
| 5. Badges | Bestseller, Amazon's Choice build trust |
| 6. Price | Find the CVR/profit balance point |
| 7. Shipping Speed | FBA/Prime essential in most categories |

**Key Principle:** Ads amplify what already exists—good or bad. Fix these before scaling.

**Sources:** [amazon-ppc-iteration-methodology-29jan-training](./amazon-ppc-iteration-methodology-29jan-training.md)

---

## Essential Checklists

### Before Analyzing Any Campaign
- [ ] Verify date range is correct
- [ ] Check lifetime performance (not just recent)
- [ ] Understand placement distribution
- [ ] Separate branded vs non-branded
- [ ] Check if budget/bids are constraining

### Before Making Bid/Budget Changes
- [ ] Sufficient data to judge (min $100-300 spend)
- [ ] Checked historical performance
- [ ] Understand placement impact
- [ ] Have hypothesis for why current state exists

### Before Presenting to Client
- [ ] Cite specific data points
- [ ] Acknowledge uncertainties
- [ ] Provide context (benchmarks, history)
- [ ] Clear recommendations with expected outcomes

---

## Documentation Best Practice

### Problem-Analysis-Action Format
When documenting findings, use three-column structure:

| Problem Identified | Analysis/Evidence | Proposed Action |
|-------------------|-------------------|-----------------|
| Low non-branded CVR (6%) | Review issues, price positioning | Fix reviews, improve A+ content |
| High CPC ($2.37) | Auto campaigns in competitive category | Test manual campaigns |

**Benefits:**
- Easy to explain thought process in meetings
- Clear connection between problem → analysis → solution
- Creates reference for future discussions
- Helps team members learn analytical framework

**Sources:** [amazon-account-analysis-methodology-29jan-call](./amazon-account-analysis-methodology-29jan-call.md)

---

## Key Metrics & Benchmarks

| Metric | Good | Average | Poor | Notes |
|--------|------|---------|------|-------|
| CTR (Top of Search) | >2% | 1-2% | <1% | Category dependent |
| CTR (Product Pages) | >0.5% | 0.2-0.5% | <0.2% | Always lower than search |
| ACOS (Brand Defense) | <15% | 15-25% | >25% | Should be very efficient |
| ACOS (SP Auto) | <30% | 30-40% | >40% | Discovery, can run higher |
| ACOS (SP Exact) | <25% | 25-35% | >35% | Highest intent |
| Min spend for decision | $100-300 | - | - | Statistical significance |
| Review Rating | >4.0 | 3.5-4.0 | <3.5 | Below 4.0 hurts conversion significantly |
| Review Count | >100 | 50-100 | <50 | Low count impacts trust in competitive categories |

**Sources:** [amazon-account-analysis-framework-29jan-training](./amazon-account-analysis-framework-29jan-training.md)

---

## Common Pitfalls

| Pitfall | Why It's Wrong | What To Do Instead |
|---------|----------------|-------------------|
| Daily-level pivot analysis | Fragments performance, hides overall trends | Group by week/month minimum |
| Concluding without checking history | Misses that campaign worked before | Always check lifetime data |
| Comparing CTR across categories | Different products have different expectations | Use category benchmarks |
| Pausing low-spend campaigns | Not enough data to judge | Wait for $100+ spend |
| Ignoring placement mix | Product pages drag down averages | Segment by placement |
| Increasing CTR when CVR is low | Getting more clicks increases losses | Fix conversion first, then traffic |
| Comparing prices without unit conversion | Misleading competitive analysis | Always calculate price per unit |
| Auto campaigns for expensive products | Too broad, wastes budget | Use manual campaigns with precise targeting |
| Ignoring recent negative reviews | Indicates ongoing fixable issues | Audit 1-star reviews for patterns |
| Controlling spend via budget caps | Creates artificial ceilings during peak hours | Control via bids instead, set high budgets |
| Pre-negating graduated keywords | Kills existing performance that may not replicate | Let both campaigns run until new proves performance |
| Too many keywords per campaign | One term steals budget from others | Max 5 keywords per campaign |

**Sources:** [amazon-account-analysis-framework-29jan-training](./amazon-account-analysis-framework-29jan-training.md), [amazon-ppc-iteration-methodology-29jan-training](./amazon-ppc-iteration-methodology-29jan-training.md)

---

## Report Interpretation Quick Reference

| Report | Includes | Does NOT Include |
|--------|----------|------------------|
| SQP (Search Query Performance) | Search-based traffic only | Product page impressions |
| Ads/Search Term Report | All ad impressions including product pages | Organic traffic |
| Business Report | All traffic (organic + paid) | Ad-specific metrics |
| Placement Report | Placement distribution | Search term details |

**SQP 24-Hour Attribution:** Only counts actions within 24 hours of search. Products with longer consideration windows show lower conversion rates.

**Amazon Week Definition:** Sunday to Saturday (US), Monday to Sunday (UK). Use Sunday-Saturday consistently.

---

## Red Flags to Watch

| Red Flag | Likely Issue |
|----------|--------------|
| ACoS >50% (unintentional) | Targeting or bid problem |
| Zero impressions on active products | Eligibility issue (buy box, listing) |
| High CTR + low conversion | Listing problem |
| One product dominating multi-product campaign | Need to split campaigns |
| Irrelevant search terms driving clicks | Need negative keywords |
| Ad marked "ineligible" | Buy box, stock, or listing suppression |

**Sources:** [amazon-marketing-fundamentals-29jan-training](./amazon-marketing-fundamentals-29jan-training.md)

---

## Amazon Fee Awareness

**Main Fee Types:**
- **Referral Fee:** 5-15% of sale price (category dependent)
- **FBA Fulfillment:** Pick, pack, ship (varies by size/weight)
- **Storage:** Free first 90 days, then monthly fees (escalate long-term)

**When Analyzing Profitability:** Account for all fees, not just referral fee.

**Sources:** [amazon-marketing-fundamentals-29jan-training](./amazon-marketing-fundamentals-29jan-training.md)

---

*This document is automatically updated as new knowledge is ingested.*
