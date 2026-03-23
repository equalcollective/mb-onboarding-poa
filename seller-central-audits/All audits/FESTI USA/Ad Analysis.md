# Ad Analysis: FESTI USA

**Data window:** Dec 4, 2025 to Jan 31, 2026 (58 days). Ads stopped entirely from Feb 1 onward.
**Total account spend:** $6,838 | **Total sales:** $5,985 | **Overall ROAS: 0.88** (below 1.5x break-even)

---

## Account-Level Analysis

### Campaign Structure

34 campaigns total. Several campaigns from "theppclab" (external PPC agency) and several "TGA" campaigns (likely managed in-house). Campaign structure is mixed:

- The "TGA" balloon shine spray campaigns are well-structured: 1-3 keywords per campaign with proper EXACT/PHRASE separation. This is correct
- The "theppclab" campaigns are overstuffed: "B0FN9LZKQ3 Exact SPT Theppclab" has 21 targets, "PDT B0FFJ4WCRK SPT theppclab" has 38 targets. Budget starvation is visible: most targets in these campaigns have zero spend

**Problem:** Two different campaign management approaches are running simultaneously (TGA in-house vs theppclab agency), with no apparent coordination. The theppclab campaigns for Big Head Cardboard Cutout are burning money (0.04-0.52 ROAS) while the TGA campaigns for Balloon Shine Spray have one profitable campaign being starved of budget.

### Auto vs Manual Split

The account is ~97% manual campaigns. Three small auto campaigns exist (ATM Facecutout, B0FFJ4WCRK ATM, B0FN9LZKQ3 ATM) with combined spend of ~$185. Auto is negligible. This is fine if the manual campaigns were well-targeted, but given the overall 0.88 ROAS, the manual targeting needs significant optimization.

### Campaign Profitability

| Campaign | Product | Spend | Sales | ROAS | Clicks | Orders |
|----------|---------|-------|-------|------|--------|--------|
| DecoShine Manual balloon spray shine NS | Balloon Shine Spray | $416 | $674 | **1.62** | 329 | 41 |
| Manual PH - cardboard cutout | Life Size Cutout | $1,356 | $1,779 | 1.31 | 1,548 | 56 |
| Manual EX - custom cardboard cutout | Life Size Cutout | $526 | $863 | **1.64** | 456 | 18 |
| Manual PH - custom cardboard cutout | Life Size Cutout | $371 | $454 | 1.23 | 389 | 12 |
| Manual EX - balloon shine spray | Balloon Shine Spray | $317 | $300 | 0.94 | 222 | 18 |
| Manual PH - balloon shine spray | Balloon Shine Spray | $317 | $170 | 0.53 | 234 | 11 |
| PDT B0FN9LZKQ3 SPT theppclab | Big Head Cutout | $605 | $202 | **0.33** | 266 | 11 |
| Exact head cutout spt the ppclab | Big Head Cutout | $203 | $8 | **0.04** | 67 | 1 |
| Pharase Face cutout SPT theppclab | Big Head Cutout | $183 | $18 | **0.10** | 73 | 2 |
| Manual Face cutout SPT the ppclab | Big Head Cutout | $80 | $0 | **0.00** | 45 | 0 |
| **Total unprofitable (below 1.5x)** | | **$5,538** | | | | |
| **Total profitable (above 1.5x)** | | **$942** | $1,537 | **1.63** | | |

**Problem:** $5,538 of the $6,838 total spend (81%) went to campaigns below the 1.5x ROAS threshold. Only $942 (14%) went to the two profitable campaigns.

**Solution:** Pause all Big Head Cardboard Cutout campaigns immediately ($1,071 wasted on 0.04-0.52 ROAS). Reallocate to the two profitable campaigns.

**Impact:** $1,071 reallocated to the DecoShine campaign at its 1.62 ROAS would generate $1,735 in additional sales. Combined with the existing $674, that is $2,409 from the same money currently generating $228.

### Targeting Strategy

**Keyword vs Product Targeting:**

| Targeting Strategy | Clicks | Spend | Sales | ROAS | CVR |
|-------------------|--------|-------|-------|------|-----|
| Keyword Targeting | 5,188 | $6,104 | $5,769 | 0.95 | 4.11% |
| Product Targeting | 383 | $734 | $216 | **0.29** | 3.39% |

Product targeting (ASIN targeting) is performing terribly at 0.29 ROAS. All product targeting spend is on the Big Head Cardboard Cutout campaigns from theppclab. This should be paused entirely.

**Match Type Breakdown:**

| Match Type | Clicks | Spend | Sales | ROAS | CVR |
|------------|--------|-------|-------|------|-----|
| PHRASE | 2,483 | $2,639 | $2,795 | **1.06** | 3.71% |
| EXACT | 1,832 | $2,473 | $2,132 | 0.86 | 3.88% |
| BROAD | 626 | $658 | $705 | **1.07** | 6.23% |

