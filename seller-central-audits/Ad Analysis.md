# Step 4: Ad Analysis

## Objective

Analyze the prospect's advertising campaigns to identify problems, solutions, and their impact. This step produces actionable findings in a Problem-Solution-Impact format for the sales call.

## Data Source

**Metabase API (direct)** via saved questions from the "Campaign Analysis V1" dashboard (Collection ID: 103, Database ID: 4).

### API Reference

**Base URL:** `https://metabase.equalcollective.com`

**Authentication:** Header `x-api-key: mb_9BXilSfuMV8Cwdf6QoWAketWc96cUD2CNZKOLvhcfcU=`

**Endpoint:**

```
POST /api/card/{card_id}/query/json
```

Returns clean JSON rows directly (no Metabase metadata wrapper).

### Parameter Format

Parameters are passed as a JSON array in the request body. Each parameter object has `type`, `target`, and `value`. There are two target formats depending on the template tag type in the saved question:

- **Dimension tags** (dropdown filters like `seller_name`, `status`, `campaign_name`):
  ```json
  {"type": "string/=", "target": ["dimension", ["template-tag", "seller_name"]], "value": ["SellerName"]}
  ```

- **Variable tags** (text/number/date inputs like `start_date`, `end_date`, `min_roas`):
  ```json
  {"type": "date/single", "target": ["variable", ["template-tag", "start_date"]], "value": "2026-01-01"}
  {"type": "number/=", "target": ["variable", ["template-tag", "min_roas"]], "value": 1.5}
  ```

**How to determine which format:** Check the saved question's template tags via `GET /api/card/{card_id}`. Look at `dataset_query.native.template-tags.{tag_name}.type`. If `"dimension"`, use the dimension target. If `"text"`, `"number"`, or `"date"`, use the variable target.

**Example:**
```bash
curl -s -H "x-api-key: $API_KEY" \
  -X POST "https://metabase.equalcollective.com/api/card/699/query/json" \
  -H "Content-Type: application/json" \
  -d '{"parameters": [
    {"type": "string/=", "target": ["dimension", ["template-tag", "seller_name"]], "value": ["ReignDrops"]},
    {"type": "string/=", "target": ["dimension", ["template-tag", "status"]], "value": ["ENABLED"]},
    {"type": "date/single", "target": ["variable", ["template-tag", "start_date"]], "value": "2026-01-01"},
    {"type": "date/single", "target": ["variable", ["template-tag", "end_date"]], "value": "2026-01-31"}
  ]}'
```

### Available Reports and Relevant Questions

Documentation for each report is in the [Ad Analysis Metasbase Documentation](./Ad%20Analysis%20Metasbase%20Documentation/) folder.

#### Campaign Report (campaign-level performance)

| Question ID | Name | Granularity | Key Filters |
|-------------|------|-------------|-------------|
| 699 | Campaign view for Period A range | Campaign level, custom date range | seller_name (dimension), status (dimension), start_date, end_date, min_roas, min_clicks, min_orders |
| 698 | Campaign Monthly Performance Report | Campaign level, month-over-month | seller_name (dimension), status (dimension) |
| 703 | Auto vs Manual comparison on period A date range | Seller + targeting type, custom date range | seller_name (dimension), status (dimension), start_date, end_date |

#### Targeting Report (keyword/ASIN/category targeting performance)

| Question ID | Name | Granularity | Key Filters |
|-------------|------|-------------|-------------|
| 709 | Product vs Keyword Targeting (Seller Level) | Seller level | seller_name, start_date, end_date |
| 710 | Keyword Match Type Breakdown (Seller Level) | Seller level | seller_name, start_date, end_date |
| 712 | All Targetings Compared (Seller Level) | Individual targets | seller_name, start_date, end_date, min_roas, min_clicks, min_orders |
| 713 | All Targetings by Campaign (Period A) | Campaign + targeting | seller_name, campaign_name, start_date, end_date |

#### Search Term Report (what customers actually searched)

