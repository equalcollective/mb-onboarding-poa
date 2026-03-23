# Seller Central Audit Workflow

This is the workflow for performing a Seller Central level audit on a prospect's Amazon account. Within the MerchantBots sales process, this audit happens **before the second sales call** - the Sales lead performs this audit to prepare insights, a plan of action, and growth levers to present on the call.

## Required Tools

Three MCP servers and one direct API are required to execute this process:

1. **Metabase MCP** - Connected to the Keepa-sourced ASIN database (product snapshots, signals, price/rating/sales rank history). Used for product understanding (Step 2) via `execute_query` against `orange_schema` tables (`new_asins`, `new_asin_signals`, `new_asin_rating_history`, `new_asin_sales_rank_history`).
2. **Metabase API (direct)** - Used for ad analysis (Step 4) to execute saved questions from the "Campaign Analysis V1" dashboard. Uses the REST API directly via curl instead of the MCP tool. See [Ad Analysis.md](./Ad%20Analysis.md) for endpoint details, parameter format, and available questions.
3. **SQP Analyzer MCP** - Search Query Performance data from Brand Analytics. Used for market sizing, competitive positioning, and search funnel analysis (Step 3).
4. **Seller Analytics MCP (MB Onboarding MCP)** - Business report + advertising metrics from Seller Central. Used for ASIN selection (Step 1), performance trends, and critical issue detection.

## Important: This Is Not a Linear Process

These steps are written sequentially, but the actual analysis is iterative. You will:
- Do Step 1, build an initial understanding
- Move to Step 2 to deeply understand the products
- Move to Step 3 and Step 4, verify that understanding
- Come back to earlier steps to get more insights based on what you learned
- Verify again from Step 3 and Step 4

The workflow below is a guiding principle, not a rigid checklist. Insights from later steps will inform and sometimes change conclusions from earlier steps.

## Important: Only Add What You Are Confident About

Across every step of this workflow, do not add information, insights, or flags unnecessarily. If a bucket (Insights, Things to Investigate, Questions for the Seller) has nothing meaningful to add, leave it blank. Only include information you are genuinely confident is relevant and actionable. This applies to every step, not just the final output.

## Important: No Duplication Across Insight Buckets

Every observation goes in exactly ONE bucket. Never repeat the same fact across Insights, Things to Investigate, and Questions for the Seller. This is true for all reports across all steps.

**How to decide which bucket:**
- **Insights:** You are confident about BOTH the what and the why, and you have a solution or clear implication. This is a conclusion, not a hypothesis.
- **Things to Investigate:** You can dig deeper using the tools available (SQP, Seller Analytics, Metabase, web search). You might need to go back to a previous step or forward to a next step. If you can get to the answer yourself, it belongs here, not as a question.
- **Questions for the Seller:** You have no way to figure out the "why" from the data or tools. Only the seller knows. Include a hypothesis when asking (e.g., "Sales dropped 68% from Sep to Feb. Is this a normal seasonal pattern for balloon supplies, or has something changed?").

If you find yourself wanting to add the same fact to multiple buckets, you haven't decided what you actually know. Stop and decide.

## Important: High Readability

The whole point of this workflow is to significantly reduce the closer's time spent on the audit. The closer needs to be able to skim the output quickly and then explain to the brand owner how we will grow the brand. To ensure this:
- Remove all unnecessary information
- Use bullet points and sub-bullets for clarity. Dense paragraphs are harder to skim than structured pointers
- Whenever referencing a product in insights, investigation items, or questions, always include both the **priority level and product name** (e.g., "P2 (Graphene Coating for Cars)")

## Important: Writing Style

- **Never use long dashes (em dashes) anywhere in the output.** Use a comma, period, or restructure the sentence instead. Long dashes make text look AI-generated, and audit outputs may be shared with clients.
- Keep language natural and direct. Avoid overly polished or formulaic phrasing.

## Important: Post-Execution Retrospective

