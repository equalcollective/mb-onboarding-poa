# Step 2: Product Understanding

## Objective

Build a deep understanding of the **hero ASIN (P0)** confirmed in Step 1 across six dimensions: the product itself, the customer, the brand, the competitive landscape, target keywords, and listing quality. This understanding is foundational - it informs every decision in Steps 3 and 4.

**Scope:** By default, this step is performed only on P0. After the P0 deep dive is complete, check with Aryan whether additional ASINs (P1, P2, P3) need the same treatment or if the information is sufficient for the call.

## Data Sources

**Keepa-sourced ASIN database** via Metabase MCP (Database: "Jeff Azure Db Public", ID: 4, schema: `orange_schema`):

| Table | Metabase ID | Purpose |
|-------|-------------|---------|
| `new_asins` | 260 | Product snapshot - title, features, description, images, category, brand, ratings |
| `new_asin_rating_history` | 267 | Historical rating changes over time |
| `new_asin_sales_rank_history` | 272 | Historical sales rank by category |
| `new_asin_signals` | 270 | Computed listing quality signals - title, bullets, A+, images, videos |

**How tables link:** All history tables reference `new_asins` via `new_asin_id` (foreign key → `new_asins.id`). To query history for a specific ASIN, first get its `id` from `new_asins` by filtering on `asin = '<ASIN>'`.

**Marketplace handling:** The database may contain the same ASIN across multiple marketplaces (US, UK, CA, and more over time). Do NOT hardcode `marketplace = 'US'`. Instead:
1. Query by ASIN without a marketplace filter first
2. If multiple marketplace rows exist, pick the one where the product is selling more - use review count (`rating_dist_total`) or other snapshot signals as a proxy
3. Use that marketplace's `id` for all subsequent history queries

**Data conventions:**
- **Ratings:** Stored as rating × 10 (integer). `45` = 4.5 stars.
- **Images:** Stored as JSONB: `{"main": "imageId.jpg", "variants": ["id1.jpg", ...], "count": N}`. To view, prefix with `https://m.media-amazon.com/images/I/`.

**Web search** for brand understanding and competitive landscape.

## Step 2a: Product & Customer

**Tool:** `execute_query` (Metabase MCP)

**Note:** Step 1 selected ASINs at the parent level. The Keepa data here is at the child level. Before querying, identify which child ASIN to use for P0. In most cases one child is enough (variations like color/size share the same product identity). If children under the parent are meaningfully different products, flag it and decide whether to pull more.

```sql
SELECT asin, marketplace, title, brand, description, features,
       category_tree, product_group, department,
       images, videos, aplus,
       current_rating, rating_dist_total,
       rating_dist_one_star, rating_dist_two_star,
       rating_dist_three_star, rating_dist_four_star,
       rating_dist_five_star,
       is_sns, id
FROM orange_schema.new_asins
WHERE asin = '<P0_CHILD_ASIN>'
```

If multiple rows (marketplaces) are returned, pick the one with the higher `rating_dist_total` and use that row's `id` going forward.

Then download and view the main product image:
1. Extract the `main` value from the `images` JSONB field
2. Construct URL: `https://m.media-amazon.com/images/I/{image_id}`
3. Download via curl and view

From this data + the image, build understanding of:

**Product:**
- One-line description of what the product is
- What makes it unique - key features, materials, formulation, design
- How it adds value - what problem does it solve
- Why people buy it - the core purchase motivation

**Customer:**
- Who is the target buyer (demographic, use case, skill level)
- What problem or need drives the purchase

## Step 2b: Brand Understanding

**Source:** Product data (brand name, images already viewed) + web search

Steps:
1. Note the brand name from the product data
2. Search the web for the brand's website and online presence
3. If not confident the correct website was found, ask Aryan for the URL

**Output:**
- Is this a generic/white-label brand or a registered brand with real identity?
- Brand maturity - new, established, DTC-first, Amazon-native?
- Notable brand signals - professional website, social media presence, retail distribution, other sales channels

## Step 2c: Competitive Landscape

**Source:** Web search (search for the product type/category, NOT the offers table)

Search for the product category to identify:
- Top 2-4 competitor brands in this space
- Their key products that compete directly with P0
- General price range in the market - this gives a proxy for where P0 sits (premium, mid-range, budget) without needing price history data

**Output:**
- Who are the main competitors (brand + product)?
- Where does P0 sit in the market - premium, mid-range, budget?
- Any obvious differentiation or gaps?

