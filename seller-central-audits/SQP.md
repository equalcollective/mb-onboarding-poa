# Step 3: SQP Analysis

## Objective

Understand the market opportunity for P0 through Search Query Performance data. Three questions to answer:
1. **How big is the market?** (Market sizing)
2. **How much have we captured, and is there more to capture?** (Market share and potential)
3. **What's stopping us from capturing more?** (Blockers)

**Scope:** P0 hero ASIN only by default. The SQP data is brand-level, so the first task is isolating P0-relevant queries from the brand's entire query universe.

## Data Source

**SQP Analyzer MCP** provides brand-level Search Query Performance data from Amazon Brand Analytics.

Key tools:
- `search_queries` - Raw query-level data with brand vs industry funnel metrics
- `manage_query_tags` - Tag queries with framework labels
- `get_trends_absolute_numbers` - Brand vs total counts over time (filterable by tags)
- `get_trends_share_metrics` - Brand share of impressions, clicks, carts, purchases (filterable by tags)
- `get_trends_percentage_metrics` - Brand vs industry conversion rates at each funnel stage (filterable by tags)
- `preview_data` - Check data size before pulling

**Funnel stages (ICAP):** Impression > Click > Add to Cart > Purchase

## Step 3a: Tagging

Tagging is the foundation. Everything after depends on clean, accurate tags. Tags persist in the SQP Analyzer for use by the service team.

### Phase 1: See where we're selling

Pull queries with a minimum filter to surface where the brand is actually converting. Use the full data range (no startDate/endDate) and monthly granularity to get the broadest view of where the brand has traction.

```
search_queries(brandName, periodGranularity="monthly", minCartAdds=1)
```

If this returns too few queries, fall back to `minClicks=3` or `minClicks=5`.

Cross-reference these queries with the product understanding from Step 2. Identify which queries are relevant to P0 vs other products in the brand's catalog.

### Phase 2: See high-volume opportunities

Remove the cart/click filter and pull by search volume instead. Same full data range, monthly granularity.

```
search_queries(brandName, periodGranularity="monthly", minSearchVolume=500, limit=50)
```

Look through these with the product understanding context. Identify high-volume queries relevant to P0 where the brand may not be converting yet.

### Phase 3: Select tagging framework

Using the product understanding from Step 2 and the queries from Phases 1 and 2, decide which tagging framework to use.

**Tier 1 / Tier 2 / Tier 3** (use in ~80% of cases):
- **Tier 1 (Hero):** The 3-5 most directly relevant keywords. Exact product type. These are where you must win. Can be high or low volume.
- **Tier 2 (Core market):** Relevant but one step broader. The main market around the product.
- **Tier 3 (Broad/adjacent):** Related but broader, or secondary use cases. Opportunity to capture adjacent demand.

**Important: No keyword overlap across tiers.** Each keyword must belong to exactly one tier. If a keyword is tagged as Tier 1, it cannot also appear in Tier 2 or Tier 3. This ensures the search volumes, market sizes, and share metrics for each tier are completely separate with no double-counting. When in doubt about which tier a keyword belongs to, pick the one that best matches its intent and commit to it.

**Branded vs Non-branded** (add only when branded volume is meaningful):
- If the brand has significant branded search volume, tag those separately. Branded traffic converts differently and should be analyzed on its own.

**Other frameworks:** If neither Tier 1/2/3 nor Branded/Non-branded adequately captures the query landscape, propose a custom framework and ask Aryan to add the relevant tags to the SQP MCP before proceeding. Only do this when the standard frameworks genuinely don't fit.

Tags for Tier 1, Tier 2, and Tier 3 already exist in the SQP Analyzer.

### Phase 4: Apply tags

Tag the selected queries using `manage_query_tags`. Target ~15-20 queries total. Cover the dominant search volume queries and the most relevant long-tail terms. Accuracy is more important than coverage.

The target keywords from Step 2 are a compass, not a checklist. They inform your judgment on which queries matter, but you're tagging actual SQP queries, not pasting in Step 2's keyword list.

## Step 3b: Market Sizing

**Question:** How big is the market for P0?

**Time window:** 12 months, monthly granularity. Use the last 12 complete months of data. The annual window smooths out seasonality and gives reliable monthly averages.

**Tools (call both per tier):**
1. `get_trends_overall_metrics(brandName, granularity="monthly", startDate, endDate, includeTags=["Tier X"])` for search volume
2. `get_trends_absolute_numbers(brandName, granularity="monthly", startDate, endDate, includeTags=["Tier X"])` for impressions, clicks, cart adds, purchases

**Important:** `get_trends_absolute_numbers` does not return search volume. You must use `get_trends_overall_metrics` for that metric.

