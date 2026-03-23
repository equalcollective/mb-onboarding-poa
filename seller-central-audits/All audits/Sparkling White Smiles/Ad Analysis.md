# Ad Analysis: Sparkling White Smiles

**Period:** Dec 2025 - Feb 2026 (90 days)

## Account-Level Analysis

### Campaign Structure

The account has only 4 campaigns. Three are manual keyword campaigns and one is auto.

| Campaign | Product | Targets | Spend | Sales | ROAS | Clicks | Orders |
|----------|---------|---------|-------|-------|------|--------|--------|
| Sport Mouth Guard | Sport Mouth Guards (P0) | 76 | $5,432 | $8,155 | 1.50 | 5,555 | 786 |
| Remin Gel | Teeth Sensitivity Kit (P3) | 98 | $4,916 | $2,259 | 0.46 | 4,038 | 110 |
| PearlDent Foam Keywords | Pearldent Dental Foam (P1) | 43 | $1,656 | $1,419 | 0.86 | 851 | 68 |
| Oral Guard 3 Month | Appliance Cleaner (P2) | auto | $29 | $330 | 11.30 | 68 | 21 |

**Finding: Campaign structure is deeply problematic.**

**Problem:**
- The Sport Mouth Guard campaign has 76 keywords in a single campaign, all BROAD match. Amazon's budget allocation algorithm picks winners and starves the rest. Only ~20 of the 76 targets are spending meaningfully.
- The Remin Gel campaign has 98 keywords in a single campaign with a 0.46 ROAS. $4,916 spent on a product that generates only $2,259 in ad sales.
- No campaign uses product targeting. All spend is keyword-based.
- All keywords across all campaigns are BROAD match (97% of spend). No exact match campaigns exist for proven winners.

**Solution:**
- Restructure the Sport Mouth Guard campaign into 3-5 keyword campaigns (3-5 keywords each) grouped by intent
- Pause the Remin Gel campaign entirely (addressed below in profitability)
- Create exact match campaigns for proven high-ROAS keywords
- Launch product targeting campaigns for competitor ASINs

### Auto vs Manual Split

| Targeting Type | Clicks | Spend | Sales | ROAS | AOV | CPC | CVR |
|----------------|--------|-------|-------|------|-----|-----|-----|
| Automatic | 68 | $29 | $330 | 11.30 | $15.70 | $0.43 | 30.88% |
| Manual | 10,444 | $12,004 | $11,833 | 0.99 | $12.27 | $1.15 | 9.23% |

**Finding: Auto campaign massively outperforms manual but gets <0.3% of the budget.**

**Problem:**
- The auto campaign converts at 30.88% CVR with an 11.3x ROAS. It found customers at $0.43 CPC who convert at 3x the manual rate.
- Manual campaigns are collectively at 0.99 ROAS, meaning every dollar spent generates only $0.99 back. The account is losing money on manual campaigns.
- The auto campaign ("Oral Guard 3 Month") is mapped to the Appliance Cleaner product, which already has 61% organic CVR. Amazon's auto-targeting found a perfect audience for this product, but it's barely funded.

**Solution:**
- Increase auto campaign budget significantly. At 11.3x ROAS, even 10x the current spend ($290) would be highly profitable.
- Mine the auto campaign's search terms to identify what's converting, then create manual exact match campaigns for those terms.
- Simultaneously fix the manual campaigns (restructure, negate wasted keywords, shift to exact match).

### Campaign Profitability

| Campaign | Spend | Sales | ROAS | Clicks | Orders |
|----------|-------|-------|------|--------|--------|
| Remin Gel (P3) | $4,916 | $2,259 | 0.46 | 4,038 | 110 |
| PearlDent Foam (P1) | $1,656 | $1,419 | 0.86 | 851 | 68 |
| **Total unprofitable** | **$6,572** | | | | |

