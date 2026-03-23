# Ad Analysis - Magic AutoCare

**Date:** 2026-03-22
**Analysis Period:** Dec 7, 2025 - Feb 28, 2026 (83 days)
**Total Account Ad Spend:** $17,813
**Total Account Ad Sales:** $22,633

## Account-Level Analysis

### Auto vs Manual Split

| Targeting Type | Clicks | Spend | Sales | ROAS | AOV | CPC | CVR |
|----------------|--------|-------|-------|------|-----|-----|-----|
| Automatic | 4,087 | $3,557 | $4,813 | 1.35 | $43.36 | $0.87 | 2.72% |
| Manual | 10,253 | $14,255 | $17,820 | 1.25 | $45.58 | $1.39 | 3.81% |

Manual drives 80% of spend, which is the healthy pattern. However, auto has better ROAS (1.35 vs 1.25) despite lower CVR (2.72% vs 3.81%). This is because auto's CPC ($0.87) is significantly lower than manual's ($1.39). Manual campaigns are converting at a better rate but overpaying for those clicks.

### Campaign Profitability

**Finding: $7,600+ in ad spend on non-P0 products at sub-1.0 blended ROAS**

**Problem:**
- P1 (Diamond AI Coating) campaigns: $3,632 combined spend, blended ROAS ~0.87
- P2 (Graphene Coating for Cars) campaigns: $3,150 combined spend, blended ROAS ~0.76
- P3 (Ceramic Coating PRO 10H) campaigns: $1,729 combined spend, blended ROAS ~0.72

These three products consumed $8,511 in ad spend (48% of total) while generating only $6,820 in ad sales. Net loss: ~$1,691 on ad spend alone, before accounting for product costs.

**Solution:**
- Pause the worst offenders (CUSTOM-GRAPHENE COATING-KEYWORD at 0.59 ROAS, ALS-Ceramic-SP-Product Target at 0.51 ROAS, Universal-Graphene Coating Auto at 0.64 ROAS)
- Reduce bids on remaining non-P0 campaigns to lower CPC
- Reallocate recovered budget to P0 campaigns, which have proven ROAS

**Impact:**
- $2,000+ in unprofitable spend (from campaigns below 0.75 ROAS) can be redirected
- Reallocated to P0's best campaign (Universal-Graphene Spray Manual at 1.83 ROAS), this would generate ~$3,660 in additional sales
- Net improvement: ~$3,660 in sales vs. the current ~$1,500 those campaigns generate

### Targeting Strategy

**Keyword vs Product Targeting:**

| Targeting Strategy | Clicks | Spend | Sales | ROAS | AOV | CPC | CVR |
|-------------------|--------|-------|-------|------|-----|-----|-----|
| Keyword Targeting | 8,695 | $10,845 | $13,798 | 1.27 | $42.72 | $1.25 | 3.71% |
| Product Targeting | 5,645 | $6,968 | $8,836 | 1.27 | $49.36 | $1.23 | 3.17% |

Split is 61/39 keyword/product. ROAS is identical at 1.27. Product targeting generates a higher AOV ($49.36 vs $42.72), suggesting it drives higher-priced product sales (likely the Diamond AI Coating). No major reallocation needed here.

**Match Type Breakdown:**

| Match Type | Clicks | Spend | Sales | ROAS | AOV | CPC | CVR |
|------------|--------|-------|-------|------|-----|-----|-----|
| EXACT | 2,976 | $4,504 | $5,141 | 1.14 | $45.90 | $1.51 | 3.76% |
| BROAD | 2,431 | $3,126 | $4,586 | 1.47 | $39.19 | $1.29 | 4.81% |
| PHRASE | 1,083 | $1,489 | $1,542 | 1.04 | $51.39 | $1.38 | 2.77% |

**Finding: Broad is outperforming Exact (harvest-and-scale loop is broken)**

