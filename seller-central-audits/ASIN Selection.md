# Step 1: ASIN Selection

## Objective

Build an overall catalog understanding of the brand and assign priority levels:
- **P0 (Hero ASIN)** - The primary ASIN to go deepest on.
- **P1, P2, P3 (Focus ASINs)** - The next three ASINs in order of priority.

## Step 0: Data Readiness Check

**Tool:** `get_data_coverage(seller_name)`

1. Check `ads_day_count`:
   - **>= 70 days:** Proceed with the last 3 complete months
   - **< 70 days:** Flag this. Fall back to the months where ad data actually exists (could be 2 months). Only analyze the period covered by ad data.
2. Calculate date range: Use `biz_max_date` to determine the latest complete month, then go back 3 months (or fewer if ad data is limited).

## Step 1: Pull Catalog Data

**Tool:** `get_pivot_table`

```
get_pivot_table(
  seller_name="...",
  granularity="monthly",
  aggregation_level="parent",
  start_date="<calculated>",
  end_date="<calculated>",
  metrics=["total_sales", "units", "ad_spend", "roas", "tacos_pct",
           "cvr_pct", "organic_sales", "ad_sales", "ad_sales_pct",
           "buy_box_pct", "ad_cvr_pct"]
)
```

This returns one row per parent ASIN with monthly columns for each metric.

## Step 2: Check for Seller Guidance

If the seller has specified which product to focus on, use that as a guiding principle - weight that product more heavily. But still analyze the full catalog data and form your own understanding. Do not skip the analysis.

## Step 3: Select Hero ASIN (P0)

**Primary criterion:** Highest total sales (sum across the months).

**When it's not clear-cut** (products have similar sales levels):
- Look at trend direction - prefer a growing ASIN over a flat one (doubling down on growth)
- The reverse can also be true - a declining ASIN may need urgent attention
- Use judgment; the "right" answer will often become clearer after SQP and Ad analysis in later steps

## Step 4: Select Focus ASINs (P1, P2, P3)

From the remaining parents, rank by:
1. **Total sales** (next three highest)
2. **Cross-check against ad spend** - confirm these are products the seller is actively investing in
3. If sales and ad spend don't align (e.g., high ad spend but low sales), note that as something to investigate

## Step 5: P0 Annual Trend Check

**Tool:** `get_metrics`

Pull 12 months of data for the P0 ASIN to understand longer-term trajectory before diving into product-level analysis.

```
get_metrics(
  seller_name="...",
  granularity="monthly",
  parent_asin="<P0 parent ASIN>",
  start_date="<12 months before end_date>",
  end_date="<latest complete month>",
  metrics=["total_sales", "sessions", "cvr_pct", "buy_box_pct"],
  include_comparison=true
)
```

**What to look for:**

- **Sales trajectory:** Is it growing, declining, flat, or seasonal? A 12-month view reveals patterns that the 3-month selection window misses.
- **Session trajectory:** Are sessions tracking with sales, or diverging? Sessions declining faster than sales suggests price increases masking a traffic problem. Sessions growing while sales are flat suggests a conversion problem.
- **CVR health:** Is conversion rate stable, improving, or degrading over time? A CVR that was 25% six months ago and is now 15% tells a different story than one that has always been 15%.
- **Buy box consistency:** Did they have buy box issues at specific points? A buy box that dropped from 95% to 70% three months ago points to a specific event (new competitor, pricing issue, stock-out).

**Output:** 2-3 bullet points summarizing the annual context. These observations carry forward into Product Understanding and SQP Analysis. Do not repeat the raw numbers, focus on the pattern and what it means.

## Step 6: Output

### Priority Table

| Priority | Product | 3-Mo Sales | 3-Mo Ad Spend | ROAS | TACoS | Organic Sales | Ad Sales % | Buy Box % | CVR | Trend |
|----------|---------|-----------|--------------|------|-------|---------------|-----------|-----------|-----|-------|
| P0 | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |
| P1 | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |
| P2 | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |
| P3 | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

Trend is an intuitive assessment (Growing / Flat / Declining) based on the monthly progression - no fixed threshold, just judgment from an Amazon sales perspective.

### P0 Annual Trend

A compact table showing key metrics at a few relevant time periods (e.g., 12 months ago, 6 months ago, 3 months ago, latest month), followed by 2-3 bullet points of context. Pick the time periods that best illustrate the pattern (peak, trough, inflection points). Not all four columns are required every time, use what tells the story.

| Metric | [Period 1] | [Period 2] | [Period 3] | [Period 4] |
|--------|-----------|-----------|-----------|-----------|
| Total Sales | ... | ... | ... | ... |
| Sessions | ... | ... | ... | ... |
| CVR | ... | ... | ... | ... |
| Buy Box % | ... | ... | ... | ... |

- Bullet 1: Overall trajectory and what it means
- Bullet 2: Any divergence or notable pattern (e.g., sessions dropping but sales stable)
- Bullet 3 (if needed): Buy box or CVR specific observation

### Insights

Observations that are clear from the data. Examples:
- Buy box is low on a product → buy box is a constraint
- Capital allocation issue: spending heavily on a low-ROAS product when another product has better returns
- A product is almost entirely ad-dependent (high ad sales %) with near-zero organic

Only include insights you are confident about. If nothing stands out, leave this blank.

### Things to Investigate Further

Questions or patterns that *could* become insights after SQP and Ad analysis. These are hypotheses, not conclusions. Examples:
- "Product X has a high CVR but low sessions - is there a traffic problem?"
- "ROAS dropped significantly in the latest month - need to check ad campaigns"

These will be investigated in later steps (Product Understanding, SQP, Ad Analysis). Only include if genuinely worth investigating. If nothing stands out, leave this blank.

### Questions for the Seller

Data points where the "why" cannot be answered from the data alone. These become questions to ask the prospect on the call. Examples:
- "Sales for Product X are very volatile - is this seasonal or stock-related?"
- "No ad spend on Product Y - is this intentional?"

Only include questions that are relevant and that you genuinely cannot answer from the data. If nothing stands out, leave this blank.