After completing the full audit workflow end to end for a seller, add any improvement ideas to [Version 2 Implementation Ideas.md](./Version%202%20Implementation%20Ideas.md). Each idea should include:
- The idea itself
- The specific example from the audit that triggered it
- Which step it applies to

## Domain Knowledge: Listing and Content

### A+ Content Best Practice
A+ content should be image-only with no standalone text modules. In 2026, shoppers are visual and do not want to read blocks of text on a product page. The ideal A+ layout is high-quality images with any necessary text (feature callouts, comparison info, FAQs) designed directly onto the images themselves. When evaluating a listing's A+ content, do not flag "zero text word count" as an opportunity. Instead, assess whether the images themselves communicate the key selling points effectively. The only time A+ text is a concern is when the images are generic or fail to convey product benefits, in which case the fix is better images with text overlays, not adding standalone text modules.

## Domain Knowledge: SQP and Market Analysis

### Impression Share Cap
Maximum impression share for a single brand on a single keyword is ~8-9%. This is because 1 search = 25 impressions (25 products load on the search results page). So a brand getting 1 impression out of 25 = 4% share. The only exception is when a brand has multiple products that can show up for the same keyword, because SQP is brand-level, not ASIN-level.

### Statistical Significance
When analyzing rates (CVR, CTR, ATC rate, share metrics), always check that the underlying numbers are large enough to be meaningful. A 30% conversion rate from 1 conversion and 3 clicks is not actionable. Do not highlight or build insights on metrics with low base numbers. Use judgment, but as a rough guide: be cautious with any rate derived from fewer than ~20 clicks or ~5 conversions per month.

### ICAP Funnel and Product-Level Blocker Detection

The core thesis: **Units Sold = Impressions x CTR% x CVR%**

Our job is to increase units sold. There are three levers, and any of them can be the blocker. The action plan flows directly from identifying which lever is the biggest blocker.

Note: The ICAP funnel has four stages (Impression, Click, Add to Cart, Purchase), but for blocker detection we use three metrics because Add to Cart and Purchase are bunched into CVR. If CVR is the blocker, then drill deeper into ATC rate vs Cart-to-Purchase rate to pinpoint where the drop-off happens.

#### Blocker 1: Impression Share

**How to detect:** Brand impression share is low (below ~3% of a max ~8-9%) on relevant tiers.

**PPC lever:** Bid on the keywords where impression share is low. This is the fastest, most deterministic way to increase visibility.

**Listing lever:** Add those keywords to the listing (title, bullets, backend) for better organic SEO and ranking.

#### Blocker 2: CTR (Click-Through Rate)

**How to detect:** Brand CTR is significantly below industry CTR on the same queries. The brand shows up but doesn't get clicked.

**PPC lever:** Better placements (Top of Search bid adjustments get higher CTR). Better targeting (more relevant keyword targeting so the product matches what the searcher expects to see).

**Listing lever:** There are 7 components that affect CTR on the search results page:
1. Main image
2. Price
3. Title
4. Ratings
5. Reviews (count)
6. Delivery times
7. Coupons and discounts

#### Blocker 3: CVR (Conversion Rate)

**How to detect:** Brand CVR is significantly below industry CVR (>20% gap). If CVR is the blocker, drill into ATC rate vs Cart-to-Purchase rate to find where the drop-off is.

**PPC lever:** Placement targeting (Top of Search converts better). Defensive ads (Sponsored Display on own listing to prevent competitor poaching, Product Targeting on own ASIN).

**Listing lever:** The components that affect CVR on the product detail page:
- Title
- Images (all, not just main)
- A+ Content
- B+ Content (brand story)
- Bullet points
- Price
- Delivery
- Brand story
- Ratings and reviews
- Return policy

**If Cart-to-Purchase rate is specifically the blocker:** Brand Tailored Promotions (retargeting abandoned cart customers). There are likely additional levers here that will be discovered over time.

#### How This Maps to the Action Plan

The action plan should:
1. Identify the biggest blocker (impression share, CTR, or CVR) per tier
2. Explain the blocker and the potential (how much growth is locked behind it)
3. Prescribe the solution, split into PPC levers (earlier weeks) and listing levers (later weeks)