**Problem:**
- Two campaigns are running below 1.5x ROAS, burning a combined $6,572 in 90 days. The Remin Gel campaign alone accounts for $4,916 of this, with a 0.46x ROAS and 98 BROAD keywords in a single campaign. This campaign is the single largest driver of the account's TACoS explosion.

**Solution:**
- Pause the Remin Gel campaign immediately. The Teeth Sensitivity Kit has a 3.9% overall CVR (from Step 1), meaning the product itself does not convert. Continuing to spend on ads will not fix a product/listing problem.
- Restructure the PearlDent Foam campaign: reduce from 43 targets to the top 5-10 converting keywords. Test at lower spend with better targeting before scaling.

**Impact:**
- $6,572 in savings over 90 days. If reallocated to the Sport Mouth Guard campaign's high-ROAS keywords (3.78x ROAS on "sports mouth guards"), this spend would generate $24,842 in additional sales. Net improvement: ~$23,000 in sales from the same total ad budget.

### Targeting Strategy

**Keyword vs Product Targeting:**

| Targeting Strategy | Clicks | Spend | Sales | ROAS | AOV | CPC | CVR |
|-------------------|--------|-------|-------|------|-----|-----|-----|
| Keyword Targeting | 17,984 | $22,138 | $16,211 | 0.73 | $13.80 | $1.23 | 6.53% |
| Product Targeting | 0 | $0 | $0 | N/A | N/A | N/A | N/A |

No product targeting exists. 100% of spend is on keyword targeting. This is a missed opportunity, especially for a product in a competitive category where appearing on competitor product pages can capture ready-to-buy shoppers.

**Match Type Breakdown:**

| Match Type | Clicks | Spend | Sales | ROAS | AOV | CPC | CVR |
|------------|--------|-------|-------|------|-----|-----|-----|
| BROAD | 16,562 | $19,133 | $14,422 | 0.75 | $13.21 | $1.16 | 6.59% |
| PHRASE | 1,331 | $2,881 | $1,859 | 0.65 | $20.43 | $2.16 | 6.84% |
| EXACT | 159 | $153 | $260 | 1.70 | $19.99 | $0.96 | 8.18% |

**Finding: Exact match has the best ROAS (1.70x) and lowest CPC ($0.96) but gets less than 1% of the budget.**

The harvest-and-scale loop is completely absent. Winning search terms from BROAD should be extracted into EXACT match campaigns with dedicated budgets and controlled bids. Instead, all spend stays in BROAD, where Amazon's algorithm decides which search terms to show the ad on, with unpredictable results.

## P0 Product-Level Findings

### P0 Campaign Map

| Campaign | Spend | Sales | ROAS | Clicks | Orders |
|----------|-------|-------|------|--------|--------|
| Sport Mouth Guard | $5,432 | $8,155 | 1.50 | 5,555 | 786 |

P0 accounts for 24.5% of total account ad spend ($5,432 of $22,167) but generates 54.8% of all ad sales. The Teeth Sensitivity Kit (P3) consumes 61.5% of ad spend at a 0.46x ROAS.

### Placement Analysis

| Placement | Spend | Sales | ROAS | CTR | CVR |
|-----------|-------|-------|------|-----|-----|
| Top of Search | $3,811 (17%) | $5,915 | 1.55 | 7.75% | 16.24% |
| Rest of Search | $3,499 (16%) | $5,230 | 1.49 | 1.15% | 8.53% |
| Product Pages | $14,842 (67%) | $5,396 | 0.36 | 0.69% | 3.08% |

**Finding: 67% of ad spend goes to Product Pages at 0.36x ROAS.**

**Problem:**
- Product Pages receives $14,842 (67% of all spend) but generates only $5,396 in sales at a 0.36 ROAS. CVR on Product Pages is 3.08%.
- Top of Search converts at 16.24% CVR (5.3x better than Product Pages) and generates the highest sales ($5,915) while consuming only 17% of spend.
- This is the primary reason the account-level ROAS is so low. The ads are being shown primarily on competitor product pages where they don't convert, instead of at the top of search results where intent is highest.