**Problem:**
- Broad ROAS (1.47) is 29% higher than Exact ROAS (1.14)
- Broad CVR (4.81%) is 28% higher than Exact CVR (3.76%)
- Broad CPC ($1.29) is 15% lower than Exact CPC ($1.51)
- This is the reverse of the expected pattern. Exact match should outperform because it targets the most specific, highest-intent queries. When broad outperforms, it means Amazon's algorithm is finding good converting terms through broad match that haven't been harvested into exact campaigns.

**Solution:**
- Mine the broad match search term reports for high-ROAS terms
- Create dedicated exact match campaigns for those terms at controlled bids
- This gives those terms more budget and more bid control

**Impact:**
- If top broad performers are moved to exact with dedicated budgets, ROAS should improve further. The $3,126 in broad spend at 1.47 ROAS would have generated even more at the typically higher exact-match conversion rate if the right terms were isolated.

## P0 Product-Level Findings

### P0 Campaign Map

| Campaign | Spend | Sales | ROAS | Clicks | Orders |
|----------|-------|-------|------|--------|--------|
| Universal-Graphene Spray Manual (9oz) | $2,480 | $4,532 | 1.83 | 2,403 | 135 |
| Graphene Spray-SP-Product Target (16oz) | $1,795 | $2,961 | 1.65 | 1,793 | 79 |
| Universal-Graphene Spray Auto (9oz) | $845 | $1,754 | 2.08 | 1,350 | 56 |
| Universal-Graphene Spray Manual (16oz) | $390 | $1,039 | 2.66 | 281 | 29 |
| CUSTOM-GRAPHENE SPRAY-CONVERSION (9oz) | $337 | $334 | 0.99 | 264 | 10 |
| Universal-Graphene Spray Auto (16oz) | $99 | $287 | 2.91 | 146 | 8 |
| Graphene Coating & Spray BOOST (9oz) | $36 | $81 | 2.27 | 24 | 3 |
| graphene ceramic spray coating KB1 (9oz) | $27 | $28 | 1.04 | 24 | 1 |
| **P0 Total** | **$6,009** | **$11,016** | **1.83** | **6,285** | **321** |

P0 receives 34% of total ad spend ($6,009 of $17,813) and generates 49% of total ad sales ($11,016 of $22,633). P0's blended ROAS of 1.83 is the best in the account by far.

**Key observation:** The 16oz variant has significantly better ROAS (Manual: 2.66, Auto: 2.91) than the 9oz variant (Manual: 1.83, Auto: 2.08), but gets far less spend. The 16oz is the higher-margin product. Shifting more ad spend toward the 16oz variant is a quick win.

### Blocker Analysis (CVR is Primary Blocker from SQP)

**Search Term Performance on Tagged Keywords:**

| Search Term | Tier | Spend | Sales | ROAS | Clicks | Orders | CVR |
|-------------|------|-------|-------|------|--------|--------|-----|
| graphene ceramic coating | Tier 1 | $483 | $727 | 1.51 | 392 | 21 | 5.36% |
| graphene coating for cars | Tier 1 | $215 | $108 | 0.50 | 138 | 3 | 2.17% |
| graphene spray coating | Tier 1* | $103 | $281 | 2.72 | 71 | 7 | 9.86% |
| ceramic coating for cars | Tier 2 | $748 | $728 | 0.97 | 597 | 18 | 3.02% |
| ceramic coating | Tier 2 | $551 | $240 | 0.44 | 292 | 6 | 2.05% |
| car ceramic coating | Tier 2* | $262 | $212 | 0.81 | 180 | 5 | 2.78% |
| ceramic spray | Tier 2* | $118 | $186 | 1.58 | 72 | 6 | 8.33% |
| car wax | Tier 3 | $66 | $0 | 0.00 | 48 | 0 | 0.00% |

*Related terms not explicitly tagged but relevant to the tier

**Analysis by tier:**

- **Tier 1:** Mixed performance. "Graphene ceramic coating" is the star (5.36% CVR, 1.51 ROAS) with meaningful volume. "Graphene spray coating" converts even better (9.86% CVR, 2.72 ROAS) but has lower volume. However, "graphene coating for cars" is burning $215 at 0.50 ROAS with only 2.17% CVR. This term likely triggers for Graphene Coating for Cars (P2), not the Spray Coating (P0), which explains the poor conversion.