| Question ID | Name | Granularity | Key Filters |
|-------------|------|-------------|-------------|
| 719 | ST: Seller Level (Date Range) | Seller + search term (aggregated across campaigns) | seller_name (dimension), start_date, end_date, min_clicks, min_orders, customer_search_term (dimension) |
| 717 | ST: Campaign Level (W/O Targeting) | Campaign + search term | seller_name (dimension), campaign_name (dimension), start_date, end_date, customer_search_term (dimension), min_clicks, min_orders, min_roas |
| 718 | ST: Campaign Level (W Targeting) | Campaign + targeting + match type + search term | seller_name (dimension), campaign_name (dimension), start_date, end_date, customer_search_term (dimension) |

**Q719 vs Q717:** Q719 aggregates search terms across all campaigns (one row per search term). Q717 shows search terms per campaign (one row per campaign + search term). Use Q719 by default. Use Q717 when you need to filter to specific P0 campaigns (from the product-to-campaign mapping) to reduce noise.

#### Placement Report (where ads appeared)

| Question ID | Name | Granularity | Key Filters |
|-------------|------|-------------|-------------|
| 705 | Placement Performance by Date Range (Campaign Level) | Campaign + placement | seller_name (dimension), campaign_name (dimension), start_date, end_date |
| 706 | Placement Performance by Date Range (Seller Level) | Seller + placement | seller_name (dimension), start_date, end_date |

#### Advertised Product Report (which ASINs are being advertised)

| Question ID | Name | Granularity | Key Filters |
|-------------|------|-------------|-------------|
| 726 | Advertised Product: ASIN Level - Period A Range | Campaign + ASIN | seller_name, campaign_name, start_date, end_date, min_clicks, min_orders |

### Metrics Returned by All Queries

| Metric | Description |
|--------|-------------|
| impressions | Times ads were shown |
| clicks | Times ads were clicked |
| orders | Orders attributed to ads (7-day attribution) |
| spend | Total ad spend ($) |
| sales | Total ad-attributed sales ($) |
| roas | Return on Ad Spend (sales / spend) |
| acos | Advertising Cost of Sales (spend / sales x 100%) |
| cpc | Cost Per Click (spend / clicks) |
| ctr | Click-Through Rate (clicks / impressions x 100%) |
| cvr | Conversion Rate (orders / clicks x 100%) |
| aov | Average Order Value (sales / orders) |

## Workflow

The ad analysis has two levels:

- **Account-level analysis (4a-4d):** How is the overall account being managed? Campaign structure, auto vs manual split, profitability, targeting strategy. These are general talking points about capital allocation and strategic issues.
- **Product-level analysis (4e onwards):** For P0 specifically, can we pull the PPC levers to solve the blockers identified in Step 3? This connects the SQP findings (impression share, CTR, CVR blockers) to concrete ad actions.

### Account-Level Analysis

#### Step 4a: Campaign Structure Check

**Question:** Is the campaign structure set up correctly, or are campaigns overstuffed with too many keywords?

**Why this matters:** Amazon allocates a campaign's daily budget across all keywords in that campaign. With too many keywords, the algorithm picks a few "winners" and starves the rest, regardless of their potential. The ideal structure is 3-5 keywords per manual campaign, so each keyword gets meaningful budget and independent bid control.

**Tool:** Q713 (All Targetings by Campaign, Period A)

**Time window:** 90 days. Use a range that covers at least the full ad data availability from Step 1.

**Process:**

1. Pull Q713 for the seller and group results by campaign. Count targeting rows per campaign.
2. Focus on manual campaigns only. Auto campaigns naturally have ~4 targets (close match, loose match, substitutes, complements), that is expected.
3. For manual campaigns with too many targets, look at:
   - How many targets are actually spending vs sitting at zero? This confirms whether budget starvation is happening.
   - How concentrated is the spend? If the top few targets eat most of the budget, the rest are effectively dead.
4. Identify high-ROAS keywords that are clearly underspending because they're competing with too many other keywords for budget. Use statistical significance judgment (don't draw conclusions from 2 clicks). These are the revenue opportunity.
5. If Step 3 identified impression share as a blocker on Tier 1 queries, check whether those Tier 1 keywords exist in overstuffed campaigns. If they do, the campaign structure is a direct cause of the impression share problem.

**If the campaigns are already structured with 3-5 keywords each, note it and move on.**

