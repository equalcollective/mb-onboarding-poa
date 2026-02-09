# Listing Optimization System

> **Purpose:** The complete product design for the listing optimization workflow — a structured pipeline that transforms raw product inputs into a fully optimized Amazon listing strategy.
>
> **Audience:** Product managers, account managers, and the tech team building this system.
>
> **How to read this:** Each phase explains *what* we do, *why* it matters, *what it produces*, and *how it looks* using Maurice's Piggie Park BBQ Sauce as a running example.

---

## Table of Contents

1. [The Problem & Vision](#1-the-problem--vision)
2. [Pipeline Overview](#2-pipeline-overview)
3. [Inputs](#3-inputs)
4. [Phase 1: Research & Discovery](#4-phase-1-research--discovery)
5. [Phase 2: Brand Understanding](#5-phase-2-brand-understanding)
6. [Phase 3: Product Knowledge](#6-phase-3-product-knowledge)
7. [Phase 4: Positioning](#7-phase-4-positioning)
8. [Phase 5: Buyer Personas](#8-phase-5-buyer-personas)
9. [Phase 6: Product Attributes](#9-phase-6-product-attributes)
10. [Phase 7: Keyword Strategy](#10-phase-7-keyword-strategy)
11. [Phase 8: Listing Real Estate Allocation](#11-phase-8-listing-real-estate-allocation)
12. [Phase 9: Listing Copy — Title & Bullets](#12-phase-9-listing-copy--title--bullets)
13. [Phase 10: Design Brief — Images & A+ Content](#13-phase-10-design-brief--images--a-content)
14. [Compliance & Suppression Guardrails](#compliance--suppression-guardrails)
15. [Amazon Ecosystem Features to Consider](#amazon-ecosystem-features-to-consider)

---

## 1. The Problem & Vision

### The Problem

Most Amazon listings **describe** the product. They don't **sell** it.

Someone writes bullets based on what they *think* matters, picks images based on what *looks nice*, and stuffs keywords based on what *seems relevant*. The result: a listing that works as a product spec sheet but fails as a sales machine.

Selling requires a fundamentally different process:
1. **Know the brand and product deeply** — not just what it is, but what the brand stands for, what cultural and emotional context the product carries, and where the uniqueness actually lives
2. **Know where you stand** — how is this product positioned? Who are you really competing with? Are you a mass-market option or a niche treasure?
3. **Know the buyer deeply** — not demographics, but decision drivers, objections, and the things they need to see to convert but didn't know to ask for
4. **Map product truths to buyer needs** — which attributes matter to which buyers, which are they aware of and which are hidden conversion drivers, and where on the listing each should appear
5. **Express those mappings** across every inch of listing real estate — title, bullets, images, A+ content — with surgical precision about what goes where and why

### The Vision

A structured, phased pipeline that takes raw inputs (an ASIN, a website URL, some competitor links, a brand story) and produces:

- A deep brand and product knowledge base
- A clear market positioning with competitor mapping
- Evidence-based buyer personas
- A prioritized attribute list tagged by type, awareness level, and persona relevance
- A keyword strategy built from all upstream intelligence
- A listing real estate allocation map — what goes where and why
- Multiple title and bullet point options
- Comprehensive design briefs for images and A+ content

Every output builds on the previous one. Every claim is evidence-backed. The AM validates at every checkpoint before the system proceeds.

### What This Is NOT

This is not a one-shot "generate a listing" tool. It's a **structured thinking framework** that produces artifacts at each stage. The AI does the heavy research, synthesis, and generation — the AM validates and steers. The result is a listing strategy that's both deeply informed and human-approved.

---

## 2. Pipeline Overview

### The Phases

```
 UNDERSTAND                    STRATEGIZE                     EXECUTE
 ──────────────────────────    ────────────────────────────    ──────────────────────────
 Phase 1: Research             Phase 6: Product Attributes    Phase 9:  Listing Copy
 Phase 2: Brand Understanding  Phase 7: Keyword Strategy      Phase 10: Design Brief
 Phase 3: Product Knowledge    Phase 8: Real Estate Allocation
 Phase 4: Positioning
 Phase 5: Buyer Personas
```

### Phase Flow & Dependencies

| # | Phase | Depends On | Produces | AM Checkpoint? |
|---|-------|-----------|----------|----------------|
| 1 | Research & Discovery | Inputs | Evidence corpus (reviews, competitors, cultural data, search data) | Yes |
| 2 | Brand Understanding | Phase 1 + Inputs | Brand identity, founder story, brand voice, values | Yes |
| 3 | Product Knowledge | Phase 1 + 2 | Product essence, uniqueness, value props, subjective/objective classification | Yes |
| 4 | Positioning | Phase 1 + 2 + 3 | Positioning statements, competitor map (direct/indirect), mass vs niche | Yes |
| 5 | Buyer Personas | Phase 1 + 3 + 4 | Persona profiles with decision drivers and objections | Yes |
| 6 | Product Attributes | Phase 3 + 4 + 5 | Tagged, prioritized, aware/unaware classified, persona-mapped attributes | Yes |
| 7 | Keyword Strategy | Phase 1 + 4 + 5 + 6 + SQP + Ads Data + Data Dive | Categorized, prioritized keywords from all upstream intelligence | Yes (AM approves P0/P1) |
| 8 | Real Estate Allocation | Phase 5 + 6 + 7 | Product-specific map of what goes where on the listing | Yes |
| 9 | Listing Copy | Phase 6 + 7 + 8 | Title options, bullet options, backend keywords | Yes |
| 10 | Design Brief | Phase 2 + 3 + 5 + 6 + 8 | Image slot briefs, A+ module briefs, trust threading map | Yes |

### Key Principle: Each Phase Feeds the Next

Nothing is generated in isolation. The keyword strategy in Phase 7 isn't based on a keyword tool alone — it's informed by who the buyers are (Phase 5), what attributes matter (Phase 6), how the product is positioned (Phase 4), and what cultural language surrounds it (Phase 2). Every phase is richer because of the phases before it.

---

## 3. Inputs

The system accepts multiple input types. More inputs produce richer outputs, but even a single ASIN can start the pipeline.

| Input | What It Gives Us | Required? |
|-------|-----------------|-----------|
| **Existing Amazon listing** (ASIN) | Current title, bullets, images, reviews, Q&A, price, rating, variants, "also viewed" competitors | Any one input is enough to start |
| **D2C website** | Brand story, product descriptions (often richer than Amazon), visual identity, testimonials, FAQ | Optional |
| **Social media** (Instagram, TikTok) | Brand voice, content that resonates, user-generated content, lifestyle context | Optional |
| **AM/client knowledge dump** | Insider knowledge, origin stories, founder context, differentiation the internet doesn't know | Optional |
| **Competitor list** (ASINs + off-Amazon) | Competitive positioning, gaps, what their customers love/hate | Optional |
| **Brand facts** | Certifications, values, founder story, history, manufacturing details, social cause | Optional |
| **SQP data** (Search Query Performance) | Click share, conversion share, impression share for keywords we already rank for | Optional |
| **Amazon Ads data** (Search Term Report, campaign data) | Actual converting search terms, high-spend/low-conversion terms, auto discoveries | Optional |
| **Data Dive / market tools** | Category-level search volume, keyword opportunities, competitor keyword rankings | Optional |

### Input Quality → Output Quality

| What You Provide | Output Quality |
|-----------------|---------------|
| Just an ASIN | Good (70%) — enough to research, but missing insider context |
| ASIN + competitors | Better (80%) — competitive context sharpens everything |
| ASIN + competitors + D2C + brand context | Great (90%) — full picture |
| All of the above + SQP + Data Dive | Maximum (100%) — data-backed at every step |

---

## 4. Phase 1: Research & Discovery

### What We're Doing
Digging deep into the product from every angle — not just reading the listing, but understanding the cultural context, mining reviews (ours and competitors'), studying search behavior, and finding evidence from Reddit, blogs, and media.

### The Seven Research Domains

**1. Product Deep-Dive**
What is this product at its most fundamental level? What are the ingredients, materials, or components? What's the manufacturing process — and does the process matter to buyers? What certifications or standards does it meet?

**2. Cultural & Historical Context**
Does this product have regional or cultural significance? Is there a heritage story? A trend it connects to (clean eating, artisan food, sustainability, K-beauty)? Are there cultural moments that spike demand? What does Reddit say about this category?

*Evidence sources:* Reddit threads, food/beauty/hobby blogs, YouTube reviews, news articles, listicles ("Best BBQ Sauces of 2026"), social media engagement patterns.

**3. Competitive Landscape & Competitor Listing Analysis**
Who competes directly (same sub-category)? Indirectly (adjacent products solving the same problem)? What does each competitor do well and poorly? Where is the white space?

This is one of the most important research domains because **competitor Amazon listings are the closest reference for how products are actually sold on Amazon.** They show us what attributes the market thinks are worth promoting, what messaging approaches win, and — critically — where there are gaps we can own.

*Per competitor, we analyze at two levels:*

**Level 1 — Product & Market Position:**
- Positioning (premium, value, artisan, mass-market)
- Price and price-per-unit
- Rating, review count, BSR
- Variant strategy (how many variants, what roles they play)

**Level 2 — Listing Teardown (this is where the gold is):**

We break down every competitor's listing element-by-element:

*Title Analysis:*
- What keywords do they lead with?
- What attributes do they put in the title?
- What's their title structure/formula?
- What's missing from their title that we could include?

*Bullet Point Analysis:*
- What attributes/benefits do they promote in each bullet?
- What's their bullet structure (feature-led, benefit-led, story-led)?
- What claims do they make?
- What do they NOT talk about that their reviews say matters?

*Image Analysis:*
- What image types do they use? (studio, lifestyle, infographic, comparison)
- What attributes do they visualize?
- What's the quality level? Where are they lazy?
- Do they have video?

*A+ Content Analysis:*
- Do they have A+ / Premium A+?
- What modules do they use?
- What story do they tell?
- Do they use a comparison chart? Against whom?

*Review & Q&A Analysis:*
- What do their customers love? (exact themes)
- What do their customers complain about? (gaps we can fill)
- What questions come up repeatedly? (information their listing fails to provide)

*Keyword Strategy (Reverse-Engineered):*
- What keywords are in their title?
- What keywords are in their bullets?
- What keywords are they likely targeting in ads? (from sponsored placements)

**The Gap Analysis:**

After analyzing competitors, we produce a gap matrix — attributes that matter to buyers (from reviews) but that competitors are NOT promoting well in their listings:

```
Attribute         Buyer Cares?   Competitor A    Competitor B    Competitor C    Gap for Us?
────────────────  ────────────   ────────────    ────────────    ────────────    ──────────
Heritage/origin   Yes (reviews)  Not mentioned   Weak mention    Not mentioned   ✅ BIG GAP
Clean ingredients Yes (reviews)  In bullet 4     Not mentioned   In title        ✅ MODERATE
Versatility       Yes (Q&A)      Not mentioned   Bullet 5        Not mentioned   ✅ GAP
Taste description Yes (reviews)  Generic         Good            Generic         ⚠️ SMALL
Price value       Yes (reviews)  Mentioned       Not mentioned   Mentioned       ⚠️ SMALL
```

Gaps where competitors are silent but buyers care = our biggest opportunities. These gaps should become P0 or P1 attributes in Phase 6.

**4. Review Mining**
The most valuable evidence source. Reviews are unfiltered customer voice.

*Our reviews:* What do 5-star reviewers specifically praise? What do 1-3 star reviewers complain about? What use cases do they describe? What comparisons do they make? ("better than X", "switched from Y")

*Competitor reviews:* What do their customers wish was different? What gaps exist that we can fill?

*Q&A section:* What pre-purchase questions keep coming up? These reveal objections and information gaps.

**5. Search Behavior**
What do people actually type when looking for this product? What are the auto-complete suggestions? What are the "Customers also searched for" suggestions? What keywords do top competitors rank for?

**6. Category Norms**
What does a typical listing look like in this category? What image styles dominate? What price range is "normal"? What bullet themes are common? What A+ content modules are popular? What review count is considered "established"?

**7. Brand Ecosystem**
What else does this brand sell on Amazon? How does this product fit in the portfolio? Is there cross-sell potential? What does the brand store look like?

### Additional Data Sources (When Available)

**SQP (Search Query Performance) Data:**
If we have access, SQP shows us exactly which keywords we already rank for, our click share vs competitors, and our conversion share. This is gold for Phase 7 (Keywords) but we collect it here.

**Data Dive / Market Research Tools:**
Tools like Helium 10, Jungle Scout, or Brand Analytics provide category-level search volumes, keyword opportunity gaps, and competitor keyword rankings. These supplement our manual research.

### Maurice's Example: Key Research Findings

```
PRODUCT FACTS:
• Mustard-based BBQ sauce (South Carolina regional style)
• 2-pack, 36oz total (2 × 18oz), $22-24 ($0.61/oz)
• 4.7 stars, 989 reviews, Amazon's Choice
• All-natural, no HFCS, no preservatives
• From Maurice's Piggie Park restaurants (est. 1953), same recipe for 70+ years

CULTURAL CONTEXT:
• SC mustard BBQ is one of four regional US BBQ styles
  (KC tomato, TX vinegar-pepper, NC vinegar, SC mustard)
• Most Americans default to tomato-based = education opportunity
• Reddit r/BBQ: "Mustard BBQ is the most underrated sauce style"
• Southern food tourism trend = growing interest in regional authenticity
• BBQ competitions and food festivals drive awareness

COMPETITOR INSIGHTS:
• Cattlemen's: Mass-market, 1/3 our price, 1,642 reviews
  Reviews: "decent flavor" but "generic", "watery", "not authentic"
• Lillie's Q: Artisan, Chicago-based (not SC), $0.45/oz
  Reviews: "complex flavor" but "too sweet", "expensive for size"

COMPETITOR LISTING TEARDOWN:
• Cattlemen's Title: Leads with "Carolina Tangy Gold" — no heritage, no origin story
  Bullets: Generic benefit language, mentions "No HFCS" in bullet 3
  Images: Basic product shots, no lifestyle, no infographics
  A+: Minimal — no brand story, no comparison chart
  Gaps: NO heritage story, NO restaurant connection, NO cultural context,
        NO education about what mustard BBQ is

• Lillie's Q Title: Leads with "Gold Barbecue Sauce" — mentions Chicago, not SC
  Bullets: Well-written but focused on "craft" and "small batch"
  Images: Good lifestyle shots but not BBQ-specific
  A+: Clean design, brand story about Chicago chef
  Gaps: NOT from SC (major authenticity gap), no restaurant heritage,
        positioned as "chef-driven" not "tradition-driven"

GAP ANALYSIS:
• Heritage/origin: NEITHER competitor promotes restaurant heritage → BIG GAP for us
• SC authenticity: Cattlemen's is mass-market, Lillie's is Chicago → GAP
• "Education about mustard BBQ": Nobody explains what it IS → OPPORTUNITY
• Clean ingredients: Cattlemen's buries it, Lillie's doesn't mention → MODERATE GAP
• Versatility / use cases: Both are weak on showing uses → GAP

OUR REVIEW THEMES:
• Love: "Best mustard BBQ", "just like the restaurant", "perfect on pulled pork"
• Hate: "Too tangy for some", "expected more heat", "price seems high"
• Repeat buyers say: "Worth every penny — I order this every few months"

SEARCH DATA:
• "bbq sauce" — 100K+/mo (master keyword)
• "mustard bbq sauce" — 5-10K/mo
• "carolina bbq sauce" — 3-5K/mo
• "maurice's bbq sauce" — 1-2K/mo (branded)
• "south carolina bbq sauce" — 1-2K/mo
• "yellow bbq sauce" — 500-1K (emerging term)
```

**AM Checkpoint:** "Did we miss any competitors? Any facts wrong? Any insider knowledge to add? Do you have SQP or Data Dive data to plug in?"

---

## 5. Phase 2: Brand Understanding

### What We're Doing
Before we can sell the product, we need to understand the brand behind it. A product from a founder-led craft company with a social mission is sold completely differently from the same physical product made by a faceless manufacturer. The brand story is a conversion lever.

### What We Need to Understand

**Brand Identity:**
- What does this brand stand for? What's the core brand promise?
- How long has the brand existed? What's the origin story?
- Is there a founder? Is it a founder-led brand? Does the founder story add value?
- What are the brand's values? (quality, authenticity, sustainability, innovation, etc.)

**Brand Story & Heritage:**
- Is there a compelling story behind the brand? (heritage recipe, family business, breakthrough invention, personal struggle → solution)
- Does the story matter for conversion? (A heritage food brand's story matters enormously. A commodity brand's story matters less.)
- Can the story be told visually? (old photos, factory shots, founder in action)

**Social Cause or Mission:**
- Is there a social cause attached? (sustainability, charity, community support)
- Does the brand take a stand? (organic-only, fair trade, minority-owned, veteran-owned)
- Is this a conversion factor for the target buyer? (for some categories like beauty, food, baby products — mission matters a lot)

**Brand Voice & Local Culture:**
This is about capturing the *authentic voice* of the product. If this product were being sold in its home market by someone who truly knows it, how would they talk about it? What words would they use? What tone?

This is not about marketing polish — it's about authenticity. A South Carolina BBQ sauce should sound like it comes from a pit master, not a Madison Avenue copywriter. A Japanese skincare brand should carry the precision and care of J-beauty. A Brooklyn craft hot sauce should feel scrappy and bold.

We capture:
- The natural language of the product's world
- Phrases, idioms, or terms that insiders use
- The tone (warm, authoritative, playful, reverent, no-nonsense)
- Words that would feel *wrong* for this brand

### Maurice's Example: Brand Understanding

```
BRAND IDENTITY:
• Maurice's Piggie Park — South Carolina BBQ restaurant chain since 1953
• Not a "made-for-Amazon" brand — it's a real restaurant with physical locations
• The brand IS the restaurant. The sauce IS the restaurant experience, bottled.

FOUNDER STORY:
• Founded by Maurice Bessinger in 1953
• Built one of the most famous BBQ chains in South Carolina
• The mustard BBQ recipe is his original — never changed in 70+ years
• This is a founder-led brand in spirit even if the founder has passed
  — the legacy and recipe ARE the brand

BRAND VALUES:
• Authenticity — "the real thing, not a knockoff"
• Tradition — "same recipe since 1953"
• Quality ingredients — "all-natural, no shortcuts"
• Regional pride — "South Carolina's BBQ tradition"

SOCIAL CAUSE / MISSION:
• Not a social-cause brand, but a HERITAGE brand
• The "cause" is preservation of regional food traditions
• This resonates with the growing movement of supporting authentic,
  small-batch, origin-specific food over mass-market homogenization

BRAND VOICE & LOCAL CULTURE:
How would a Piggie Park pit master talk about this sauce?

"This ain't your grocery store BBQ sauce. This is Southern Gold —
the same mustard BBQ we've been serving at Piggie Park since your
grandparents were coming through. It's tangy, it's bold, and there's
nothing else like it outside of South Carolina. Once you try it on
pulled pork, you'll wonder why you ever settled for ketchup-based sauce."

Key voice notes:
• Warm, confident, a little proud — not boastful
• Uses "Southern Gold" as a term of endearment
• References tradition and heritage naturally
• Slightly educational — "this is what real SC BBQ is"
• Would NEVER sound corporate, slick, or generic
• Words that fit: authentic, legendary, heritage, tradition, Southern Gold
• Words that DON'T fit: innovative, cutting-edge, disrupting, revolutionary
```

**AM Checkpoint:** "Does this capture the brand accurately? Does the client have a founder story they'd want us to use? Is the brand voice right? Any brand guidelines we should know about?"

---

## 6. Phase 3: Product Knowledge

### What We're Doing
Synthesizing all research into a deep, structured understanding of what this product **truly is** — its essence, its uniqueness, its value hierarchy, and crucially: how subjective or objective it is as a purchase, which fundamentally changes how we sell it.

### Product Essence: Five Layers

Every product has five layers of identity:

| Layer | Question | Maurice's |
|-------|----------|-----------|
| **Category** | What is it? | Mustard-based BBQ sauce |
| **Functional** | What does it do? | Flavors and enhances grilled/smoked meats |
| **Emotional** | What does it mean to the buyer? | A taste of South Carolina heritage |
| **Cultural** | What larger context does it sit in? | One of four great American BBQ traditions — the most underrated one |
| **Origin** | Where does it come from and why does that matter? | Maurice's Piggie Park, SC, est. 1953 — same recipe for 70+ years |

### Uniqueness Analysis: Where Does the Uniqueness Live?

| Dimension | What's unique? | Rare? | Defensible? | Buyer cares? |
|-----------|---------------|-------|-------------|-------------|
| **Recipe/Formulation** | Mustard-based (only ~5% of BBQ sauces) | High | Medium | Yes — it's the core reason to buy |
| **Process** | Same recipe, same facility as the restaurants | Medium | High | Yes — validates authenticity |
| **Origin** | From a legendary 70+ year SC restaurant chain | High | High | Very much — heritage can't be faked |
| **Experience** | Tangy, bold, completely different from tomato BBQ | High | Low | Yes — but hard to convey without tasting |
| **Category** | Defines an underrepresented sub-category | High | High | Yes for the educated buyer; needs teaching for others |

### Value Proposition Hierarchy

```
CORE (the ONE thing — if they remember nothing else, remember this):
"Authentic South Carolina mustard BBQ sauce from a legendary 70-year restaurant chain"

PRIMARY (directly supports the core):
1. Restaurant heritage = proven, trusted recipe
2. Mustard-based = unique flavor you can't find elsewhere
3. All-natural ingredients, no HFCS

SECONDARY (adds value but isn't the main reason to buy):
1. 2-pack format = value for regular users
2. Versatile — pork, chicken, ribs, dipping, marinades
3. 989 reviews at 4.7★

TERTIARY (nice-to-have for specific segments):
1. Makes a great gift
2. Prime shipping
3. Multi-flavor variety available
```

### The Subjective–Objective Spectrum

This is a critical classification that changes how we sell the product.

**Objective products** are purely functional. A USB cable, a banana, a set of screws. You know exactly what you're getting. The buyer cares about: quality, trust, price. The listing should be clear, factual, and efficient. There's not much emotional selling to do — just prove competence and value.

**Subjective products** have an emotional or personal dimension. Food, beauty, fashion, home decor, fragrances. Different buyers want different things. Taste is personal. Style is personal. The "right" product depends on who you are. The listing must do more work: it must evoke, persuade, educate, and build enough trust that the buyer takes a chance on something they can't try before buying.

Most products fall somewhere on this spectrum:

```
OBJECTIVE ◄──────────────────────────────────────────────► SUBJECTIVE
 USB cable    Vitamins    BBQ Sauce    Skincare    Art Print    Perfume
   │            │            │            │            │           │
   │            │            ▲            │            │           │
   │            │         Maurice's       │            │           │
   │            │                         │            │           │
 "Just works"  "Trust +    "Taste +      "Personal   "Pure       "100%
               evidence"   heritage +     preference  taste"      personal"
                           trust"
```

**Why this matters:** The further right on the spectrum, the more the listing must:
- Evoke sensory experiences (images of food, textures, colors)
- Build trust (because the buyer can't verify before buying)
- Tell a story (because emotion drives the decision)
- Show social proof (because "other people like it" reduces risk)
- Educate (because the buyer may not know what they're looking at)

**Maurice's classification: Center-right (subjective)**
- Taste is subjective — "tangy mustard BBQ" is unfamiliar to most
- There's a strong emotional/cultural component (heritage, nostalgia, discovery)
- Trust matters — they can't taste it before buying
- Education is needed — many don't know mustard BBQ exists
- BUT there are objective elements too (ingredients, size, certifications)

**Implication for the listing:** We must lean hard into storytelling, heritage visuals, food photography that evokes taste, social proof from reviews, and education about what mustard BBQ is. We can't just list features and expect conversion.

### Does the Buyer Need Education?

Some products require the buyer to learn something new before they can appreciate the value.

| Question | Maurice's Answer |
|----------|-----------------|
| Does the buyer need to learn something to appreciate this? | Yes — many don't know mustard BBQ exists as a style |
| What must they learn? | "This is not a regular BBQ sauce. It's a completely different regional tradition." |
| Where do we teach them? | Title (mention "mustard" and "Carolina"), Bullet 1, Image 4 (infographic), A+ Module 6 (FAQ) |
| What happens if we don't educate? | Confused buyers bounce — "I wanted BBQ sauce, this is yellow?" |

**AM Checkpoint:** "Is the essence right? Is the subjective/objective classification accurate? Does the client agree with the value prop hierarchy? Is the education framing correct?"

---

## 7. Phase 4: Positioning

### What We're Doing
Defining **how this product should be positioned** in the market. Positioning isn't just about the product — it's about which competitive frame you put yourself in. The same product can be positioned multiple ways, and the choice affects everything downstream: which competitors matter, which keywords to target, which buyer personas to prioritize, and how to talk about the product.

### What Is Positioning?

Positioning is the answer to: **"When the buyer sees this product, what category do they put it in, and who are they comparing it to?"**

The same liquid blush could be positioned as:
- Competing with other **liquid blushes** (narrow — small competitive set)
- Competing with all **blushes** (broad — cream, powder, liquid, stick)
- As a **"watercolor blush"** (niche — creating a new micro-category)

Each positioning changes the competitive set, the price expectations, the keywords, and the messaging.

### Positioning Dimensions

**1. Competitive Frame — Who are you competing with?**

| Frame | Definition | Example |
|-------|-----------|---------|
| **Narrow** | Direct substitutes in the same sub-category | Other mustard BBQ sauces |
| **Broad** | All products in the parent category | All BBQ sauces |
| **Adjacent** | Products in neighboring categories that serve the same need | Marinades, hot sauces, cooking sauces |
| **Unique** | No direct comparison — you're creating a micro-category | "Southern Gold" as its own thing |

**2. Market Tier — Where do you sit on the price/quality spectrum?**

| Tier | Definition | Example |
|------|-----------|---------|
| **Mass market** | Widely available, lowest price, brand matters less | Cattlemen's ($0.19/oz) |
| **Premium** | Higher price, justified by quality or brand | Lillie's Q ($0.45/oz) |
| **Artisan/Specialty** | Highest price, justified by origin, craft, or exclusivity | Maurice's ($0.61/oz) |
| **Luxury** | Price is a feature, not a barrier | N/A for BBQ sauce |

**3. Positioning Type — What's the core claim?**

| Type | Core Claim | Works When... |
|------|-----------|--------------|
| **Heritage/Origin** | "We're the original / the authentic one" | Brand has real history, provenance matters |
| **Quality/Craft** | "We're made better than the rest" | Ingredients, process, or outcome is demonstrably superior |
| **Innovation** | "We've reinvented the category" | Product is genuinely new or different |
| **Value** | "Same quality, better price" | Competing against overpriced incumbents |
| **Mission** | "We stand for something bigger" | Social cause resonates with target buyer |
| **Experience** | "The way it makes you feel" | Product is subjective, emotional connection matters |

### Positioning Statements

We generate 3-4 positioning statements. One is selected as P0 (primary), others can be mixed in.

### Maurice's Example: Positioning

**Statement A — Heritage Positioning (P0 — Primary):**
> "Maurice's is the authentic South Carolina mustard BBQ sauce from a legendary restaurant chain. For buyers who want the real thing — not a mass-market imitation — this is the original, perfected over 70 years."

**Statement B — Category Education:**
> "Maurice's introduces buyers to an entirely different BBQ tradition. For food explorers tired of the same tomato-based sauces, this is their gateway into South Carolina's best-kept secret."

**Statement C — Quality/Craft:**
> "Maurice's is restaurant-grade BBQ sauce made with all-natural ingredients. For health-conscious cooks who won't compromise on flavor, this is premium BBQ without the junk."

**Statement D — Regional Pride:**
> "Maurice's is the taste of South Carolina, shipped nationwide. For Southern expats and BBQ pilgrims, this is nostalgia in a bottle."

**Selected P0:** Statement A (Heritage Positioning)
**Mixed in:** Elements of B (education) and D (nostalgia) for specific personas.

### Competitor Map Based on Positioning

Positioning clarifies who we're really competing with:

```
                    MASS MARKET ◄─────────────────────► ARTISAN/SPECIALTY
                         │                                      │
     BROAD              Cattlemen's                            │
     (all BBQ           Sweet Baby Ray's                       │
      sauces)           Stubb's                                │
                         │                                      │
                         │                                      │
     NARROW             │                         Lillie's Q    │
     (mustard           │                         Bessinger's   │
      BBQ only)         │                         ▲ MAURICE'S   │
                         │                                      │
                         │                                      │
     ADJACENT           Rufus Teague (marinades)               │
     (cooking           Frank's RedHot (hot sauce)             │
      sauces)           │                                      │
```

**Direct competitors** (same positioning frame): Lillie's Q, Bessinger's
**Indirect competitors** (different frame, same buyer): Cattlemen's, Sweet Baby Ray's
**Adjacent competitors** (different category, same use case): premium marinades, hot sauces

**Mass vs. Niche classification:**
Maurice's is a **niche product with mainstream accessibility.** The core buyer is specific (BBQ enthusiasts, SC nostalgics), but the product is approachable enough for curious foodies. This means: speak to the niche first, but make the listing welcoming to the mainstream.

**AM Checkpoint:** "Is the P0 positioning right? Does the competitive map reflect reality? Are we comfortable being 'artisan/specialty' or does the client want to move toward 'premium mass'?"

---

## 8. Phase 5: Buyer Personas

### What We're Doing
Building evidence-based profiles of who actually buys this product — not demographics, but what drives their decision, what stops them, what they search for, and what they need to see (but didn't ask for) to convert.

### Where Personas Come From

| Source | What It Reveals |
|--------|----------------|
| Our reviews | Who buys, what they value, how they describe the product |
| Competitor reviews | Who buys alternatives, what they wish was different |
| Search terms | How different segments think about the category |
| Reddit/forums | Unfiltered discussions about purchase decisions |
| Q&A section | Specific pre-purchase concerns |
| Positioning (Phase 4) | Which buyer segments our positioning speaks to |

### Persona Structure

Each persona has:
- **Who they are** — brief demographic + lifestyle sketch
- **How they search** — what they type, what intent they have
- **What drives the purchase** — the things that make them click "Add to Cart"
- **What stops them** — objections and concerns
- **What they need to see but didn't search for** — unstated needs that drive conversion
- **Where they focus on the listing** — which listing elements matter most to them

### Maurice's Buyer Personas

#### Persona 1: "The BBQ Purist" — ~35% of traffic
**Who:** Knows what mustard BBQ is. Deep researcher — reads reviews, checks ingredients, compares options.
**Searches for:** "mustard bbq sauce", "carolina bbq sauce", "best bbq sauce for pulled pork"

| Decision Drivers | Priority | Where to Address |
|-----------------|----------|-----------------|
| Flavor authenticity | P0 | Bullet 1, Image 3, A+ |
| Ingredient quality | P1 | Bullet 3, Image 5 |
| Reviews from fellow BBQ people | P1 | Bullet 5, A+ |

**Objection:** "Is this actually authentic or just marketing?"
**Counter:** Restaurant connection, heritage visuals, real food photography.
**Unstated need:** Wants to see real food photography and BBQ usage — not stock photos.

---

#### Persona 2: "The SC Nostalgic" — ~25% of traffic
**Who:** Has eaten at the restaurant or knows the brand from living in/visiting SC. High intent.
**Searches for:** "maurice's bbq sauce", "piggie park sauce", "south carolina bbq"

| Decision Drivers | Priority | Where to Address |
|-----------------|----------|-----------------|
| "Is this the real thing?" | P0 | Bullet 2, Image 6, A+ Module 1 |
| Heritage/story connection | P0 | A+ hero, Image 6 |
| Taste matches the restaurant | P1 | Bullet 1, reviews |

**Objection:** Price on repeat orders.
**Counter:** 2-pack value, "stock up" messaging.
**Unstated need:** Wants to SEE the restaurant — photos, heritage, "Since 1953."

---

#### Persona 3: "The Curious Foodie" — ~20% of traffic
**Who:** Browsing the category. Doesn't know mustard BBQ exists. Quick decider if hooked.
**Searches for:** "bbq sauce", "unique bbq sauce", "gourmet bbq sauce"

| Decision Drivers | Priority | Where to Address |
|-----------------|----------|-----------------|
| Uniqueness — "this is different" | P0 | Main image (gold stands out), Title, Bullet 1 |
| Social proof — "others love it" | P0 | Bullet 5, A+ |
| Taste description that sounds good | P1 | Bullet 1, Image 3, A+ |

**Objection:** "I've never tried mustard BBQ — will I like it?" + "Why $22?"
**Counter:** Education + "nearly 1,000 five-star reviews."
**Unstated need:** Needs education on what mustard BBQ IS. Needs flavor reassurance.

---

#### Persona 4: "The Gift Buyer" — ~10% of traffic
**Who:** Buying for someone else. Needs something that looks special and has a story.
**Searches for:** "bbq gift", "bbq sauce gift set", "southern food gift"

| Decision Drivers | Priority | Where to Address |
|-----------------|----------|-----------------|
| Presentation/giftability | P0 | Images, A+ |
| Uniqueness (interesting gift) | P0 | Title, Bullet 1, A+ |
| Brand story to share | P1 | A+ Module 1 |

**Objection:** "Will the recipient like it?" → **Counter:** 4.7★, "nearly 1,000 love it."

---

#### Persona 5: "The Health-Conscious Cook" — ~10% of traffic
**Who:** Wants BBQ sauce meeting dietary criteria. Reads ingredient lists first.
**Searches for:** "natural bbq sauce", "bbq sauce no hfcs", "healthy bbq sauce"

| Decision Drivers | Priority | Where to Address |
|-----------------|----------|-----------------|
| Ingredients — what's in it | P0 | Bullet 3, Image 5, A+ |
| "No [X]" claims | P0 | Title, Bullet 3 |
| Taste despite being "natural" | P1 | Bullet 1, reviews |

**Objection:** "Natural = bad taste." → **Counter:** "70 years of restaurant customers disagree."

---

### Where Each Persona Focuses

```
                        Purist    Nostalgic   Foodie    Gift    Health
Title                   ████░      ███░░       ██░░░     ██░░░   ███░░
Main Image              ████░      █████       █████     █████   ██░░░
Bullets 1-2             █████      ███░░       ███░░     ███░░   █████
Bullets 3-5             ███░░      ██░░░       ██░░░     ██░░░   █████
Secondary Images        ████░      █████       █████     █████   ███░░
A+ Content              ███░░      █████       ██░░░     ██░░░   ████░
Reviews                 █████      ███░░       ████░     ████░   █████
Price Sensitivity       ██░░░      ██░░░       █████     █████   ███░░
```

**AM Checkpoint:** "Do you recognize these buyer types? Are the traffic shares reasonable? Anything missing?"

---

## 9. Phase 6: Product Attributes

### What We're Doing
Creating a **master list** of every product attribute that could matter to any buyer — tagged by type, priority, persona relevance, and a crucial new dimension: **whether the buyer is aware they want it.**

### Attribute Types

| Type | What it means | Example |
|------|--------------|---------|
| **Feature** | A factual characteristic | "Mustard-based recipe" |
| **Uniqueness** | Differentiates from competitors | "70+ year restaurant heritage" |
| **Problem Solver** | Addresses a customer pain | "No HFCS — clean label" |
| **Trust Signal** | Builds credibility | "989 reviews at 4.7★" |
| **Experience** | Describes the outcome of using it | "Bold, tangy flavor" |
| **Social Proof** | External validation | "Amazon's Choice badge" |
| **Convenience** | Makes buying/using easier | "2-pack, won't run out" |
| **Value** | Addresses price-to-worth | "Restaurant-quality at grocery price" |

### Priority Levels

| Priority | Meaning | Listing Placement |
|----------|---------|-------------------|
| **P0** | If the buyer doesn't see this, they won't buy | Title + first 2 bullets + images |
| **P1** | Strengthens the decision | Bullets + images or A+ |
| **P2** | Adds value for specific segments | A+ or later bullets |
| **P3** | Supporting facts, indexing help | Backend keywords only |

### The Aware / Unaware Framework

This is the distinction between what buyers **know they want** and what they **need to see to convert but didn't know to ask for.**

**Aware attributes** — The buyer actively searches for these or consciously evaluates them. They know they want this before they land on the listing.

*Function:* These **capture** the buyer. They drive clicks. They should appear in the title, main image, and first bullets — anywhere the buyer makes the "click or skip" decision.

*Examples for Maurice's:*
- "Mustard BBQ sauce" — they searched for this
- "Carolina BBQ" — they're looking for this region
- "All-natural / No HFCS" — they filtered for this

**Unaware attributes** — The buyer doesn't search for these, but seeing them tips the decision from "interested" to "add to cart." They're hidden conversion drivers.

*Function:* These **convert** the buyer. They work below the fold, in secondary images, in A+ content, and in later bullets — anywhere the buyer is evaluating whether to actually buy.

*Examples for Maurice's:*
- "70+ year restaurant heritage" — nobody searches for this, but it builds massive trust
- "Same recipe as the restaurants" — removes doubt about authenticity
- "Versatile — works on 6+ different foods" — expands perceived value
- "Nearly 1,000 five-star reviews" — they see the stars but the explicit callout reinforces

**The interplay:**
```
AWARE attributes → GET the buyer to the listing (CTR) and anchor their attention
UNAWARE attributes → KEEP the buyer on the listing and push toward purchase (CVR)

A listing that only shows AWARE attributes: High CTR, low CVR (attracts but doesn't convert)
A listing that only shows UNAWARE attributes: Low CTR, high CVR (converts but nobody finds it)
The goal: AWARE attributes in high-visibility spots + UNAWARE attributes woven throughout
```

### Maurice's Attribute Master List

| # | Attribute | Type | Priority | Aware? | Key Personas | Placement |
|---|-----------|------|----------|--------|-------------|-----------|
| 1 | Mustard-based recipe | Feature + Uniqueness | **P0** | **Aware** | Purist, Foodie | Title, Bullet 1, Image 4 |
| 2 | Carolina BBQ style | Feature | **P0** | **Aware** | Purist, Nostalgic | Title, Bullet 1 |
| 3 | All-natural, no HFCS | Problem Solver | **P0** | **Aware** | Health-Conscious | Title, Bullet 3, Image 5 |
| 4 | 70+ year restaurant heritage | Uniqueness + Trust | **P0** | **Unaware** | Nostalgic, Purist | Bullet 2, Image 6, A+ Module 1 |
| 5 | Same recipe as the restaurants | Trust Signal | **P1** | **Unaware** | Purist, Nostalgic | Bullet 2, Image 6, A+ Module 1 |
| 6 | Bold, tangy, savory flavor | Experience | **P1** | **Mixed** | Purist, Nostalgic | Bullet 1, Image 3, A+ Module 2 |
| 7 | 2-pack format (36oz) | Convenience | **P1** | **Aware** | Nostalgic, Gift | Title, Bullet 5 |
| 8 | 4.7★ with 989 reviews | Social Proof | **P1** | **Unaware** | Foodie, Gift, Health | Bullet 5, A+ |
| 9 | Versatile uses | Feature | **P2** | **Unaware** | Purist, Foodie | Bullet 4, Image 5, A+ |
| 10 | South Carolina BBQ tradition | Uniqueness | **P2** | **Unaware** | Nostalgic, Foodie | A+ Module 1, A+ Module 5 |
| 11 | Makes a great gift | Value | **P2** | **Aware** (for gift buyers) | Gift Buyer | Image 7, A+ |
| 12 | Amazon's Choice badge | Social Proof | **P2** | **Unaware** | Foodie, Gift | (organic) |

Notice: Attributes #1, 2, 3 are **Aware** — people search for these. They belong in high-visibility positions (title, first bullets, main image context).

Attributes #4, 5, 8, 9, 10 are **Unaware** — nobody searches for "70-year heritage" or "versatile BBQ sauce," but these are what tip the buyer from "looks interesting" to "add to cart." They belong in the middle and lower listing sections where buyers are evaluating.

**AM Checkpoint:** "Is the priority ranking right? Is the aware/unaware classification accurate? Anything missing?"

---

## 10. Phase 7: Keyword Strategy

### What We're Doing
Building a keyword strategy that draws from **everything upstream** — not just a keyword tool, but the buyer personas, the attributes, the positioning, the cultural language, and the brand voice. Keywords aren't researched in isolation; they emerge from deep product understanding.

### Where Keywords Come From

| Source | What It Contributes |
|--------|-------------------|
| **Phase 1 Research** | Search behavior data, auto-suggest terms, competitor keyword reverse-engineering |
| **Phase 2 Brand** | Branded terms, brand voice language, cultural terms |
| **Phase 3 Product Knowledge** | Category terms, ingredient terms, process terms |
| **Phase 4 Positioning** | Positioning-specific terms (e.g., "artisan" vs "value"), competitive frame terms |
| **Phase 5 Personas** | How each persona searches, intent-specific terms |
| **Phase 6 Attributes** | Feature terms, benefit terms, problem terms |
| **SQP Data** (if available) | Keywords we already rank for, click share gaps, conversion share gaps |
| **Data Dive / Market Tools** (if available) | Category search volumes, keyword opportunity scores, competitor rankings |

### Keyword Types

| Type | What it is | Example | Volume |
|------|-----------|---------|--------|
| **Master** | Core category term | "bbq sauce" | Highest |
| **Sub-Category** | More specific category | "mustard bbq sauce" | Medium |
| **Long-Tail** | Multi-word specific phrase | "south carolina mustard bbq sauce for pulled pork" | Lower, higher intent |
| **Branded** | Includes our brand | "maurice's bbq sauce" | Low, highest intent |
| **Competitor Branded** | Includes competitor brand | "sweet baby ray's alternative" | Low |
| **Problem / Use-Case** | Describes a need | "sauce for pulled pork" | Medium |
| **Question** | Framed as a question | "what is mustard bbq sauce" | Low |
| **Seasonal** | Time-bound relevance | "bbq sauce for memorial day" | Spiky |
| **Cross-Sell** | Adjacent product searches | "bbq gift basket" | Low |
| **Cultural / Voice** | Terms from brand voice and local culture | "Southern Gold", "Carolina Gold" | Low |

### Keyword-to-Phase Traceability

Every keyword should trace back to why it matters:

| Keyword | Type | Came From | Why It Matters |
|---------|------|-----------|---------------|
| "bbq sauce" | Master | Phase 1 (search data) | Highest volume, must index |
| "mustard bbq sauce" | Sub-Category | Phase 3 (product knowledge) + Phase 1 | Core product definition |
| "maurice's bbq sauce" | Branded | Phase 2 (brand understanding) | Brand defense |
| "sauce for pulled pork" | Problem | Phase 5 (Purist persona) + Phase 6 (use-case attribute) | Persona 1's actual search |
| "natural bbq sauce no hfcs" | Problem | Phase 5 (Health persona) + Phase 6 (Aware P0 attribute) | Persona 5's filter |
| "Southern Gold" | Cultural | Phase 2 (brand voice) | Cultural term insiders use |
| "bbq gift basket" | Cross-Sell | Phase 5 (Gift persona) | Capture gift traffic |

### How SQP Data Strengthens Keywords

When SQP data is available, it shows us:
- **Keywords we rank for but don't convert on** → listing may not address that keyword's intent
- **Keywords we convert on but have low click share** → need more ad spend or better title presence
- **Keywords competitors dominate** → opportunity to target or gap to accept
- **High-impression, low-click keywords** → our main image or title may not match the search intent

This data directly informs keyword priority and listing placement decisions.

### How Data Dive / Market Tools Strengthen Keywords

Market research tools provide:
- **Absolute search volume** → validates our relative volume estimates
- **Keyword opportunity scores** → high volume + low competition = go after it
- **Trending keywords** → emerging terms to get ahead of
- **Competitor keyword rankings** → what they rank for that we don't

### How Amazon Ads Data Strengthens Keywords

If the product is already running Amazon ads, the Search Term Report and campaign data provide:
- **Actual converting search terms** → these are proven buyers, not just searchers
- **High-spend, low-conversion terms** → keywords where our listing fails to convert (listing issue, not keyword issue)
- **Auto campaign discoveries** → terms Amazon's algorithm matched us to
- **ACOS by keyword** → which keywords are profitable, which aren't

This is some of the highest-confidence keyword data because it's based on actual purchases, not estimates.

### AM Keyword Approval

Before keywords flow into copy (Phase 9), the AM reviews and approves the priority assignments:
- Confirms P0 keywords (these MUST appear in title + bullets)
- Confirms P1 keywords (these SHOULD appear in bullets or title)
- May promote or demote keywords based on client/brand preferences
- May add keywords from their own knowledge that the system missed

**The AM-approved P0 and P1 keyword list becomes a hard constraint for Phase 9** — every title and bullet option must show which of these keywords it captures, and the selected combination must cover all P0 keywords.

### Keyword Priority & Placement

**P0 — Must rank:**
Title positions 1-3. Exact match campaigns. These drive the majority of traffic.
- "bbq sauce", "mustard bbq sauce", "carolina bbq sauce", "maurice's bbq sauce"

**P1 — Should rank:**
Title positions 4-5, bullets, broad/phrase campaigns.
- "south carolina bbq sauce", "yellow bbq sauce", "bbq sauce for pulled pork", "natural bbq sauce"

**P2 — Should index:**
Backend keywords, A+ alt text. Long-tail campaigns.
- "southern cooking sauce", "bbq gift", "meat marinade"

**P3 — Backend only:**
Misspellings, synonyms, regional terms.
- "barbeque", "bar-b-que", "condiment", "table sauce", "SC BBQ"

**AM Checkpoint:** "Are the P0/P1 keyword assignments correct? Any keywords to add, promote, or remove? Are there keywords from your ads data or SQP that should be included?" The AM-approved keyword list becomes a hard requirement for copy generation in Phase 9.

---

## 11. Phase 8: Listing Real Estate Allocation

### What We're Doing
The Amazon product page has limited real estate. Every element — title, main image, up to 8 secondary images/videos (7 visible in gallery), 5 bullets, A+ content modules — must earn its place. This phase creates a **framework** for how to allocate that real estate, then produces a **product-specific allocation map** for this listing.

### The Framework: Principles of Allocation

**Principle 1: Aware attributes go high, Unaware attributes go deep.**

The top of the listing (title, main image, bullets 1-2) is where buyers decide to keep reading or bounce. This space must contain **Aware** attributes — the things they actively searched for. If they searched "mustard bbq sauce" and don't see "mustard" in the title and "mustard" represented in the first two bullets, they leave.

The deeper parts of the listing (bullets 3-5, secondary images, A+ content) are where buyers who are already interested evaluate whether to buy. This is where **Unaware** attributes do their work — heritage, versatility, social proof, ingredient transparency.

```
LISTING POSITION        AWARENESS LEVEL           FUNCTION
───────────────────     ─────────────────────     ────────────────
Title                   AWARE attributes only      Capture + Index
Main Image              AWARE + key differentiator Capture
Bullets 1-2             AWARE + primary benefit    Hook
Bullets 3-5             Mix of AWARE + UNAWARE     Convince
Secondary Images 2-4    Mix                        Show + Prove
Secondary Images 5-7    UNAWARE + brand story      Reassure + Close
Video (slot 8 or 9)     UNAWARE + experience       Engage + Convert
A+ Modules 1-3          UNAWARE + emotional        Deepen
A+ Modules 4-5          UNAWARE + comparison/FAQ   Convert + Cross-sell
Backend Keywords        Anything not already used  Index
Backend Fields          Subject, Intended Use, etc Extra Index
```

**Note on image slots:** Amazon allows up to 9 total media slots (main image + 8 additional). 7 are visible in the gallery; slots 8-9 may only show when scrolling. Video counts as one slot. Most categories support at least 1 video slot.

**Principle 2: P0 attributes get multiple touchpoints. P1 attributes get one strong touchpoint.**

A P0 attribute isn't mentioned once — it's **threaded** across the listing. The buyer encounters it in the title, again in a bullet, again in an image, and again in A+. Each touchpoint reinforces it. By the time they reach "Add to Cart," the P0 attributes are deeply embedded.

A P1 attribute appears once or twice, strongly. A P2 attribute gets one mention. A P3 attribute is backend only.

| Priority | # of Touchpoints | Where |
|----------|-----------------|-------|
| P0 | 3-5 touchpoints | Title + bullet + image + A+ (threaded) |
| P1 | 1-2 touchpoints | Bullet OR image + A+ |
| P2 | 1 touchpoint | A+ module or later bullet |
| P3 | 0 visible | Backend keywords only |

**Principle 3: Each listing element has a job. Don't duplicate jobs.**

| Element | Primary Job | Secondary Job |
|---------|------------|---------------|
| Title | Index for search + capture on SERP | Communicate category + brand |
| Main Image | Stop the scroll | Differentiate from competitors |
| Image 2-3 | Show the product experience | Build desire |
| Image 4-5 | Prove claims (infographics) | Educate + differentiate |
| Image 6-7 | Build trust + close | Brand story + value |
| Video | Show product in use, build desire | Increase time-on-page (ranking signal) |
| Bullet 1 | Core value prop + primary keyword | Hook the reader |
| Bullet 2 | Key differentiator | Win the comparison |
| Bullet 3 | Address a specific concern | Expand to new persona |
| Bullet 4 | Show versatility/use cases | Broaden appeal |
| Bullet 5 | Value/format + social proof | Close with confidence |
| A+ Module 1-2 | Emotional story + experience | Brand building |
| A+ Module 3-4 | Proof + comparison | Conversion |
| A+ Module 5 | FAQ + cross-sell | Deepen + expand |

**Principle 4: Subjective products need more "show." Objective products need more "tell."**

From Phase 3, we know where the product sits on the subjective–objective spectrum. This affects allocation:

| | Objective Product | Subjective Product |
|--|------------------|-------------------|
| Images | Focus on specs, dimensions, technical detail | Focus on lifestyle, experience, emotion |
| Bullets | Feature-fact-benefit structure | Story-emotion-proof structure |
| A+ | Comparison charts, tech specs | Hero imagery, brand story, sensory language |
| Trust signals | Certifications, test results | Heritage, reviews, social proof |

**Principle 5: The mobile experience is different from desktop.**

On mobile (60%+ of Amazon traffic):
- Only the first **70-80 characters** of the title show (70 is the safe cutoff)
- Only bullets 1-2 show above the fold
- Images are swiped, not browsed — image 2 gets 80% of views, image 7 gets 20%
- A+ content is scrolled quickly — hero banners catch eyes, dense text is skipped

Implication: **Front-load everything.** The most important message must be in the first 70 title characters, bullets 1-2, and images 1-3.

### Product-Specific Allocation Map: Maurice's

Based on all the principles above, here's the specific allocation for Maurice's BBQ Sauce:

```
TITLE (148 chars):
├── Pos 1: Brand → "Maurice's Piggie Park Southern Gold"     [Brand, Aware]
├── Pos 2: Category → "Authentic SC Mustard BBQ Sauce"        [Aware: mustard, BBQ, carolina]
├── Pos 3: Trust/Quality → "All Natural, No HFCS"             [Aware: health]
├── Pos 4: Heritage → "Original Restaurant Recipe Since 1953"  [Unaware: heritage]
└── Pos 5: Format → "18oz (Pack of 2)"                        [Aware: size]

MAIN IMAGE:
└── Two bottles, gold sauce prominent, premium studio shot     [Aware: BBQ sauce]
    (Gold color = natural differentiation vs red competitors)   [Differentiation]

IMAGE 2: Sauce close-up — texture, golden color, appetizing    [Experience]
IMAGE 3: Lifestyle — sauce on pulled pork at backyard BBQ      [Experience, Desire]
IMAGE 4: Infographic — "What Makes Southern Gold Different"    [Differentiation, Education]
IMAGE 5: Use cases grid — 6 foods with sauce                  [Versatility — Unaware]
IMAGE 6: Heritage — restaurant photo + "Since 1953"           [Trust — Unaware]
IMAGE 7: Value — 2-pack callout + review quotes               [Close]

VIDEO (Slot 8): 15-30sec — sauce being brushed on pulled pork  [Experience, Desire]
  Specs: MP4/MOV, 16:9 ratio, 6-45 seconds, ≤500MB

BULLET 1: Core value prop (flavor + heritage)                  [Aware: mustard BBQ + Unaware: heritage]
BULLET 2: Differentiator (real restaurant, not a knockoff)     [Unaware: trust]
BULLET 3: Clean label (all natural, no HFCS)                   [Aware: health]
BULLET 4: Use cases (pulled pork, chicken, ribs, dipping...)   [Unaware: versatility]
BULLET 5: Format + social proof (2-pack, 989 reviews, 4.7★)   [Aware: format + Unaware: proof]

A+ MODULE 1: Hero — "The Sauce That Built a Southern Legend"   [Unaware: brand story]
A+ MODULE 2: 4-image grid — four use cases                    [Unaware: versatility]
A+ MODULE 3: Three pillars — Mustard, Natural, Heritage        [Differentiation summary]
A+ MODULE 4: Comparison chart — OUR variants (Original vs Hot   [Cross-sell + differentiate]
             vs Hickory vs Sweet) — NOT competitors (Amazon rule)
A+ MODULE 5: Lifestyle banner — backyard BBQ scene             [Experience, emotion]
A+ MODULE 6: FAQ — "What is mustard BBQ?" etc.                 [Education, objection handling]
A+ MODULE 7: Cross-sell — other flavors + variety pack         [Expansion]

BACKEND KEYWORDS:
"barbeque bar-b-que dipping pulled pork smoker ribs grilling
 marinade condiment tangy vinegar southern gold SC regional
 specialty gourmet artisan gift cookout tailgate summer picnic
 gluten free"
```

### How the Allocation Serves Each Persona

| Persona | Their "path" through the listing |
|---------|--------------------------------|
| **BBQ Purist** | Title (mustard BBQ) → Bullet 1 (flavor) → Image 3 (food shot) → Bullet 3 (ingredients) → Reviews |
| **SC Nostalgic** | Title (Maurice's Piggie Park) → Image 6 (restaurant) → Bullet 2 (real thing) → A+ Module 1 (heritage) |
| **Curious Foodie** | Main Image (gold stands out) → Bullet 1 (what is this?) → Image 4 (education) → A+ Module 6 (FAQ) → Bullet 5 (social proof) |
| **Gift Buyer** | Main Image (looks premium) → Image 7 (value) → A+ Module 1 (story to tell) → Bullet 5 (others love it) |
| **Health-Conscious** | Title (All Natural, No HFCS) → Bullet 3 (clean label) → Image 5 (ingredients) → Bullet 1 (still tastes great) |

**AM Checkpoint:** "Does this allocation feel right? Are we emphasizing the right things in the right places? Any elements the client would want to change?"

---

## 12. Phase 9: Listing Copy — Title & Bullets

### What We're Doing
Generating **multiple options** for title and bullets. The AM picks the winning combination from the options.

### Amazon Title Rules (Updated Jan 2025)

- **Max 200 characters** (strictly enforced since Jan 2025; 150 recommended)
- Brand name first (exception: in Grocery & Gourmet, brand goes at the **end** unless it has high brand recognition)
- First **70-80 characters** show on mobile (70 is the safe cutoff) — **front-load what matters**
- **No word repetition**: the same word can appear **max 2 times** in a title. Amazon's system auto-flags titles with repetition.
- No ALL CAPS, no promotional language ("best", "sale", "#1"), no special characters (!, $, @)
- Every word is indexed for Amazon search
- Must be human-readable, not a keyword dump
- **14-day grace period**: After Amazon flags a non-compliant title, you have 14 days to fix it. After that, Amazon **auto-overrides** the title with their version (which is usually worse).

### Keyword Threading in Titles

The title must index for all P0 keywords. Each title option shows which keywords it captures:

### Title Options

**Option A — Heritage-Led:**
> Maurice's Piggie Park Southern Gold – Authentic SC Mustard BBQ Sauce, Original Restaurant Recipe Since 1953 – All Natural, Bold Tangy Flavor – 18oz (Pack of 2)

Mobile preview: *"Maurice's Piggie Park Southern Gold – Authentic SC Mustard BBQ Sauce, Origina..."*
Best for: SC Nostalgic, BBQ Purist. Leads with brand heritage.
P0 keywords captured: `bbq sauce` ✓ `mustard` ✓ `carolina` ✗ (has "SC") `maurice's` ✓ `all natural` ✓
P1 keywords captured: `southern gold` ✓ `restaurant` ✓ `tangy` ✓ `18oz` ✓ `pack of 2` ✓

**Option B — Keyword-Optimized:**
> Maurice's Southern Gold BBQ Sauce – Carolina Mustard Barbecue Sauce, All Natural, No HFCS – Authentic SC Restaurant Recipe – 18oz (Pack of 2)

Mobile preview: *"Maurice's Southern Gold BBQ Sauce – Carolina Mustard Barbecue Sauce, All Natu..."*
Best for: Maximum search indexing. Front-loads category keywords.
P0 keywords captured: `bbq sauce` ✓ `mustard` ✓ `carolina` ✓ `barbecue sauce` ✓ `maurice's` ✓ `all natural` ✓ `no hfcs` ✓
P1 keywords captured: `southern gold` ✓ `restaurant` ✓ `18oz` ✓ `pack of 2` ✓

**Option C — Benefit-Led:**
> Maurice's Southern Gold Mustard BBQ Sauce – Bold Tangy Flavor, All Natural, No Preservatives – Authentic Carolina Barbecue from a Legendary SC Restaurant – 2 Pack (18oz Each)

Mobile preview: *"Maurice's Southern Gold Mustard BBQ Sauce – Bold Tangy Flavor, All Natural, N..."*
Best for: Curious Foodie, Health-Conscious. Leads with taste + clean label.
P0 keywords captured: `bbq sauce` ✓ `mustard` ✓ `carolina` ✓ `barbecue` ✓ `maurice's` ✓ `all natural` ✓
P1 keywords captured: `southern gold` ✓ `tangy` ✓ `restaurant` ✓ `18oz` ✓ `2 pack` ✓

### Amazon Bullet Point Rules

- **Max 5 bullet points** for most categories
- **500 characters per bullet** for sellers (250 for vendors) — but keep to ~200 for readability
- **Only the first 1,000 bytes total** across all 5 bullets are indexed for Amazon search. Anything beyond 1,000 bytes is still visible to the buyer but **invisible to Amazon's search algorithm**. This means: **front-load your most important keywords into bullets 1-3.**
- **No full ALL CAPS bullets** — Amazon prohibits entire bullets in all-caps (can trigger listing suppression). However, ALL CAPS in the **lead-in header** (first few words before the dash) is a common accepted practice and helps scannability.
- No promotional claims ("best seller", "limited time", "#1")
- No pricing or shipping information
- No HTML tags

### How Bullets Work: Frames, Keywords, and Selection

Bullet points must accomplish three things simultaneously: **convert** the reader, **index** for search (keywords), and **differentiate** from competitors. To do this well, we don't just write 2 options per slot — we write from **10 different frames** (angles of attack), each producing one or more bullet options. Then we select the best 5 from the full set and assign them to slots.

**Why frames matter:** Each frame approaches the product from a different angle. A "Heritage" frame tells the origin story. A "Comparison" frame positions against competitors. A "Use-Case" frame shows versatility. Different frames resonate with different personas and naturally incorporate different keywords. By generating from 10 frames, we ensure we're not stuck in one voice.

### Keyword Threading in Bullets

Every bullet must weave in **AM-approved P0 and P1 keywords** from Phase 7. The system tracks which keywords each bullet captures:

```
Bullet text: "AUTHENTIC SOUTH CAROLINA MUSTARD BBQ SAUCE – Maurice's
Southern Gold is the signature sauce from Piggie Park..."

Keywords captured: [mustard bbq sauce ✓] [carolina ✓] [bbq sauce ✓]
                   [southern gold ✓] [authentic ✓]
Missing P0 keywords: [barbecue sauce] [all natural]
→ These must be captured in other bullets or title
```

After generating all bullets, we produce a **keyword coverage matrix** showing which P0/P1 keywords are covered across the selected 5 bullets + title, and flag any gaps that need backend keywords.

### The 10 Bullet Frames

| Frame | Angle | Best For Persona | Natural Keywords |
|-------|-------|-----------------|-----------------|
| **1. Heritage** | Brand story, origin, founder, tradition | Nostalgic, Purist | Brand terms, origin terms |
| **2. Flavor/Experience** | Taste, sensory description, what it's like | Purist, Foodie | Flavor terms, category terms |
| **3. Differentiation** | Why we're different from competitors | Purist, Foodie | Category terms, comparison terms |
| **4. Clean Label / Health** | Ingredients, what's NOT in it, certifications | Health-Conscious | Ingredient terms, "no [X]" terms |
| **5. Use Cases** | How and when to use the product | Purist, Foodie | Use-case keywords, recipe terms |
| **6. Social Proof** | Reviews, ratings, awards, popularity | Foodie, Gift | (No specific keywords — conversion driver) |
| **7. Value / Format** | Size, quantity, price-per-unit, pack value | Gift, repeat buyers | Format terms, size terms |
| **8. Education** | Teach the buyer something they don't know | Foodie (new to category) | Category education terms |
| **9. Occasion / Gift** | When to buy, who it's for, seasonal | Gift Buyer | Occasion keywords, gift terms |
| **10. Comparison** | Direct or implied comparison to alternatives | Purist, Foodie, Health | Competitor category terms |

### Maurice's Bullet Options by Frame

**Frame 1: Heritage** (2 options)

> **1a:** AUTHENTIC SOUTH CAROLINA MUSTARD BBQ SAUCE – Maurice's Southern Gold is the signature sauce from Piggie Park, one of South Carolina's most legendary BBQ restaurants. This isn't imitation mustard BBQ — it's the original recipe served in our restaurants for over 70 years. Bold, tangy, and utterly unique.
> *Keywords: mustard bbq sauce, carolina, bbq, restaurant, southern gold*

> **1b:** FROM A LEGENDARY SC RESTAURANT, SINCE 1953 – Maurice's Piggie Park has been serving South Carolina's most famous mustard barbecue sauce for over 70 years. Now, the same recipe that made our restaurants an institution is in your hands. This is the real deal.
> *Keywords: mustard barbecue sauce, south carolina, restaurant*

---

**Frame 2: Flavor / Experience** (2 options)

> **2a:** BOLD TANGY MUSTARD BBQ UNLIKE ANYTHING IN YOUR PANTRY – Forget tomato-based sauces. Maurice's Southern Gold is a true Carolina mustard BBQ sauce — tangy, savory, and packed with flavor that's been perfected over 70+ years. One taste and you'll understand why people drive hours for this sauce.
> *Keywords: mustard bbq, carolina, tangy, bbq sauce*

> **2b:** THE TASTE OF SOUTHERN GOLD – Rich mustard tang balanced with savory spice and just a touch of vinegar bite. This isn't sweet, it isn't smoky — it's an entirely different BBQ experience. The kind of flavor that turns first-timers into lifelong fans.
> *Keywords: mustard, bbq, southern gold, vinegar*

---

**Frame 3: Differentiation** (2 options)

> **3a:** THE REAL THING, NOT A KNOCKOFF – Unlike mass-market "carolina style" sauces, Maurice's comes from an actual restaurant chain that's been serving mustard BBQ since 1953. Same recipe, same facility, same taste that made Piggie Park a South Carolina institution.
> *Keywords: carolina, mustard bbq, restaurant*

> **3b:** RESTAURANT-QUALITY SAUCE DELIVERED TO YOUR DOOR – This is the exact same sauce served at Maurice's Piggie Park restaurants — not a watered-down retail version. Made in our own facility using our original recipe, every bottle is restaurant-grade barbecue sauce.
> *Keywords: restaurant, barbecue sauce*

---

**Frame 4: Clean Label / Health** (2 options)

> **4a:** ALL NATURAL, NO JUNK – Made with real mustard, vinegar, and spices. No high fructose corn syrup, no artificial preservatives, no artificial colors. Just honest ingredients that let the flavor shine. Check the label — you'll recognize every ingredient.
> *Keywords: all natural, no hfcs, mustard, vinegar*

> **4b:** CLEANER THAN THE BIG BRANDS – While most BBQ sauces load up on HFCS and artificial additives, Maurice's Southern Gold keeps it simple: mustard, vinegar, spices. All natural ingredients you can pronounce. Better for you, better tasting.
> *Keywords: bbq sauce, all natural, southern gold*

---

**Frame 5: Use Cases** (2 options)

> **5a:** ENDLESS WAYS TO USE IT – Perfect on pulled pork (the classic), but that's just the start. Brush it on grilled chicken, drizzle over smoked ribs, use as a dipping sauce for fries, mix into coleslaw, or marinate shrimp. One bottle replaces three sauces.
> *Keywords: pulled pork, grilled chicken, ribs, dipping sauce, marinade*

> **5b:** THE ULTIMATE GRILLING COMPANION – Whether you're low-and-slow smoking a pork shoulder, grilling wings for game day, or need a tangy dipping sauce for a summer cookout, Southern Gold brings the flavor. Try it anywhere you'd use ketchup — you won't go back.
> *Keywords: grilling, smoking, dipping sauce, cookout, southern gold*

---

**Frame 6: Social Proof** (2 options)

> **6a:** NEARLY 1,000 FIVE-STAR REVIEWS – There's a reason Maurice's Southern Gold has a 4.7-star rating and the Amazon's Choice badge. Once people try authentic mustard BBQ, they don't go back to the red stuff. Join the thousands who've made the switch.
> *Keywords: mustard bbq, bbq*

> **6b:** "BEST MUSTARD BBQ SAUCE, PERIOD" – That's what our customers keep saying. With nearly 1,000 reviews and a 4.7-star rating, Maurice's Southern Gold isn't just popular — it's the standard for Carolina mustard barbecue sauce on Amazon.
> *Keywords: mustard bbq sauce, carolina, barbecue sauce*

---

**Frame 7: Value / Format** (2 options)

> **7a:** STOCK UP WITH THE 2-PACK (36oz TOTAL) – Two 18oz bottles so you're set for grilling season. At restaurant quality, this sauce goes fast — the 2-pack means you won't run out mid-cookout. Better value than buying single bottles.
> *Keywords: 2 pack, 18oz, grilling*

> **7b:** 2-PACK OF 18oz BOTTLES (36oz TOTAL) – Generously sized so you can sauce liberally without rationing. At $0.61/oz for restaurant-grade mustard BBQ sauce, this is a fraction of what you'd pay dining out. Stock up and have it on hand all season.
> *Keywords: 2 pack, 18oz, mustard bbq sauce*

---

**Frame 8: Education** (2 options)

> **8a:** WHAT IS MUSTARD BBQ? – If you've only had tomato-based BBQ sauce, you're missing out on an entire American tradition. South Carolina's signature style uses mustard as the base instead of tomato — creating a tangy, savory flavor profile that pairs perfectly with pork, chicken, and more. This is your introduction.
> *Keywords: mustard bbq, bbq sauce, south carolina, pork, chicken*

> **8b:** DISCOVER SOUTH CAROLINA'S BEST-KEPT SECRET – Most BBQ sauces are red. This one is gold — and for good reason. SC mustard BBQ is one of America's four great barbecue traditions, and Maurice's Southern Gold is its most famous ambassador. If you're curious about what real Carolina BBQ tastes like, start here.
> *Keywords: bbq sauce, mustard bbq, carolina bbq, barbecue, southern gold*

---

**Frame 9: Occasion / Gift** (1 option)

> **9a:** THE PERFECT GIFT FOR BBQ LOVERS – Searching for a unique food gift? Maurice's Southern Gold is a conversation starter — a legendary sauce from a 70-year-old SC restaurant that most people have never tried. Beautiful 2-pack presentation, a great story to tell, and a 4.7-star rating to back it up.
> *Keywords: gift, bbq, 2 pack*

---

**Frame 10: Comparison** (1 option)

> **10a:** NOT YOUR GROCERY STORE BBQ SAUCE – There's mass-market barbecue sauce, and then there's Maurice's. No generic flavoring, no high fructose corn syrup, no factory shortcuts. Just an authentic SC restaurant recipe made with all natural ingredients — the kind of sauce that has people asking "where did you get this?"
> *Keywords: bbq sauce, barbecue sauce, all natural, restaurant*

---

### Bullet Selection: Choosing the Best 5

From these 18 options across 10 frames, the AM selects 5 and assigns them to slots. Selection criteria:

1. **Keyword coverage** — do the 5 selected bullets + title cover all P0 and P1 keywords?
2. **Persona coverage** — does each major persona find at least one bullet that speaks directly to them?
3. **Aware/Unaware balance** — are bullets 1-2 anchored in Aware attributes (to hook) and bullets 3-5 in Unaware attributes (to convert)?
4. **No redundancy** — each bullet should add something new, not repeat the same point differently
5. **Flow** — do the 5 bullets read well as a sequence? (hook → differentiate → reassure → expand → close)

**Example selection for Maurice's:**
| Slot | Selected | Frame | Primary Persona | Keywords Captured |
|------|----------|-------|----------------|------------------|
| 1 | 2a (Flavor) | Flavor/Experience | Purist, Foodie | mustard bbq, carolina, tangy, bbq sauce |
| 2 | 3a (Real Thing) | Differentiation | Purist, Nostalgic | carolina, mustard bbq, restaurant |
| 3 | 4a (All Natural) | Clean Label | Health-Conscious | all natural, no hfcs, mustard, vinegar |
| 4 | 5a (Endless Ways) | Use Cases | Purist, Foodie | pulled pork, chicken, ribs, dipping sauce |
| 5 | 7b (2-Pack + Value) | Value/Format | Gift, Repeat | 2 pack, 18oz, mustard bbq sauce |

**Keyword coverage check:**
```
P0 Keywords:     bbq sauce ✓(B1) | mustard bbq ✓(B1,B2) | carolina ✓(B1,B2) | all natural ✓(B3)
P1 Keywords:     pulled pork ✓(B4) | grilling ✗ | south carolina ✗ | no hfcs ✓(B3)
Gaps to fill:    "grilling" → backend | "south carolina" → title covers it
```

### Backend Keywords

**Main Search Terms field: 249 bytes** (not 250 — exceeding by even 1 byte de-indexes the ENTIRE field). No words repeated from title or bullets. No commas needed — space-separated. **No plurals** — Amazon handles stemming ("sauce" covers "sauces"). **Order matters** — Amazon gives more weight to keywords that appear earlier.

```
barbeque bar-b-que dipping pulled pork smoker rib grilling
marinade condiment tangy vinegar SC regional specialty
gourmet artisan gift basket cookout tailgate summer picnic
gluten free meat wing shrimp coleslaw
```

**Additional Backend Fields** (often overlooked — each provides extra indexed keyword space):

| Field | Limit | Maurice's Example |
|-------|-------|------------------|
| **Subject Matter** | 5 terms | South Carolina BBQ, Mustard Barbecue, Regional Sauce, Gourmet Condiment, Grilling Sauce |
| **Intended Use** | 5 terms | Grilling, Dipping, Marinating, Pulled Pork, Cookout |
| **Target Audience** | 5 terms | BBQ Enthusiasts, Home Cooks, Grill Masters, Food Gifters, Southern Food Fans |
| **Other Attributes** | Varies | All Natural, No HFCS, Restaurant Recipe |

These fields provide **3x more keyword space** beyond the main Search Terms field. Most sellers leave them blank — filling them is an easy win.

**AM Checkpoint:** "Which title? Which bullet combination? Any claims to verify with the client?"

---

## 13. Phase 10: Design Brief — Images & A+ Content

### What We're Doing
Creating detailed briefs for every image slot and A+ module, with multiple creative directions per slot, and a strategy for **threading** key messages across all visual elements.

### Trust Threading

Trust isn't built in one image. It's **woven** across the entire listing. If "authenticity" matters, the buyer should feel it in the main image, see it in the lifestyle shot, read it in the infographic, and absorb it in the A+ hero — not just in a single slide.

**Maurice's Trust Threads:**

| Trust Signal | Where It Appears |
|-------------|-----------------|
| **"This is authentic"** | Main image (premium label), Image 3 (real food), Image 6 (restaurant photo), A+ Module 1 (heritage), A+ Module 4 (comparison), Bullet 2 |
| **"This is safe to buy"** | Image 5 (ingredients), Bullet 3 (no HFCS), Bullet 5 (989 reviews), A+ Module 4 (transparency), A+ Module 6 (FAQ) |
| **"This is worth the price"** | Image 4 (restaurant-grade), Bullet 2 (same as restaurant), A+ Module 1 (heritage = authenticity costs more), A+ Module 3 (quality ingredients), Bullet 5 (2-pack value) |

### Amazon A+ Content Rules

- **Standard A+**: Up to 5 modules per layout from 17 available module types
- **Premium A+** (Brand Story required): 16-19 additional modules including video, interactive hotspots, carousel. Requirements: Brand Story published on ALL ASINs + at least 5 approved A+ projects in the last 12 months.
- **A+ body text is NOT indexed** for Amazon search. However, **image alt text IS indexed** — always fill alt text with keywords.
- **Comparison charts** can only compare **your own products** (2-6 ASINs). You cannot name or compare against competitors.
- Brand Story module: A carousel that appears above A+ content. Required for Premium A+ eligibility. Publish it on ALL ASINs even if minimal.

### Amazon Video Rules

- Duration: **6-45 seconds** (15-30 seconds is optimal)
- Format: MP4 or MOV, 16:9 aspect ratio
- Max file size: 500MB
- Resolution: 1920x1080 minimum
- No external URLs, QR codes, or competitor references
- Video significantly increases time-on-page (a ranking signal)

### Image Slot Briefs

**Up to 9 media slots** (1 main + up to 8 secondary including video). 7 visible in gallery without scrolling.

**Slot 1: Main Image — Stop the Scroll**
Amazon requires white background, product fills 85%+. No text overlays, no badges, no additional objects.
**Key advantage:** Our gold/yellow sauce POPS against competitors' red/brown in search results.

Creative directions:
- A: Classic studio — two bottles upright, hero lighting
- B: Angled duo — slight angle, dynamic, label clearly visible
- C: 3D render — hyper-clean, maximizes gold color vibrancy

---

**Slot 2: Close-Up — Show What's Inside**
Set flavor expectations. Buyer should think "that looks delicious."
- A: Slow golden pour showing consistency and color
- B: Ingredient beauty shot — mustard seeds, spices around bottle
- C: Open bottle, sauce on cap/rim — texture visible

---

**Slot 3: Lifestyle — Show It in Action**
Create desire. "I want to cook with this."
- A: Sauce brushed on pulled pork at backyard BBQ, warm lighting
- B: Plated meal — pulled pork sandwich with golden sauce, coleslaw
- C: Grilling scene — someone at the grill, bottle visible, friends nearby

---

**Slot 4: Infographic — Prove Differentiation**
"This is why it's different."
- A: Three pillars: Mustard-Based Recipe | Restaurant Heritage | All Natural
- B: Color comparison — golden Maurice's vs red competitors
- C: "3 Things You Should Know" — with icons and callouts

---

**Slot 5: Use Cases — Show Versatility**
"I can use this for so many things."
- A: 6-panel grid — pulled pork, chicken, ribs, dipping, coleslaw, marinade
- B: "One Sauce, Endless Possibilities" — visual wheel
- C: Before/after — plain grilled chicken vs sauced

---

**Slot 6: Heritage & Trust — Build Credibility**
"I trust this brand."
- A: Restaurant photo + "Same Recipe Since 1953"
- B: Timeline — 1953 → decades → "Now delivered to your door"
- C: Split — restaurant kitchen left, home BBQ right, "From Our Kitchen to Yours"

---

**Slot 7: Value + Close**
Last push to "Add to Cart."
- A: 2-pack value callout + review quotes
- B: Full flavor range (Original, Hot, Hickory, Sweet)
- C: "Join 989 Happy Customers" + star rating + value prop

---

**Slot 8: Video (15-30 seconds)**
Show the product in action — what images can't convey.
- A: "From Bottle to Table" — sauce being poured, brushed on meat, people tasting and reacting
- B: Heritage story — quick shots of restaurant, sauce being made, family BBQ at home
- C: Recipe/how-to — "3 Ways to Use Southern Gold" (pulled pork, wings, dipping)
Specs: MP4/MOV, 16:9, 1080p min, no external URLs or competitor mentions.

### A+ Content Module Briefs

| Module | Type | Purpose | Key Message |
|--------|------|---------|------------|
| 1 | Hero Banner | Set the emotional stage | "The Sauce That Built a Southern Legend" — heritage, 1953 |
| 2 | 4-Image Grid | Show the experience | Four use cases with appetizing photography |
| 3 | Feature Highlights | Three pillars | Mustard-Based, All Natural, SC Heritage |
| 4 | Comparison Chart | Cross-sell our variants | Original vs Hot vs Hickory vs Sweet (Amazon only allows comparing your OWN products, 2-6 ASINs) |
| 5 | Lifestyle Banner + FAQ | Show the payoff + handle objections | Backyard BBQ scene + "What is mustard BBQ?", "Is it spicy?", "Why $22?" |

**Standard A+ allows 5 modules per layout.** For more, qualify for Premium A+ (requires Brand Story on all ASINs + 5 approved A+ projects).

**Critical:** A+ body text is NOT indexed for search. But **image alt text IS indexed**. Always fill every A+ image's alt text with relevant keywords. This is free, hidden keyword space that most sellers ignore.

### Per-Attribute Visual Ideas (P0 Attributes)

**"Mustard-based recipe" — Feature + Uniqueness:**
| Visual Idea | Placement | Emotion | Text Overlay |
|------------|-----------|---------|-------------|
| Golden sauce vs red competitors side-by-side | Image 4, A+ Module 3 | Different, special | "Not Your Typical BBQ Sauce" |
| Ingredient beauty shot — mustard, vinegar, spices | Image 2 | Natural, crafted | "Real Mustard. Real Flavor." |
| Person trying it, pleasant surprise reaction | Image 3, A+ Module 2 | Discovery | "The Flavor You Didn't Know You Were Missing" |

**"70+ year restaurant heritage" — Trust + Uniqueness:**
| Visual Idea | Placement | Emotion | Text Overlay |
|------------|-----------|---------|-------------|
| Vintage restaurant photo + current bottle | A+ Module 1, Image 6 | Authentic, timeless | "Same Recipe Since 1953" |
| Restaurant kitchen → home BBQ split | Image 6, A+ Module 3 | Connection | "From Our Kitchen to Yours" |
| Heritage badge/seal design | Image 4 corner, Image 6 | Credibility | (none — visual trust signal) |

**"All-natural, no HFCS" — Problem Solver:**
| Visual Idea | Placement | Emotion | Text Overlay |
|------------|-----------|---------|-------------|
| Clean ingredient list zoomed with highlights | Image 5, A+ Module 4 | Transparency | "Nothing Artificial. Ever." |
| Flat-lay of actual ingredients | Image 2 alt, A+ Module 2 | Wholesome | "Just 7 Ingredients" |
| Our short label vs blurred long competitor label | Image 5, A+ Module 4 | Confidence | "Read the Label." |

**AM Checkpoint:** "Which creative directions should we pursue? Does the client have restaurant photos? Any brand guidelines or visual restrictions?"

---

## Compliance & Suppression Guardrails

### Why This Matters

Amazon actively suppresses non-compliant listings. A suppressed or non-compliant listing can see **up to 40% less traffic** — undoing all our optimization work. These guardrails should be checked before any listing goes live.

### Title Compliance Checklist

- [ ] Under 200 characters total
- [ ] First 70 characters contain P0 keywords and make sense standalone (mobile preview)
- [ ] No word appears more than 2 times (Jan 2025 enforcement)
- [ ] No ALL CAPS (except brand name if registered that way)
- [ ] No promotional language ("best", "#1", "sale", "free shipping")
- [ ] No special characters (!, $, @, ~)
- [ ] Brand name in correct position (first for most categories; last for Grocery unless high brand recognition)
- [ ] Size/quantity at the end

### Bullet Compliance Checklist

- [ ] Each bullet under 500 characters (250 for vendors)
- [ ] Total bullet content under 1,000 bytes for indexing purposes (front-load keywords in bullets 1-3)
- [ ] No full ALL CAPS bullets (ALL CAPS lead-in headers are OK)
- [ ] No promotional claims, pricing, shipping, or "guarantee" language
- [ ] No HTML tags

### Backend Keyword Compliance Checklist

- [ ] Main Search Terms field under **249 bytes** (count carefully — 1 byte over = entire field de-indexed)
- [ ] No brand names (yours or competitors') in backend
- [ ] No ASINs in backend
- [ ] No profanity, no "by" claims (e.g., "as seen on TV")
- [ ] Subject Matter, Intended Use, and Target Audience fields filled

### Image Compliance Checklist

- [ ] Main image: pure white background (RGB 255,255,255), product fills 85%+, no text/badges/watermarks
- [ ] No offensive or misleading content
- [ ] Minimum 1000px on longest side (2000px+ recommended for zoom)
- [ ] No Amazon trademarks or badges recreated in images

### A+ Content Compliance Checklist

- [ ] Comparison chart only compares YOUR OWN products (no competitor names)
- [ ] No pricing, promotional, shipping, or warranty info
- [ ] No external links or calls-to-action outside Amazon
- [ ] Alt text filled on all images (this IS indexed for search)
- [ ] No competitor brand names mentioned anywhere

### Category-Specific Rules (Grocery)

For Maurice's and similar food products:
- FBA requires minimum **105 days** remaining shelf life at time of inbound shipment
- FDA registration may be required
- Ingredient list must match physical label exactly
- Nutritional claims must be FDA-compliant

### Listing Quality Dashboard

After optimization, check the **Listing Quality Dashboard** in Seller Central. It scores your listing completeness and flags specific issues. Target: all green checkmarks across all recommended attributes.

---

## Amazon Ecosystem Features to Consider

These are Amazon features that interact with or enhance our listing optimization work:

### Brand Story Module
A scrollable carousel that appears above A+ content. Required for Premium A+ eligibility. Even a minimal version should be published on ALL ASINs. For Maurice's, this is a natural fit — use it for the heritage timeline / restaurant story.

### Virtual Bundles
Combine 2-5 FBA products into a virtual bundle with its own ASIN — no extra inventory needed. For Maurice's: bundle Original + Hot sauce as "The Piggie Park Sampler." Virtual bundles get their own listing and A+ content.

### Product Opportunity Explorer
Seller Central tool that shows search volume, trending keywords, and customer demand for niches. Use it during Phase 1 (Research) and Phase 7 (Keywords) to validate our keyword volume estimates.

### External Traffic & Brand Referral Bonus
Amazon gives a **10% referral fee rebate** on sales driven by external traffic (through Amazon Attribution). External traffic also gets up to **3x ranking weight** in Amazon's algorithm. This means: if the client drives traffic from social media, their Amazon ranking improves disproportionately. Worth noting in strategy discussions.

### Amazon's AI Listing Tools
Amazon now offers "Enhance My Listing" — AI-generated title, bullet, and description suggestions. 90% of merchants accept them. **Our system's value is the human-expert layering**: brand voice, positioning, buyer psychology, and strategic keyword threading that generic AI can't do. But we should check what Amazon's AI suggests to ensure we're not worse than the default.

---

## Summary: The Complete Pipeline

| # | Phase | What It Produces | Key Concepts |
|---|-------|-----------------|-------------|
| 1 | Research | Evidence corpus from 7 domains + SQP + Data Dive | Review mining, competitor analysis, cultural research |
| 2 | Brand Understanding | Brand identity, founder story, brand voice, local culture | How would a local salesman talk about this? |
| 3 | Product Knowledge | Essence, uniqueness, value props, subjective/objective classification | Does the buyer need education? How emotional is the purchase? |
| 4 | Positioning | Positioning statements (P0 selected), competitor map, mass vs niche | "Which competitive frame are we in?" |
| 5 | Buyer Personas | 4-6 personas with decision drivers, objections, unstated needs | What they search for vs what they need to see |
| 6 | Product Attributes | Tagged, prioritized, persona-mapped, **aware/unaware** classified | Aware captures, Unaware converts |
| 7 | Keyword Strategy | Categorized keywords from ALL upstream intelligence | SQP gaps, Data Dive volumes, cultural terms |
| 8 | Real Estate Allocation | Framework + product-specific map of what goes where | Aware high, Unaware deep, P0 threaded, mobile-first |
| 9 | Listing Copy | Title options, bullet options, backend keywords | 3 titles, 10 bullets, AM picks the combination |
| 10 | Design Brief | Image briefs, A+ briefs, trust threading map | Multiple creative directions per slot |

Each phase builds on all previous phases. The AM validates at 8 checkpoints. Every claim traces back to evidence. Nothing is guesswork.

**Post-optimization:** Run the listing through the Compliance Guardrails checklist, check the Listing Quality Dashboard, and consider Brand Story / Virtual Bundle opportunities.

---

*End of Listing Optimization System Design*