- **Tier 2:** Confirming the SQP finding. "Ceramic coating for cars" and "ceramic coating" are getting significant spend ($1,299 combined) but both below break-even (0.97 and 0.44 ROAS). CVR is 2-3%, well below industry 7-8%. On these broader queries, the Graphene Spray Coating is competing against traditional ceramic liquid coatings which dominate the search results. The product shows up but doesn't convert because the searcher is often looking for a different product format.

- **Tier 3:** Confirmed dead. "Car wax" has $66 in spend, 48 clicks, 0 orders. These queries should be negated.

**Placement Performance (Account Level):**

| Placement | Spend | Sales | ROAS | CTR | CVR |
|-----------|-------|-------|------|-----|-----|
| Top of Search | $5,649 | $6,859 | 1.21 | 5.10% | 4.25% |
| Rest of Search | $5,770 | $8,014 | 1.39 | 0.63% | 3.09% |
| Product Pages | $6,322 | $7,760 | 1.23 | 0.26% | 3.50% |

Top of Search has the best CTR (5.10%) and CVR (4.25%) but the worst ROAS (1.21) because CPC is highest ($1.57). Rest of Search delivers the best ROAS (1.39) at a lower CPC ($1.06). Product Pages absorb the most spend ($6,322) with moderate returns.

For a CVR blocker, Top of Search placement is the right lever because it converts best (4.25%). However, the CPC premium is high. A better lever here is reducing wasted spend on non-converting terms (see below) and reallocating to Top of Search on the terms that are already converting.

**Wasted Spend Summary:**

| Category | Spend | Sales | ROAS | Action |
|----------|-------|-------|------|--------|
| "ceramic coating" (Tier 2, 0.44 ROAS) | $551 | $240 | 0.44 | Reduce bids or negate from non-P0 campaigns |
| "graphene coating for cars" (likely P2 traffic) | $215 | $108 | 0.50 | Negate from P0 campaigns (belongs to P2) |
| "car wax" (Tier 3, 0 orders) | $66 | $0 | 0.00 | Negate immediately |
| "car ceramic coating" (Tier 2, 0.81 ROAS) | $262 | $212 | 0.81 | Reduce bids |
| **Total addressable waste** | **$1,094** | | | |

This $1,094 can be reallocated to the high-ROAS terms: "graphene ceramic coating" (1.51 ROAS), "graphene spray coating" (2.72 ROAS), and the 16oz variant campaigns (2.66-2.91 ROAS).

## Insights

- **P0 ads are fundamentally healthy.** The core Graphene Spray campaigns are running at 1.83-2.91 ROAS. The CVR issue from SQP is driven more by non-P0 products diluting brand-level metrics and by spending on broad Tier 2 keywords where the product doesn't match searcher intent.
- **The account's real problem is capital misallocation, not ad management.** P0 generates 1.83 ROAS while non-P0 products average 0.78 ROAS. The $8,511 spent on P1/P2/P3 ads is mostly unprofitable. Reallocating even half of that to P0 would dramatically improve account-level returns.
- **The 16oz variant is the hidden opportunity.** It has 2.66-2.91 ROAS across its campaigns but receives only $489 in total ad spend. The 9oz variant gets 5x more spend at lower ROAS. Shifting spend toward 16oz is a quick, low-risk win.

## Things to Investigate Further

- **"Shiny car stuff" is converting at 2.07 ROAS with $538 spend.** This is a non-obvious search term driving real volume. Worth investigating whether this is a trending term and scaling spend on it.
- **ASIN targeting (b0b94g13cn, b09q3mmq6p)** is performing well (1.73 and 3.09 ROAS). These are likely competitor ASINs. Worth identifying them and expanding product targeting to similar ASINs.

## Questions for the Seller

- **Is Perpetua managing the campaigns?** The campaign naming convention ("Perpetua - SP") suggests a third-party tool is managing the ads. If so, the harvest-and-scale loop (broad to exact migration) should be happening automatically but isn't. Worth understanding the current management setup before recommending structural changes.