#### Step 4b: Auto vs Manual Split

**Question:** Is the account being actively managed, or is it running on autopilot?

**Why this matters:** Auto campaigns are Amazon's algorithm finding customers for you. Manual campaigns are you telling Amazon exactly which keywords/products to target. The healthy pattern is: auto is smaller, used for discovery, with lower ROAS. Manual is where the real spend goes, with better ROAS, because the winning search terms from auto have been harvested and scaled with dedicated budgets and controlled bids. If this pattern is reversed, the harvest-and-scale loop isn't happening.

**Tool:** Q703 (Auto vs Manual comparison on Period A date range)

**Time window:** Same 90-day window as Step 4a.

**What to look at:**

- **Spend split:** What share of total spend is auto vs manual?
- **ROAS comparison:** Is manual outperforming auto? If not, the manual campaigns are likely poorly targeted or structured (connects to Step 4a).
- **Sales split:** Which targeting type is driving revenue?

The healthy case is straightforward: manual drives the majority of spend and sales at better ROAS, auto is a smaller discovery channel. Note it and move on.

The unhealthy case needs context. Auto outperforming manual could mean lazy account management (no harvesting). 

**If the split looks off, this sets up the search term analysis.** The natural next question is: what's converting in auto that should have been moved to manual?

#### Step 4c: Campaign-Level Profitability

**Question:** How many campaigns are running at a loss, and how much spend is being wasted?

**Tool:** Q699 (Campaign view for Period A range)

**Time window:** Same 90-day window as Step 4a.

**Profitability threshold:** As a general rule, campaigns below 1.5x ROAS are unprofitable (the ad cost exceeds the margin on the product). This is a default assumption. Eventually, cost of goods data will be integrated for precise break-even calculations. Until then, 1.5x is the floor.

**What to look at:**

- How many campaigns are below 1.5x ROAS? What is the total spend on those campaigns?
- Among the unprofitable campaigns, which ones have enough clicks to be statistically meaningful? A campaign with 5 clicks and 0 orders is too early to call. A campaign with 30+ clicks and ROAS below 1.0 is clearly unprofitable.
- Among the profitable campaigns, which ones have room to absorb more budget? Look for campaigns with strong ROAS that could scale.