For each tier (and for P0 overall), calculate:
- **Average monthly search volume** (from `get_trends_overall_metrics`)
- **Average monthly add to carts** (total market cart additions, from `get_trends_absolute_numbers`)
- **Average monthly purchases** (total market purchases, from `get_trends_absolute_numbers`)
- **Estimated market size ($/mo):** Average product price (from Step 2's competitive landscape) x average monthly add to carts. Use add to carts rather than purchases because SQP undercounts purchases and cart additions give a better indication of actual market demand.

**Seasonality check:** With 12 months of data, look for seasonal patterns in search volume and cart adds. Note peak months and trough months. Cross-reference this with the P0 Annual Trend from Step 1 (Step 5 in [ASIN Selection.md](./ASIN%20Selection.md)). If both the SQP market data and the seller's own sales data show the same seasonal pattern, that confirms the product is market-seasonal, and revenue swings are driven by market demand, not a brand-specific issue. Call this out explicitly in the output.

Present this per tier so the relative size of each tier is visible.

## Step 3c: Market Share and Potential

**Question:** How much have we captured? Is there more to capture?

**Time window:** 3 months, monthly granularity. Use the last 3 complete months of data. Share metrics are percentages and less affected by seasonal volume swings, so a recent window accurately reflects current positioning.

**Tool:** `get_trends_share_metrics(brandName, granularity="monthly", startDate, endDate, includeTags=["Tier X"])` for each tier.

For each tier, pull:
- **Impression share %** (are we showing up?)
- **Click share %** (are we getting clicked?)
- **Cart add share %** (are we getting added to cart?)
- **Purchase share %** (are we getting bought?)

Compare across tiers:
- Where is our share strongest vs weakest?
- Which tier has the largest gap between market size and our capture?
- Is share improving or declining over time?

This directly answers "can we capture more?" If Tier 1 purchase share is 15% and Tier 2 is 3%, there's clear room to grow in Tier 2. If we're already at 25% share on a small Tier 1, the growth is in expanding to Tier 2/3.

It can also reveal that the market is too small or too captured to be worth focusing on.

## Step 3d: Blocker Detection & Growth Path Assessment

**Question:** What's stopping us from capturing more, and what's the growth path for each tier?

**Time window:** 3 months, monthly granularity. Same window as Step 3c so the rate metrics and share metrics are consistent and comparable.

**Tool:** `get_trends_absolute_numbers(brandName, granularity="monthly", startDate, endDate, includeTags=["Tier X"])` for each tier. This is the same tool used in Step 3b for market sizing. If the 3-month window is a subset of the 12-month market sizing window, you may already have the data and can skip a redundant call.

**Computing brand vs industry rates:** Calculate rates from the absolute numbers using volume-weighted averages across the 3-month window. This is more accurate than averaging monthly percentages, because months with higher volume contribute proportionally more.

For each rate, sum the numerator and denominator across all 3 months, then divide:
- **Brand CTR:** sum(clicks_brand) / sum(impressions_brand)
- **Industry CTR:** sum(clicks_total) / sum(impressions_total)
- **Brand ATC Rate:** sum(cart_adds_brand) / sum(clicks_brand)
- **Industry ATC Rate:** sum(cart_adds_total) / sum(clicks_total)
- **Brand CVR:** sum(purchases_brand) / sum(clicks_brand)
- **Industry CVR:** sum(purchases_total) / sum(clicks_total)
- **Brand Cart-to-Purchase Rate:** sum(purchases_brand) / sum(cart_adds_brand)
- **Industry Cart-to-Purchase Rate:** sum(purchases_total) / sum(cart_adds_total)

**Statistical significance check:** Before drawing conclusions from any rate (CVR, CTR, ATC rate), verify that the base numbers are large enough to be meaningful. A 30% CVR from 1 conversion and 3 clicks is not actionable. Use judgment, but be cautious with rates derived from fewer than ~20 clicks or ~5 conversions per month. If the data is too thin for a tier, note this rather than drawing false conclusions.

**Note on `get_trends_percentage_metrics`:** This tool returns pre-calculated brand vs industry rates per period. It is not used for blocker detection (volume-weighted averages from absolute numbers are more accurate), but it remains useful for diagnosing rate trends over time. For example, if you need to check whether brand CTR has been declining month-over-month or whether a CVR drop is recent vs long-standing, pull `get_trends_percentage_metrics` over a longer window (6-12 months) and inspect the monthly progression. This is an optional diagnostic step, not part of the standard blocker detection flow.

### Blocker Detection

For each tier, compare brand vs industry at each funnel stage:
- **Brand CTR vs Industry CTR** (Impression to Click)
- **Brand ATC Rate vs Industry ATC Rate** (Click to Cart)
- **Brand CVR vs Industry CVR** (Click to Purchase)
- **Brand Cart-to-Purchase Rate vs Industry** (Cart to Purchase)

The blocker is wherever brand rate is significantly below industry rate. But the blocker can also be at the share level from Step 3c: if impression share is 2% on a high-volume tier, the primary blocker is visibility, not conversion.

Identify:
- **Primary blocker:** The single biggest constraint to growth. Could be impression share (not showing up), CTR (showing up but not getting clicked), ATC rate (getting clicked but not added to cart), or CVR (not converting).
- **Secondary blockers:** Other funnel stages that are underperforming but less impactful than the primary.

Do this per tier, because the blocker often differs across tiers.

### Growth Path Assessment

After identifying blockers (above) and share position (from Step 3c), assess whether each tier is leverageable and through which growth path. Do not prematurely exclude any tier based on keyword semantics alone. Let the data decide.

For each tier, ask: **"Can we leverage this tier and scale it, and through which growth path?"**

Match the blocker + share combination against the common patterns defined in [CLAUDE.md](./CLAUDE.md) under "Domain Knowledge: SQP and Market Analysis > How This Maps to the Action Plan." Use those patterns to determine the growth path for each tier.

The output should explain this assessment for each tier: what the blocker is, which pattern it matches, and what the growth path looks like.

## Step 3e: Output

Save the output to the seller's audit folder as `SQP Analysis.md`.

### Tagging Rationale

Before presenting the data, explain the tiering logic clearly. The reader needs to understand WHY each tier is what it is. For each tier, write one sentence explaining the grouping principle and how it relates to the product. Example:

> **Tier 1 (Hero):** Queries where the customer is searching for exactly a baby ink pad. The product is the direct answer to the search.
> **Tier 2 (Core market):** Queries for baby footprint/handprint kits. The product is one solution among several (clay kits, frame kits, inkless pads). Broader competitive set but same customer need.
> **Tier 3 (Adjacent):** Generic ink pad and pet paw print queries. The product can appear but is not the primary intent for most searchers.

Also note any tiers where the current product may not be a good fit for the query intent (e.g., "kit" queries when the product is a standalone item). Be honest about what is capturable today vs what would require product changes.

### Market Sizing

Per-tier table:

| Tier | Monthly Search Volume | Monthly Add to Carts (Market) | Monthly Purchases (Market) | Est. Market Size ($/mo) |
|------|----------------------|-------------------------------|---------------------------|------------------------|
| Tier 1 | ... | ... | ... | ... |
| Tier 2 | ... | ... | ... | ... |
| Tier 3 | ... | ... | ... | ... |
| **Total P0** | ... | ... | ... | ... |

### Market Share and Potential

Per-tier share breakdown:

| Tier | Impression Share | Click Share | Cart Share | Purchase Share | Trend |
|------|-----------------|-------------|------------|---------------|-------|
| Tier 1 | ...% | ...% | ...% | ...% | ... |
| Tier 2 | ...% | ...% | ...% | ...% | ... |
| Tier 3 | ...% | ...% | ...% | ...% | ... |

Key observations on where we're strong, where we're weak, and whether share is growing or declining.

### Blockers & Growth Path

Present the blocker analysis in a table. For each tier, show whether Impression Share, CTR, and CVR are a blocker, healthy, or not applicable, along with the data points.

| Tier | Impression Share | CTR (Brand vs Industry) | CVR (Brand vs Industry) | Primary Blocker | Growth Path |
|------|-----------------|------------------------|------------------------|-----------------|-------------|
| Tier 1 | X% (of ~8-9% max) | X% vs X% | X% vs X% | [Impression Share / CTR / CVR] | [One-line: which pattern from CLAUDE.md Domain Knowledge this matches and recommended sequence] |
| Tier 2 | X% | X% vs X% | X% vs X% | ... | ... |
| Tier 3 | X% | X% vs X% | X% vs X% | ... | ... |

**How to fill the table:**
- **Impression Share:** Show the brand's current impression share. If below ~3%, it's a blocker.
- **CTR / CVR:** Show brand rate vs industry rate. If brand is significantly below industry (use judgment, but >20% gap is a good threshold), it's a blocker. Mark cells as "Blocker", "Healthy", or "N/A" (if the tier is not worth pursuing).
- **Primary Blocker:** The single biggest constraint for that tier.
- **Growth Path:** One line referencing the pattern (e.g., "PPC scaling: converts when visible, needs more impression share" or "Listing fix: CVR gap driven by missing A+ and weak bullets, fix before scaling traffic" or "Not capturable: intent mismatch, skip").

If a tier needs additional context beyond what fits in the table (e.g., explaining why it's not capturable, or a nuanced secondary blocker), add a brief bullet point below the table for that tier only.

### Insights

Clear observations from the data. Always reference tier and specific queries where relevant. Examples:
- "P0 (ReignDrop Ink Pad) has 13% purchase share on Tier 1 hero queries but only 2% on Tier 2. The Tier 2 market is 3x larger, making it the primary growth opportunity."
- "Impression share is strong across all tiers (8-10%), but CTR is consistently below industry. This is likely a listing/image issue, not a visibility issue."

### Things to Investigate Further

Hypotheses to verify in Step 4 (Ad Analysis). Examples:
- "Tier 2 impression share is low. Check in ad data whether we're bidding on these queries or missing them entirely."
- "CTR is below industry on broad queries. May need listing image optimization or better keyword targeting in titles."

### Questions for the Seller

Questions the data alone can't answer. If nothing stands out, leave blank.