Common patterns:
- **Low impression share + decent CVR:** PPC scaling opportunity. Bid on keywords, the brand converts when it shows up, it just needs to show up more.
- **Below-industry CTR + good CVR:** Listing presentation problem. Fix main image/price/title to win the search results page battle, then scale PPC.
- **Below-industry CVR + listing potential:** Listing optimization play. Fix the PDP first (A+ content, images, bullets), then scale traffic.
- **Low impression share + poor CVR:** The brand barely shows up AND doesn't convert when it does. Before scaling impressions, CVR must be addressed first, because this is a pay-per-click model: more clicks with poor conversion just burns money. Ask: "Can the CVR be fixed?" Cross-reference [Product Understanding.md](./Product%20Understanding.md) Step 2e (Listing Quality) to assess whether the listing has clear, fixable gaps (missing A+ content, poor images, weak bullets, bad title). If yes, the growth path is: fix the listing first, then scale PPC once CVR improves. If the listing is already reasonable and CVR is still poor, the issue is likely deeper: price positioning, low ratings/reviews, poor product-market fit for those queries, or delivery disadvantage. Assess honestly whether those blockers are solvable. If they are (e.g., price can be adjusted, reviews are growing), flag the specific fix needed before PPC scaling. If they are not (e.g., the product fundamentally doesn't match the query intent, ratings are low and declining with no fix in sight), then the tier is not capturable with the current product. Note this honestly and flag it as a product development opportunity if the market is large enough.
- **Maxed out potential:** Impression share is already high on Tier 1 (near the 8-9% cap), and Tier 2/Tier 3 queries are genuinely irrelevant to the product (the brand's impressions on those tiers don't convert, and they can't be made to convert because the product simply isn't what those searchers want). In this case, there is no meaningful growth left for this ASIN through search. The conclusion is: this product has reached its ceiling. Recommend moving on to the next ASIN (P1) to assess whether it has untapped potential instead.

### Branded Keywords and Branded Defense

**What branded keywords are:** Any search query that includes the brand's name in some form (e.g., "ReignDrop ink pad", "KeaBabies footprint kit"). These are searched by customers who already know the brand.

**Key principle: Branded keywords are not a growth lever.** Growth comes from non-branded (new-to-brand) keywords. Never recommend scaling spend on branded keywords, because that traffic already knows and intends to buy from the brand. Increasing spend there does not bring new customers.

**Branded defense campaign strategy:** A small percentage of ad budget (under 5%, often 2-3% for smaller brands) should be allocated to branded keywords as a defense campaign. The purpose is to prevent competitors from bidding on the brand's name and poaching customers who were already going to buy. This is protective, not growth-oriented.

**How to handle branded keywords in the audit:**
- **In SQP Analysis:** If branded queries appear with meaningful volume, tag them as "Branded" (separate from Tier 1/2/3). Report the volume and share metrics, but do not position them as a growth opportunity. Note them as existing brand equity.
- **In Ad Analysis:** Check whether the seller is running ads on branded terms.
  - If yes: note it, confirm spend is small (under 5%), and move on. If spend is disproportionately high on branded terms, flag it as misallocated budget that should be redirected to non-branded keywords.
  - If no: recommend launching a small branded defense campaign (2-5% of budget) to protect against competitor bidding on the brand name.
- **Never recommend scaling branded ad spend.** That is not true growth.

## Important: Account-Level vs. Product-Level Steps