The finding is: total wasted spend on unprofitable campaigns, the solution is pausing or restructuring those campaigns, and the impact is the reallocation of that spend to profitable campaigns (calculate the expected sales at the profitable campaign's ROAS).

#### Step 4d: Targeting Strategy Split

**Question:** Is spend allocated well across targeting types and match types?

**Tools:**
- Q709 (Product vs Keyword Targeting, Seller Level) for the keyword vs product targeting split
- Q710 (Keyword Match Type Breakdown, Seller Level) for the exact vs phrase vs broad split

**Time window:** Same 90-day window as previous steps.

**What to look at:**

**Keyword vs Product targeting:**
- Which targeting strategy is driving better results (ROAS, sales, CVR)?
- The principle is simple: whatever is working, do more of it. If product targeting is outperforming keyword targeting significantly, the account should shift more budget toward product targeting, and vice versa.
- Note the split for the output. This is useful context even if there's no clear problem.

**Match type breakdown (Exact vs Phrase vs Broad):**
- Exact match should generally have the highest ROAS and strongest conversion, because it's the most targeted. If exact is underperforming phrase or broad, something is off with keyword selection.
- The same harvest-and-scale principle from auto vs manual applies here: winning search terms from broad and phrase should be harvested into exact match campaigns. If broad is outperforming exact, it suggests the exact keywords are poorly chosen or the harvesting loop isn't happening.
- However, this is less black and white than auto vs manual. There are legitimate reasons for phrase or broad to perform well (discovery of long-tail variations, for example). Use judgment.

### Product-Level Analysis

The product-level analysis connects the blockers identified in Step 3 ([SQP.md](./SQP.md)) to concrete PPC actions. Step 3 tells us what the blocker is (impression share, CTR, or CVR). The domain knowledge in [CLAUDE.md](./CLAUDE.md) maps each blocker to PPC levers and listing levers. This section checks whether those PPC levers can actually be pulled.

#### Step 4e: Product-to-Campaign Mapping

**Question:** Which campaigns are running ads for P0, and how are they performing?

**Why this comes first:** Before analyzing whether PPC levers can solve P0's blockers, you need to know which campaigns are relevant to P0. A seller might have 36 campaigns, but only 4-5 are advertising the P0 product.

**Tool:** Q726 (Advertised Product: ASIN Level, Period A Range)

**Time window:** Same 90-day window as account-level steps.

**Process:**

1. Pull Q726 for the seller with the 90-day date range
2. Find P0 by matching on the `parent_product_name` column. This is the parent-level grouping, which aligns with how Step 1 selected ASINs at the parent level. There is also a `product_name` column (child level), but the mapping should be on `parent_product_name`.
3. List all campaigns that are advertising P0, with their spend, sales, ROAS, clicks, and orders
4. This gives you the P0 campaign map: the set of campaigns you'll analyze in the following product-level steps

This mapping also reveals whether P0 is being advertised at all, how many campaigns are targeting it, and what share of total ad spend goes to P0 vs other products.

#### Step 4f: Solving P0 Blockers via PPC

**Question:** For the primary blocker identified in Step 3, can we pull the PPC levers to fix it?

**Important:** Not every blocker has a strong PPC lever. If the data doesn't reveal a clear, actionable PPC opportunity, do not force one. It is perfectly fine to conclude that the PPC side is already reasonable and the solution is primarily on the listing side (Step 2e). Only present levers that are genuinely impactful.

This step takes the blocker from Step 3 ([SQP.md](./SQP.md)) and the P0 campaign map from Step 4e, and checks whether the PPC levers outlined in [CLAUDE.md](./CLAUDE.md) can actually be pulled. The approach depends on which blocker is primary.

**Time window:** Same 90-day window as previous steps. When filtering by campaign, use the P0 campaigns identified in Step 4e. If there are many campaigns, focus on the top 3 by spend.

---

**If the primary blocker is Impression Share:**

The PPC lever is: bid on the keywords where impression share is low.

**Tool:** Q719 (Search Term: Seller Level, Date Range). If there are too many search terms, use Q717 (Campaign Level) filtered to the P0 campaigns from Step 4e to reduce noise.

**Process:**
1. Take the Tier 1, Tier 2, and Tier 3 keywords from Step 3
2. Look up those search terms in the search term report to see the current spend, sales, ROAS, and clicks for each
3. Present a search-term-level table showing how much is currently being spent on these terms
4. The finding is usually: very low spend on the terms where we need more visibility. If the CVR/ROAS on those terms is healthy, the case is clear: increase spend, the keywords convert, they just need more budget. If CVR is poor, the issue is on the listing side (point back to the listing quality findings from Step 2).

---

**If the primary blocker is CTR or CVR:**

The PPC levers overlap: better placements (Top of Search), removing wasted spend on non-converting targets, and negating irrelevant search terms. The data pulls are the same for both blockers.

**Tools:**
- Q705 (Placement: Campaign Level, Date Range) for placement analysis
- Q713 (All Targetings by Campaign, Period A) for targeting-level analysis on manual campaigns
- Q717 (Search Term: Campaign Level) for irrelevant search term detection on broad/phrase/auto campaigns

**Process:**
1. **Placement analysis (Q705):** Pull placement data for the P0 campaigns from Step 4e. Compare Top of Search, Rest of Search, and Product Pages on CTR, CVR, ROAS, and spend. Top of Search typically has the best CTR and conversion. If it also has good ROAS, the case is: increase the Top of Search bid modifier to shift more spend to the premium placement. If Top of Search is already getting most of the spend but CTR/CVR is still below industry (from Step 3), the issue is more likely on the listing side.
2. **Targeting analysis (Q713):** Pull targeting data for the P0 manual campaigns. Identify targets with meaningful spend but low/zero conversion. These are wasting budget and should be negated. Calculate the total wasted ad spend.
3. **Search term analysis (Q717):** This is where you catch irrelevant traffic from broad, phrase, and auto campaigns. Q713 shows targeting on manual exact campaigns, but broad/phrase/auto campaigns trigger on search terms you didn't explicitly choose. Pull Q717 filtered to the P0 campaigns and look for completely irrelevant search terms eating spend. These should be negated. Add to the wasted spend total from step 2.

---

**For secondary blockers:** Apply the same logic but with less depth. The primary blocker gets the full analysis. Secondary blockers get a brief note on which lever to pull and why, without the deep data dive.

**Connecting to listing levers:** If the PPC data shows that the product converts poorly even with good targeting and placement, the issue is on the listing side. Point back to the listing quality and conversion driver findings from Step 2e. The action plan should sequence listing fixes before PPC scaling in that case.

---

## Output

Save the output to the seller's audit folder as `Ad Analysis.md`.

The output follows a **Problem-Solution-Impact** format. Each finding is a self-contained block that the closer can read independently and explain to the brand owner.

### Campaign Structure

If campaigns are overstuffed, present the finding as follows:

**Problem:** The campaign structure is off. [Campaign name] has [X] keywords in one campaign. [Y] are actively spending. Very little control over what gets the spending.

Then show the targeting data table for the worst offender (the campaign with the most targets or the hero product campaign), sorted by spend descending. Include: campaign_name, targeting, match_type, impressions, clicks, orders, spend, sales, roas.

**Revenue Opportunity:** Identify 1-2 high-ROAS keywords that are underspending and show the math:

> Move "[keyword]" to a separate campaign and increase spend. Impact:
>
> - Current:
>   - [dominant keyword]: Spend = $X, Sales = $Y
>   - [starved keyword]: Spend = $A, Sales = $B
> - After restructuring:
>   - [dominant keyword]: Spend = $A, Sales = (at its ROAS)
>   - [starved keyword]: Spend = $X, Sales = (at its ROAS)
> - Impact: $Z sales increase

The math is simple: swap the spend levels between a low-ROAS dominant keyword and a high-ROAS starved keyword, and calculate the sales at each keyword's existing ROAS. The difference is the revenue opportunity.

**Also flag:**
- If all match types in a campaign are the same (e.g., all BROAD), call this out as an additional structural issue
- If the campaign structure problem connects to an impression share blocker from Step 3, explicitly tie them together

### Auto vs Manual Split

Always include the data table regardless of whether the split is healthy or not:

| Targeting Type | Clicks | Spend | Sales | ROAS | AOV | CPC | CVR |
|----------------|--------|-------|-------|------|-----|-----|-----|
| Automatic | ... | ... | ... | ... | ... | ... | ... |
| Manual | ... | ... | ... | ... | ... | ... | ... |

**If healthy:** One line below the table: "Auto vs Manual split is healthy. Manual drives X% of spend at Y ROAS. Auto is functioning as a discovery mechanism."

**If unhealthy (auto outperforming or oversized):**

> **Problem:** Auto campaigns are [outperforming manual / driving X% of total spend]. Auto ROAS is [X] vs Manual ROAS of [Y]. This means the account is relying on Amazon's algorithm to find customers instead of actively targeting the keywords that convert. Auto is meant for discovery, not as the primary revenue driver.
>
> **What this tells us:** The winning search terms inside the auto campaigns have never been harvested and scaled. Amazon found these converting terms, but no one took the next step of giving them their own manual campaigns with dedicated budgets.
>
> **Solution:** Mine the auto campaign search terms, identify the top converters, and launch manual exact/phrase campaigns for them. Then negate those terms from auto so spend isn't duplicated.
>
> **Impact:** Manual campaigns with targeted keywords typically achieve 30-50% better ROAS than auto because you control the bid, match type, and budget. If auto is currently doing $X in sales at Y ROAS, moving the top search terms to manual and scaling them should improve ROAS while increasing total sales.

### Campaign Profitability

List unprofitable campaigns (below 1.5x ROAS) with meaningful spend/click volume:

| Campaign | Spend | Sales | ROAS | Clicks | Orders |
|----------|-------|-------|------|--------|--------|
| [unprofitable campaign 1] | ... | ... | ... | ... | ... |
| [unprofitable campaign 2] | ... | ... | ... | ... | ... |
| **Total wasted** | **$X** | | | | |

> **Problem:** [X] campaigns are running below 1.5x ROAS, spending a combined $[total] over [period]. This spend is not generating enough sales to cover product costs.
>
> **Solution:** Pause the unprofitable campaigns and reallocate that budget to the profitable campaigns that have room to scale.
>
> **Impact:** $[total wasted spend] reallocated to [best profitable campaign] at its current [X] ROAS would generate $[calculated sales] in additional sales. Net improvement: $[difference] in sales from the same total ad budget.

### Targeting Strategy

Include both tables as context, then add insights only if something stands out.

**Keyword vs Product Targeting:**

| Targeting Strategy | Clicks | Spend | Sales | ROAS | AOV | CPC | CVR |
|-------------------|--------|-------|-------|------|-----|-----|-----|
| Keyword Targeting | ... | ... | ... | ... | ... | ... | ... |
| Product Targeting | ... | ... | ... | ... | ... | ... | ... |

**Match Type Breakdown:**

| Match Type | Clicks | Spend | Sales | ROAS | AOV | CPC | CVR |
|------------|--------|-------|-------|------|-----|-----|-----|
| EXACT | ... | ... | ... | ... | ... | ... | ... |
| PHRASE | ... | ... | ... | ... | ... | ... | ... |
| BROAD | ... | ... | ... | ... | ... | ... | ... |

If there's a clear imbalance (e.g., product targeting has 2x the ROAS of keyword targeting but gets 20% of the budget), call it out as a reallocation opportunity. If exact match is underperforming phrase/broad, flag the harvest-and-scale loop issue (same principle as auto vs manual). If everything looks reasonable, just include the tables as context and move on.

### P0 Product-Level Findings

#### P0 Campaign Map

Show which campaigns are advertising P0 and their performance:

| Campaign | Spend | Sales | ROAS | Clicks | Orders |
|----------|-------|-------|------|--------|--------|
| [campaign 1] | ... | ... | ... | ... | ... |
| [campaign 2] | ... | ... | ... | ... | ... |

Note total P0 ad spend vs total account ad spend to show what share goes to the hero product.

#### Blocker Analysis

For each blocker from Step 3, present the PPC findings:

**If impression share blocker:** Show the search-term-level table for the Tier 1/2/3 keywords:

| Search Term | Tier | Spend | Sales | ROAS | Clicks | Orders |
|-------------|------|-------|-------|------|--------|--------|
| [keyword 1] | Tier 1 | ... | ... | ... | ... | ... |
| [keyword 2] | Tier 1 | ... | ... | ... | ... | ... |

If spend is very low but ROAS/CVR is healthy: the case is clear, increase spend on these terms, they convert but are underfunded. If CVR is poor: point to listing fixes from Step 2e before scaling spend.

**If CTR blocker:** Show the placement breakdown for P0's campaigns:

| Placement | Spend | Sales | ROAS | CTR | CVR |
|-----------|-------|-------|------|-----|-----|
| Top of Search | ... | ... | ... | ... | ... |
| Rest of Search | ... | ... | ... | ... | ... |
| Product Pages | ... | ... | ... | ... | ... |

If Top of Search has better CTR and good conversion: recommend increasing Top of Search bid modifier, show the current spend gap.

**If CVR blocker:** Show wasted targeting spend and irrelevant search terms:

Wasted targets (meaningful spend, low/zero conversion):

| Campaign | Targeting | Match Type | Spend | Clicks | Orders | CVR |
|----------|-----------|------------|-------|--------|--------|-----|
| ... | ... | ... | ... | ... | ... | ... |

> Total wasted targeting spend: $X. Solution: negate these targets and reallocate.

Irrelevant search terms (if found):

| Search Term | Campaign | Spend | Clicks | Orders |
|-------------|----------|-------|--------|--------|
| ... | ... | ... | ... | ... |

Then show the placement data (same table as CTR blocker) to identify which placement converts best and recommend shifting spend there.

### Insights

Only include if genuinely insightful. Always reference specific campaigns and numbers.

### Things to Investigate Further

Hypotheses that need deeper analysis (search terms, placement data, etc.).

### Questions for the Seller

Questions the data alone can't answer. If nothing stands out, leave blank.