Exact match underperforming phrase and broad is unusual. In this case, it is because the "exact" campaigns include poorly chosen keywords ("cardboard cutout life size" at 0.74 ROAS, "balloon shine spray" at 0.94 ROAS). Broad has the highest CVR (6.23%) but the smallest spend, suggesting untapped conversion potential from long-tail search terms found via broad matching.

---

## P0 Product-Level Findings

### P0 Campaign Map

| Campaign | Spend | Sales | ROAS | Clicks | Orders |
|----------|-------|-------|------|--------|--------|
| DecoShine Manual balloon spray shine NS | $416 | $674 | **1.62** | 329 | 41 |
| Manual EX - B0D4S92RYK - balloon shine spray | $317 | $300 | 0.94 | 222 | 18 |
| Manual PH - B0D4S92RYK - balloon shine spray | $317 | $170 | 0.53 | 234 | 11 |
| Manual EX - B0D4S92RYK - balloon spray | $214 | $178 | 0.83 | 141 | 10 |
| Manual PH - B0D4RBM6NH - balloon shine | $190 | $54 | 0.28 | 58 | 2 |
| Manual EX - B0D4RBM6NH - balloon shine | $146 | $35 | 0.24 | 48 | 2 |
| Manual PH - B0D4S92RYK - balloon spray | $111 | $64 | 0.58 | 62 | 4 |
| **Total P0** | **$1,711** | **$1,475** | **0.86** | **1,094** | **88** |

P0 represents 25% of total account ad spend. The profitable campaign (DecoShine Manual NS) at 1.62 ROAS generated 47% of P0's orders on only 24% of P0's spend.

The B0D4RBM6NH (Silver Glitter variant) campaigns are the worst performers: $336 combined spend at 0.26 ROAS. This variant simply doesn't convert from ads.

### Blocker Analysis: Impression Share

SQP identified impression share as the primary blocker. Here is how P0's Tier 1/2 keywords are performing in ads:

| Search Term | Tier | Spend | Sales | ROAS | Clicks | Orders | CVR |
|-------------|------|-------|-------|------|--------|--------|-----|
| balloon spray shine | 1 | $270 | $213 | 0.79 | 175 | 12 | 6.86% |
| balloon shine spray | 1 | $297 | $104 | 0.35 | 92 | 5 | 5.43% |
| deco shine balloon spray | Branded | $54 | $99 | **1.84** | 32 | 6 | **18.75%** |
| balloon shine | 1 | $40 | $33 | 0.83 | 33 | 2 | 6.06% |
| decoshine balloon spray | Branded | $11 | $50 | **4.71** | 9 | 3 | **33.33%** |
| balloons shine spray | 1 | $24 | $0 | 0.00 | 9 | 0 | 0.00% |

**Key finding:** The branded search terms ("deco shine balloon spray", "decoshine balloon spray") convert at 18-33% CVR with 1.84-4.71 ROAS. These are extremely profitable. The generic hero terms ("balloon shine spray", "balloon spray shine") have decent CVR (5-7%) but terrible ROAS due to high CPC ($3.23 and $1.54 respectively).

**The impression share problem is real but the solution is nuanced.** Simply increasing spend on "balloon shine spray" at $3.23 CPC and 0.35 ROAS would burn money. The better path:
1. Scale the branded terms (already profitable, just underfunded)
2. Lower bids on generic terms to reduce CPC while maintaining volume
3. Focus on the one profitable campaign structure (DecoShine Manual NS) and pause the others

## Insights

- The single best-performing campaign ("DecoShine Manual balloon spray shine NS" at 1.62 ROAS, 41 orders) was getting only 24% of P0's ad budget. Meanwhile, the Silver Glitter variant campaigns burned $336 at 0.26 ROAS. Simply reallocating the Silver Glitter budget to the profitable campaign would have nearly doubled P0's ad-attributed sales
- The Big Head Cardboard Cutout campaigns managed by theppclab are the worst performers in the account. Combined $1,071 spend at 0.12 ROAS. This is $1,071 that could go to the profitable DecoShine or Life Size Cutout campaigns
- The CPC for "balloon shine spray" ($3.23) is extremely high for a $16 product. At this CPC, the brand needs ~20% ad CVR to break even at 1.5x ROAS, which is unrealistic for a generic keyword. The branded terms achieve this (18-33% CVR) but generic terms do not. Bid optimization is critical before scaling

## Questions for the Seller

- Are you working with theppclab (the PPC agency managing the Big Head Cutout campaigns)? Their campaigns have 0.04-0.52 ROAS and are burning significant budget. Is there a reason these are still running?
- The DecoShine campaign at 1.62 ROAS was the most profitable campaign in the account. Why was it not scaled up while the loss-making campaigns continued to spend?