Some steps in this workflow are **account-level** (analyzing the seller's overall performance) and some are **product-level** (deep-diving into a specific ASIN). As we map out Steps 3 and 4, each sub-step will be identified as one or the other.

- **Account-level steps** provide general talking points for the sales call - these can proceed without confirmation
- **Product-level steps** require confirmation on which ASIN to focus on before proceeding

## Important: Confirmation Before Product-Level Work

After Step 1 (ASIN Selection), you must get confirmation on which products to focus on before moving to product-level work in Steps 2, 3, or 4. Present the priority selection (P0-P3) and wait for approval or adjustments. Do not proceed with product-level deep dives until confirmed.

## Important: Parent vs Child ASIN Levels

Step 1 (ASIN Selection) operates at the **parent ASIN level** - the Seller Analytics data is aggregated by parent. From Step 2 onward, the Keepa data in Metabase is at the **child ASIN level**.

In most cases, a single child ASIN is sufficient to build the product understanding - variations (color, size, pack count) share the same product identity, customer, brand, and competitive landscape. However, be mindful of cases where children under a parent are meaningfully different products (e.g., a "Car Coating" and "Boat Coating" grouped under one parent). If the children are true variations, proceed with one child. If they represent different products, flag it and consider pulling additional children.

When starting Step 2, check the `parent_asin` field and whether other children exist under the same parent to make this judgment.

## Important: Hero ASIN First, Others Only If Needed

Product-level deep dives (Product Understanding, SQP insights, Ad insights) are performed **only on the hero ASIN (P0)** by default. After completing the P0 analysis, pause and check with Aryan whether the information is sufficient for the call or whether additional ASINs (P1, P2, P3) need the same treatment. Do not automatically deep-dive into all four products.

## Folder Structure

For every new seller audit, create a folder named after the seller inside `seller-central-audits/All Audits/`. All audit output files for that seller go inside this folder. Example:

```
seller-central-audits/
├── All Audits/
│   ├── Magic AutoCare/
│   │   └── ASIN Selection.md
│   ├── Brandvine/
│   │   └── ...
```

## Workflow Steps

### Step 1: ASIN Selection
Build catalog understanding, select a hero ASIN (P0) and focus ASINs (P1, P2, P3).
→ See [ASIN Selection.md](./ASIN%20Selection.md)

### Step 2: Product Understanding
Deep-dive into each confirmed ASIN - what is the product, who buys it, competitive landscape, pricing, rating trajectory, and sales rank history. Uses Keepa data via Metabase.
→ See [Product Understanding.md](./Product%20Understanding.md)

### Step 3: SQP Analysis
Tag P0-relevant queries, size the market, assess current market share, and identify funnel blockers preventing further capture. Product-level step.
→ See [SQP.md](./SQP.md)

### Step 4: Ad Analysis
Identify growth levers and mistakes in current ad campaigns. Contains both account-level and product-level sub-steps.
→ See [Ad Analysis.md](./Ad%20Analysis.md)

## Final Output

The audit produces a single consolidated file: `Seller Central Audit - {Seller Name}.md`, saved in the seller's audit folder. This is the only file that matters for the sales call. It serves three audiences with the same need:
1. **Aryan (closer):** Prep for the call, understand the full brand depth in one place
2. **The prospect:** May be sent to them or screen-shared on the call
3. **The service team:** First context document when onboarding a new client

The file must be self-contained. The reader should never need to look at the individual step files (ASIN Selection, Product Understanding, SQP Analysis, Ad Analysis) to understand the full picture.

**Length principle:** Deep enough to demonstrate expertise, concise enough to skim in 10 minutes. Every section earns its space. Do not pad.

### Section 1: Catalog Assessment

The priority table from ASIN Selection, unchanged:

| Priority | Product | 3-Mo Sales | 3-Mo Ad Spend | ROAS | TACoS | Organic Sales | Ad Sales % | Buy Box % | CVR | Trend |
|----------|---------|-----------|--------------|------|-------|---------------|-----------|-----------|-----|-------|
| P0 | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |
| P1 | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |
| P2 | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |
| P3 | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

Brief note on any products not prioritized and why (e.g., too small, no ad spend, dead).

### Section 2: Qualitative Product Understanding (P0)

**Product:**
- One-line description
- Key features, materials, formulation, design
- Value prop: what problem it solves
- Purchase motivation: why people buy it

**Customer:**
- Target buyer (demographic, use case)
- Purchase driver

**Brand:**
- Generic/white-label vs registered brand
- Maturity (new, established, DTC-first, Amazon-native)
- Notable signals (website, social, retail)
- Brand vibe

**Competitive Landscape:**
- Price positioning (one line: market avg, P0 price, % difference)
- Top 2-4 competitors (table format)
- Key differentiators or gaps

**Listing Quality:**

Split into two sub-sections. Each listing component (Rating, Title, Bullets, Images, A+ Content, Video, Brand Store) goes into exactly one based on whether it's a strength or an opportunity.

**Strengths:**
- Components where the listing is genuinely strong. If nothing stands out, leave blank.

**Opportunities:**
- Components with genuine gaps or improvements. No minor nitpicks.

Do not include target keywords here. They feed silently into the SQP section.

### Section 3: Quantitative Product Understanding (P0)

**Annual Trend:**

The compact table from ASIN Selection Step 5 showing key metrics at inflection points:

| Metric | [Period 1] | [Period 2] | [Period 3] | [Period 4] |
|--------|-----------|-----------|-----------|-----------|
| Total Sales | ... | ... | ... | ... |
| Sessions | ... | ... | ... | ... |
| CVR | ... | ... | ... | ... |
| Buy Box % | ... | ... | ... | ... |

Below the table, include 2-3 bullets summarizing the annual context only if they are significant and not repeated in the final Insights section. If they would be redundant, skip them.

**Rating Trajectory:** One line (Improving / Stable / Declining + context). Only include if notable.

**Sales Rank Trajectory:** One line (Improving / Stable / Declining / Seasonal + context). Only include if notable.

### Section 4: Market Opportunity (SQP)

**Tier Breakdown:**

For each tier, two sub-bullets:

- **Tier 1 (Hero):**
  - **Keywords:** [list of tagged queries]
  - **Rationale:** One line explaining the grouping and how it relates to the product

- **Tier 2 (Core market):**
  - **Keywords:** [list of tagged queries]
  - **Rationale:** One line

- **Tier 3 (Broad/adjacent):**
  - **Keywords:** [list of tagged queries]
  - **Rationale:** One line

**Market Sizing:**

| Tier | Monthly Search Volume | Monthly Add to Carts (Market) | Monthly Purchases (Market) | Est. Market Size ($/mo) |
|------|----------------------|-------------------------------|---------------------------|------------------------|
| Tier 1 | ... | ... | ... | ... |
| Tier 2 | ... | ... | ... | ... |
| Tier 3 | ... | ... | ... | ... |
| **Total P0** | ... | ... | ... | ... |

**Blockers & Growth Path:**

| Tier | Impression Share | CTR (Brand vs Industry) | CVR (Brand vs Industry) | Primary Blocker | Growth Path |
|------|-----------------|------------------------|------------------------|-----------------|-------------|
| Tier 1 | ... | ... | ... | ... | ... |
| Tier 2 | ... | ... | ... | ... | ... |
| Tier 3 | ... | ... | ... | ... | ... |

**ICAP Funnel Visual:**

Include a Mermaid funnel diagram for the single tier where the most growth opportunity exists. The diagram shows the brand's metrics at each funnel stage, highlights the blocker in red, healthy stages in green, and borderline in yellow. Use this template:

```
graph TD
    A["🔍 Impressions
    Share: X% (of ~8-9% max)
    STATUS"] --> B["👆 Clicks
    CTR: X% vs X% industry
    STATUS"]
    B --> C["🛒 Add to Cart
    ATC Rate: X% vs X% industry
    STATUS"]
    C --> D["💰 Purchases
    CVR: X% vs X% industry
    STATUS"]

    style A fill:#COLOR
    style B fill:#COLOR
    style C fill:#COLOR
    style D fill:#COLOR
```

Color coding:
- Red (`#ff6b6b`, white text): Blocker stage
- Green (`#51cf66`, white text): Healthy (at or above industry)
- Yellow (`#ffd43b`, dark text): Borderline (slightly below industry but not the primary issue)

Label the blocker stage with "⚠️ PRIMARY BLOCKER" or "⚠️ SECONDARY BLOCKER". Label healthy stages with "✅ Healthy" or "✅ Above industry".

Only one funnel diagram per audit, for the highest-growth-potential tier. If ATC rate and CVR need to be separated to pinpoint the drop-off, include the full 4-stage funnel. Otherwise, collapsing ATC and CVR into one stage is fine.

Below the blocker table and funnel, add 1-3 bullets for market share context or notable SQP observations (e.g., share declining across tiers, branded vs non-branded split, seasonal volume patterns).

### Section 5: Ad Analysis

This section establishes expertise and justifies the engagement cost. The prospect should see clearly what's broken, what to do about it, and what impact fixing it will have.

**Structure principle:** Every sub-section leads with the **step name as a header**, then presents findings under it. The reader should always know what aspect of the account is being analyzed before seeing any data or conclusions. Use the **Problem-Solution-Impact** format for any finding that has an actionable problem.

**Format for each finding:**

> **Finding: [Clear one-line problem statement]**
>
> **Problem:**
> - Data point that proves the problem
> - Second data point if needed
> - [Inline table if it adds proof]
>
> **Solution:**
> - Specific action 1
> - Specific action 2
>
> **Impact:**
> - $X in savings / additional sales
> - One-line math showing how the number was derived

Not every finding needs all three components. If something is healthy (e.g., auto vs manual split is fine), show the data table and a one-line summary. Only use the full Problem-Solution-Impact block when there is an actual problem worth calling out.

#### Account Level

Each account-level step gets its own **bold header** before any findings or tables. The structure is:

**Campaign Structure**

Present the campaign structure finding here. If campaigns are overstuffed, use the PSI format. If healthy, note it and move on.

**Auto vs Manual Split**

Always include the data table, then the finding (if any) below it.

| Targeting Type | Clicks | Spend | Sales | ROAS | AOV | CPC | CVR |
|----------------|--------|-------|-------|------|-----|-----|-----|
| Automatic | ... | ... | ... | ... | ... | ... | ... |
| Manual | ... | ... | ... | ... | ... | ... | ... |

**Campaign Profitability**

List unprofitable campaigns, then the PSI block.

**Targeting Strategy**

Always include both tables as context, then insights only if something stands out.

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

#### Product Level (P0)

**P0 Campaign Map**

| Campaign | Spend | Sales | ROAS | Clicks | Orders |
|----------|-------|-------|------|--------|--------|
| ... | ... | ... | ... | ... | ... |

Note total P0 ad spend vs total account ad spend.

**Blocker-Specific Findings**

Each blocker from Section 4 (SQP) gets its own sub-section that explicitly names the blocker and explains what PPC lever is being examined to address it. The reader must be able to trace: SQP blocker -> ad data -> proposed fix.

Structure each blocker sub-section as:

> **[Blocker Name] Blocker: [What's being analyzed]**
>
> Open with 1-2 sentences connecting back to the SQP finding. Example: "Section 4 identified impression share as the primary blocker on Tier 1 (0.64% of ~8-9% max). The PPC lever is bidding on the keywords where impression share is low. Here's what the ad data shows."
>
> Then present the relevant data table (search term table for impression share, placement table for CTR, wasted targeting table for CVR), followed by the PSI block.

Example headers:
- **Impression Share Blocker: Keyword Spend vs. Tier 1/2/3 Queries** - shows whether the brand is bidding on the keywords where it needs more visibility
- **CTR Blocker: Placement Distribution** - shows whether spend is going to high-CTR placements (Top of Search) or low-CTR placements (Product Pages)
- **CVR Blocker: Wasted Targeting & Irrelevant Search Terms** - shows non-converting targets consuming budget

The specific tables and analysis approach are defined in [Ad Analysis.md](./Ad%20Analysis.md) Steps 4e-4f. The key addition here is the explicit naming and tie-back: every product-level finding must open by referencing the blocker it addresses and close by connecting the solution back to the expected impact on that blocker.

### Section 6: Action Plan

Phased 8-week plan. Every action item must trace back to something already presented in the audit. Do not introduce new findings or recommendations here. The reader should be able to point to the earlier section where each action was justified.

**General rule: PPC levers first, listing levers later.** PPC actions go in earlier weeks because they are faster to implement and produce measurable results quickly. Listing actions go in later weeks because they take longer and their impact is harder to predict. Only reverse this if PPC levers are minimal or the listing has a critical issue that would waste any ad spend (e.g., buy box is broken, CVR is catastrophically low).

Each phase should open by connecting to the blocker: "The primary blocker is [X], so the first actions focus on [Y]." This creates a narrative thread from the SQP blocker analysis through to the action plan.

**Balance principle:** The action plan should feel substantial, like we are doing a lot in those 8 weeks. But every item must be real and justified by the data. Do not pad with generic or low-impact actions. If a phase genuinely has fewer actions, that's fine. Better to have 3 high-impact actions than 6 where half are filler.

#### Weeks 1-2: Immediate Actions
- Focus on PPC quick wins (pause unprofitable campaigns, reallocate budget, restructure overstuffed campaigns, increase Top of Search modifiers)
- Tie each action to the blocker it addresses

#### Weeks 2-4: Short-Term Optimizations
- Scale what's working from Weeks 1-2
- Begin listing content preparation (writing, not publishing)
- Extract and scale high-ROAS keywords into dedicated campaigns

#### Weeks 4-6: Medium-Term Growth
- Publish listing improvements (A+ content, bullets, images, video)
- Monitor CVR impact from listing changes
- Continue PPC scaling based on performance data

#### Weeks 6-8: Scaling and Evaluation
- Scale PPC on the improved listing
- Evaluate secondary products (P1, P2) for next phase
- Prepare for seasonal peaks if applicable

### Section 7: Insights & Questions for the Seller

Consolidated from all steps, deduplicated. Two buckets only:

**Insights:**
- Confident conclusions from the data. The "what" and "why" are both clear.
- Always reference product by priority and name (e.g., "P0 (Ink Pad)")
- Pick the most impactful insights across all steps. Do not dump everything.

**Questions for the Seller:**
- Questions where the data cannot explain the "why." Only the seller knows.
- Always include a hypothesis when asking (e.g., "Sales dropped 68% from Sep to Feb. Is this a normal seasonal pattern, or has something changed?")
- Pick the most relevant questions. Do not include questions that are nice-to-know but won't change the action plan.

No "Things to Investigate" bucket. By the time the final output is written, all investigation is complete. Anything that was worth investigating has either become an insight or a question for the seller.

## Publishing to Notion

After the audit is complete and all files are saved locally in `All Audits/{Seller Name}/`, publish the audit to Notion using the **Notion MCP** (`notion-create-pages` tool).

### Parent Page

All audit pages live under **"Seller Central Audits: MB Sales"** (page ID: `310fde5f528280ccac88e7b7a8373044`).

### Workflow

1. **Create the seller page** under "Seller Central Audits: MB Sales" with the seller name as the title (e.g., "ReignDrops")
2. **Create 5 child pages** under that seller page, each containing the full markdown content from the corresponding local file:
   - ASIN Selection
   - Product Understanding
   - SQP Analysis
   - Ad Analysis
   - Seller Central Audit - {Seller Name} (the final consolidated file)

Use `notion-create-pages` with `parent: { page_id: "<seller_page_id>" }` for the child pages. All 5 child pages can be created in a single batch call.

### Notes

- The final consolidated file ("Seller Central Audit - {Seller Name}") is the primary document for the sales call. The other 4 files provide depth if needed.
- Convert markdown content as-is. Notion's markdown renderer handles tables, bullet points, bold text, and headers natively.
- **Always include mermaid diagram blocks in the Notion content.** Do not drop them during publishing. Notion renders mermaid code blocks as raw text by default, but Aryan can toggle them to render visually in the UI. The visual funnel diagram is valuable for the sales call, so it must be present in the Notion page.
- Do this as the last step after the full audit is complete and reviewed.