**Solution:**
- Add a Top of Search bid adjustment (50-100% increase) to shift more spend toward the highest-converting placement.
- The Product Pages placement cannot be directly reduced, but the Top of Search modifier will proportionally shift spend away from it.

**Impact:**
- If $5,000 of the Product Pages spend were redirected to Top of Search (through bid modifiers), the expected sales at Top of Search ROAS (1.55x) would be $7,750 vs the $1,800 those dollars generate on Product Pages (0.36x ROAS). Net improvement: ~$5,950 in additional sales.

### P0 Wasted Targeting

7 targets in the Sport Mouth Guard campaign are spending above $20 with ROAS below 1.0:

| Targeting | Match Type | Spend | Sales | ROAS | Orders |
|-----------|------------|-------|-------|------|--------|
| warm and form mouth piece | BROAD | $472 | $0 | 0.00 | 0 |
| lite bite mouthguard | BROAD | $348 | $170 | 0.49 | 17 |
| warm and form mouth guard | BROAD | $216 | $145 | 0.67 | 14 |
| warm and form mouthguard | BROAD | $206 | $70 | 0.34 | 7 |
| sports mouthguard | BROAD | $124 | $90 | 0.73 | 9 |
| bruxism guard | BROAD | $73 | $20 | 0.27 | 2 |
| boil and bite mouthpiece | BROAD | $27 | $0 | 0.00 | 0 |
| **Total wasted** | | **$1,465** | | | |

"warm and form mouth piece" alone spent $472 with zero orders. "bruxism guard" is an irrelevant keyword (bruxism is teeth grinding, not sports). These should be negated immediately.

### P0 High-ROAS Keywords (Starved)

These keywords are converting profitably but are trapped in a 76-keyword campaign competing for budget:

| Targeting | Match Type | Spend | Sales | ROAS | Orders |
|-----------|------------|-------|-------|------|--------|
| sports mouth guards | BROAD | $422 | $1,593 | 3.78 | 144 |
| lifting mouth guard | BROAD | $37 | $120 | 3.28 | 12 |
| mouth guard | BROAD | $294 | $874 | 2.97 | 82 |
| teeth protector | BROAD | $117 | $294 | 2.51 | 30 |
| teeth guard | BROAD | $30 | $70 | 2.35 | 7 |
| clear mouth guards | BROAD | $53 | $120 | 2.24 | 12 |
| mouth piece | BROAD | $49 | $100 | 2.02 | 10 |

**Revenue Opportunity:** "sports mouth guards" is the standout. At 3.78x ROAS, it generates $1,593 from only $422 in spend. If this keyword had its own campaign with 3x the current spend ($1,266), it would generate an estimated $4,785 in sales, an increase of $3,192.

Similarly, "mouth guard" at 2.97x ROAS generates $874 from $294 in spend. Scaling to $882 (3x) would generate an estimated $2,619 in sales.

Combined, restructuring these two keywords alone into dedicated campaigns and tripling their spend could generate ~$4,800 in additional sales.

## Insights

- The account's #1 problem is capital misallocation: 61.5% of ad spend goes to the Teeth Sensitivity Kit (P3) at 0.46x ROAS, while the hero product's best keywords are underfunded. Pausing Remin Gel and reallocating to P0's proven keywords would transform account profitability.
- Placement distribution is the #2 problem: 67% of all ad spend goes to Product Pages at 0.36x ROAS, while Top of Search converts at 16.24% CVR. A Top of Search bid modifier would shift spend to the highest-converting placement.
- The auto campaign's 11.3x ROAS and 30.88% CVR signals that Amazon's algorithm has found a high-converting audience for the Appliance Cleaner. This product is being underinvested in advertising despite excellent unit economics.

## Things to Investigate Further

## Questions for the Seller

- The Remin Gel campaign has spent $4,916 in 90 days at 0.46x ROAS. Is there a reason this campaign is still running? Was it intentionally launched to push the Teeth Sensitivity Kit, or has it been running unchecked?