## Step 2d: Target Keywords

**Source:** Inferred from everything above - product understanding, customer profile, competitive landscape

Identify 5-10 keywords that a customer would search on Amazon to find this product. These feed directly into Step 3 (SQP Analysis) and Step 4 (Ad Analysis).

**Output:**
- Mix of high-intent specific terms and broader category terms
- Ordered by estimated relevance/volume (judgment call)

## Step 2e: Listing Quality & Conversion Drivers

**Tool:** `execute_query` (Metabase MCP)

Pull the computed listing signals for this ASIN. Reference: [Signals Documentation](https://mb-prod-docs.vercel.app/docs/jeff/signals)

**Table structure:** `new_asin_signals` uses an EAV (row-per-signal) format. Each row has: `signal_name`, `value_int`, `value_float`, `value_bool`, `value_text`, `computed_at`, `new_asin_id`.

```sql
SELECT signal_name, value_int, value_float, value_bool, value_text
FROM orange_schema.new_asin_signals
WHERE new_asin_id = <id from Step 2a>
  AND signal_name IN (
    'title_character_length', 'title_repeated_keywords_count', 'title_repeated_keywords',
    'title_has_brand', 'title_readability_score',
    'bullet_count', 'bullet_total_length',
    'bullet_1_length', 'bullet_2_length', 'bullet_3_length', 'bullet_4_length', 'bullet_5_length',
    'bullet_has_allcaps_first_word', 'bullet_readability_score',
    'has_aplus', 'aplus_module_count', 'aplus_is_premium', 'aplus_text_word_count',
    'aplus_image_count', 'aplus_has_alt_text',
    'image_count', 'main_image_optimization',
    'has_video', 'video_count', 'video_has_over_30_sec',
    'has_brand_store'
  )
ORDER BY signal_name
```

**Signals to skip:** Rating rates (`rating_rate_*`), pricing (`price_changes_*`, `has_coupon_*`), units sold (`units_sold_growth_*`), sales rank (`bsr_change_*`), PPC keywords. These are either unreliable or covered better by other steps.

**How to analyze:** Do not just report the raw signal values. Cross-reference them with what you already know:

- **From Step 2a:** You have seen the product images and read the title, bullets, and features. You know what the listing actually looks like.
- **From Step 2c:** You have researched competitors and seen their listings, images, titles. You know what "good" looks like in this category.
- **From Step 2d:** You know the target keywords. You can check whether the title and bullets actually contain them.

For each listing component (title, bullets, images, A+ content, video), only flag it if there is a genuine gap. If it's fine, skip it. The goal is actionable observations, not a checklist.

**Examples of good observations** (from ReignDrop baby ink pad, B07J298M2Z):

- **Title:** 184 chars, includes brand, no repeated keywords
  - Safety angle ("Safe & Gentle Acid Free") buried at the end, cut off on mobile
  - Middle section is marketing fluff ("Creates Impressive Long Lasting Keepsake Stamp")
  - Suggested rewrite: "ReignDrop Baby Ink Pad for Footprint & Handprint - Safe, Acid-Free, Easy to Wipe Off Skin - Smudge Proof Keepsake Stamp for Newborn, Infant & Toddler (Black)"
- **Bullets:** 3 bullets (should be 5)
  - Bullet 3 is a generic Amazon international product disclaimer, not a real selling point. Wasted slot.
  - Neither real bullet addresses the #1 parent concern: safety. "Safe for baby skin" and "acid-free" appear in the title but are never expanded in the bullets. Safety should be bullet #1.
  - Pet paw prints (strong secondary use case) is buried mid-sentence in the multipurpose bullet. Should lead or get its own bullet.
- **A+ Content:** Absent. Baby keepsake = emotional purchase (parents preserving memories). A+ with lifestyle images (finished keepsakes framed on a wall, parent with baby) and comparison module (this product vs messy traditional ink) would strengthen the emotional driver.
- **Video:** Absent. Product's biggest selling point is "easy, no-mess process" but there's no way to demonstrate that without video. A 30-second clip (ink pad to finished print to wiping baby's foot clean) would address the main purchase hesitation.

**Examples of bad observations (avoid these):**
- "Title length is 87 characters" (so what?)
- "Consider adding A+ content" (generic, no context)
- "Image count is 5" (just restating a number)
- "Bullets are short, avg 80 chars each. Competitors use 200+ chars." (character length is objective and obvious. What content should change?)

## Step 2f: Historical Trends

**Tool:** `execute_query` (Metabase MCP)

### Rating History

```sql
SELECT rh.timestamp, rh.rating
FROM orange_schema.new_asin_rating_history rh
WHERE rh.new_asin_id = <id from Step 2a>
ORDER BY rh.timestamp DESC
```

Rating is stored as integer × 10 (so `45` = 4.5 stars).

Look for:
- **Rating trajectory** - Improving, declining, or stable?
- **Recent drops** - Could indicate a product quality issue, bad batch, or listing hijack
- **Rating relative to category** - Context matters (4.2 in supplements ≠ 4.2 in electronics)

### Sales Rank History

```sql
SELECT srh.timestamp, srh.category_id, srh.rank
FROM orange_schema.new_asin_sales_rank_history srh
WHERE srh.new_asin_id = <id from Step 2a>
ORDER BY srh.timestamp DESC
```

Look for:
- **Rank trend** - Lower rank = more sales. Gaining or losing momentum?
- **Seasonality** - Recurring patterns (e.g., better in winter, worse in summer)
- **Spikes** - Sudden improvements (promotions?) or sudden drops (stockouts?)
- **Stability** - Stable good rank = consistent demand. High volatility = unpredictable.

### Output from Historical Trends

From rating + sales rank trajectories, add to the standard buckets **only if something stands out**:

- **Insights** - e.g., "P0 (Graphene Coating) rating dropped from 4.5 to 4.1 in last 3 months while sales rank worsened - likely connected"
- **Things to Investigate** - e.g., "P0 (Graphene Coating) sales rank improved significantly in Q4 - check if seasonal or promotion-driven in Step 3/4"
- **Questions for the Seller** - e.g., "P0 (Graphene Coating) rating has been declining steadily - any known product quality issues or recent changes?"

If nothing stands out, leave these blank.

## Step 2g: Output

Save the output to the seller's audit folder as `Product Understanding.md`.

### Product Profile: P0 - {Product Name}

**Product:**
- One-line description of what the product is
- What makes it unique - key features, materials, formulation, design
- How it adds value - what problem it solves
- Why people buy it - the core purchase motivation

**Customer:**
- Who is the target buyer (demographic, use case)
- What problem or need drives the purchase

**Brand:**
- Generic/white-label vs registered brand with real identity
- Brand maturity - new, established, DTC-first, Amazon-native
- Notable signals - website quality, social presence, retail distribution
- **Brand vibe** - the aesthetic and emotional tone the brand projects through packaging, images, website, and copy (e.g., "playful and colorful," "clinical and premium," "minimalist and modern"). If the brand has no discernible vibe, say so.

**Competitive Landscape:**
- **Price positioning (lead with this):** One line showing: average market price for the exact segment P0 competes in → P0's price → percentage difference. Example: "Avg standalone ink pad: $8.50 | P0: $7.99 | 6% below average"
- Top 2-4 competitor brands and their key competing products (table format)
- Market price range by tier (standalone, bundles, premium kits)
- Key differentiators or gaps

**Target Keywords:** 5-10 keywords ordered by estimated relevance

**Listing Quality and Conversion Drivers:**

Split into two sub-sections. The same components apply (Rating, Title, Bullets, Images, A+ Content, Video, Brand Store), but each component goes into exactly one sub-section based on whether it's a strength or an opportunity.

**Strengths:**
- Components where the listing is solid. Examples: strong main image, good title with relevant keywords, 5 well-written bullets, A+ content present with lifestyle imagery.
- Only include components that are genuinely strong. If nothing stands out as a positive, leave this blank.

**Opportunities:**
- Components where there's a genuine gap or improvement to be made. Examples: no A+ content on an emotional purchase product, safety messaging buried in title, only 3/5 bullets used, no video when the product's value prop requires demonstration.
- Only include components where something is genuinely worth flagging. Do not pad this section with minor nitpicks.

Rating (X.X from N reviews, note star distribution only if skewed) goes in whichever sub-section fits. A strong rating is a strength. A declining or low rating with skewed distribution is an opportunity.

**Rating Trajectory:** Improving / Stable / Declining + brief context

**Sales Rank Trajectory:** Improving / Stable / Declining / Seasonal + brief context

### Insights

Only include if genuinely insightful. Always reference as "P0 ({Product Name})".

### Things to Investigate Further

Hypotheses to verify in Steps 3 and 4.

### Questions for the Seller

Questions where the data alone cannot explain the "why". If nothing stands out, leave blank.
