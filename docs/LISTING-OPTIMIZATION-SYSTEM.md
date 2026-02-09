# Listing Optimization System

> **Status:** Design Document
> **Created:** 2026-02-09
> **Purpose:** Complete system design for the listing optimization workflow — from raw product inputs to fully optimized Amazon listing with title, bullets, images, and A+ content.

---

## Table of Contents

1. [Vision & Problem Statement](#1-vision--problem-statement)
2. [System Overview: The Pipeline](#2-system-overview-the-pipeline)
3. [Input Model](#3-input-model)
4. [Phase 1: Research & Discovery](#4-phase-1-research--discovery)
5. [Phase 2: Product Knowledge Base](#5-phase-2-product-knowledge-base)
6. [Phase 3: Buyer Personas](#6-phase-3-buyer-personas)
7. [Phase 4: Product Attributes & Features](#7-phase-4-product-attributes--features)
8. [Phase 5: Keyword Strategy](#8-phase-5-keyword-strategy)
9. [Phase 6: Listing Copy — Title & Bullets](#9-phase-6-listing-copy--title--bullets)
10. [Phase 7: Design Brief — Images & A+ Content](#10-phase-7-design-brief--images--a-content)
11. [Domain Model: Entities & Relationships](#11-domain-model-entities--relationships)
12. [System Architecture](#12-system-architecture)
13. [Human-in-the-Loop Checkpoints](#13-human-in-the-loop-checkpoints)
14. [Example Flow: Maurice's Piggie Park BBQ Sauce](#14-example-flow-maurices-piggie-park-bbq-sauce)

---

## 1. Vision & Problem Statement

### The Problem

Every Amazon listing is a **conversion machine**. But most listings are built on guesswork — someone writes bullets based on what they *think* matters, picks images based on what *looks nice*, and stuffs keywords based on what *seems relevant*.

The result: listings that **describe** the product but don't **sell** it.

Selling requires a fundamentally different process:
1. **Know the product deeply** — not just what it is, but why it exists, what makes it irreplaceable, what cultural and emotional context it carries
2. **Know the buyer deeply** — not demographics, but decision drivers, objections, unstated needs, and what they *actually* search for
3. **Map product truths to buyer needs** — which features matter to which buyers, in what priority, and how to express each one
4. **Express those mappings** across every listing element — title, bullets, images, A+ content — with surgical precision

### The Vision

A structured, phased pipeline that transforms raw product inputs (a URL, a blob of text, some competitor ASINs) into a **fully optimized listing strategy** with:
- Deep product knowledge base with cultural and competitive context
- Evidence-based buyer personas with decision driver maps
- Prioritized, tagged, persona-mapped product attributes
- Categorized, prioritized keyword strategy
- Multiple title and bullet point options
- Comprehensive design briefs for images and A+ content

Every output builds on the previous. Every claim is evidence-backed. Every decision is documented.

### What This Is NOT

This is not a one-shot "generate a listing" tool. This is a **structured thinking framework** that produces artifacts at each stage, verified by a human, before proceeding to the next stage. The AI does the heavy lifting of research, synthesis, and generation — but the AM validates and steers at every checkpoint.

---

## 2. System Overview: The Pipeline

```
INPUTS                    RESEARCH                  KNOWLEDGE              STRATEGY                OUTPUT
─────────────────────    ──────────────────────    ───────────────────    ────────────────────    ──────────────────────
                          ┌─────────────────┐      ┌────────────────┐    ┌──────────────────┐    ┌────────────────────┐
 Existing Listing ──────► │                 │      │                │    │                  │    │                    │
 D2C Website ───────────► │  Research &     │─────►│  Product       │───►│  Attributes &    │───►│  Title Options     │
 Social Links ──────────► │  Discovery      │      │  Knowledge     │    │  Features        │    │  Bullet Options    │
 User Text ─────────────► │                 │─────►│  Base          │───►│  (tagged, mapped)│───►│  Backend Keywords  │
 Competitor ASINs ──────► │  (Phase 1)      │      │  (Phase 2)     │    │  (Phase 4)       │    │  (Phase 6)         │
 Brand Context ─────────► │                 │      │                │    │                  │    │                    │
                          └────────┬────────┘      └───────┬────────┘    └────────┬─────────┘    └────────────────────┘
                                   │                       │                      │
                                   │               ┌───────▼────────┐    ┌────────▼─────────┐    ┌────────────────────┐
                                   │               │                │    │                  │    │                    │
                                   └──────────────►│  Buyer         │───►│  Keyword         │───►│  Design Brief:     │
                                                   │  Personas      │    │  Strategy        │    │  Images & A+       │
                                                   │  (Phase 3)     │    │  (Phase 5)       │    │  (Phase 7)         │
                                                   │                │    │                  │    │                    │
                                                   └────────────────┘    └──────────────────┘    └────────────────────┘

CHECKPOINTS: ✋ Human validates after Phase 2, Phase 3, Phase 4, Phase 6, Phase 7
```

### Phase Dependencies

| Phase | Depends On | Produces |
|-------|-----------|----------|
| Phase 1: Research & Discovery | Inputs | Raw evidence corpus |
| Phase 2: Product Knowledge Base | Phase 1 | Product essence, cultural context, value propositions |
| Phase 3: Buyer Personas | Phase 1 + Phase 2 | Persona profiles with decision drivers |
| Phase 4: Product Attributes | Phase 2 + Phase 3 | Tagged, prioritized, persona-mapped attributes |
| Phase 5: Keyword Strategy | Phase 3 + Phase 4 | Categorized, prioritized keywords |
| Phase 6: Listing Copy | Phase 4 + Phase 5 | Title options, bullet options, backend keywords |
| Phase 7: Design Brief | Phase 2 + Phase 3 + Phase 4 | Image strategy, A+ module strategy |

---

## 3. Input Model

The system accepts multiple input types. Not all are required — more inputs produce richer outputs.

### 3.1 Input Types

#### Input A: Existing Amazon Listing (ASIN)

```yaml
input_type: existing_listing
asin: "B07P15CB6X"
marketplace: "US"  # US, UK, CA, etc.
```

**What the system extracts:**
- Current title, bullets, description
- Images (all slots)
- A+ content (if present)
- Price, rating, review count
- Category, BSR
- Variant structure (parent/child)
- Review corpus (our own reviews — positive and negative themes)
- Q&A section
- Competitor products shown in "Customers also viewed"

#### Input B: D2C Website

```yaml
input_type: d2c_website
url: "https://www.piggiepark.com/"
pages_to_crawl:
  - homepage
  - product_page
  - about_page
  - faq_page
```

**What the system extracts:**
- Brand story and positioning
- Product descriptions (often richer than Amazon)
- Visual identity (colors, photography style, tone)
- Value propositions as communicated off-Amazon
- Pricing (may differ from Amazon)
- Customer testimonials
- FAQ content (reveals customer concerns)

#### Input C: Social Media Links

```yaml
input_type: social_media
platforms:
  - platform: instagram
    url: "https://instagram.com/piggiepark"
  - platform: tiktok
    url: "https://tiktok.com/@piggiepark"
```

**What the system extracts:**
- Brand voice and personality
- Content themes that resonate (high engagement)
- Customer comments and questions
- User-generated content themes
- Lifestyle context (how customers use the product)
- Cultural positioning

#### Input D: User Text (AM or Client Knowledge Dump)

```yaml
input_type: user_text
content: |
  Maurice's is a legendary SC BBQ restaurant chain that's been around
  since the 1950s. Their mustard-based sauce is what SC BBQ is known for.
  Most people outside the south don't know mustard BBQ even exists...
```

**What the system extracts:**
- Insider knowledge not available from public sources
- Brand stories and origin narratives
- Client's own understanding of their differentiation
- Known customer segments from the client's perspective
- Competitive intelligence the client holds
- Historical context and cultural significance

#### Input E: Competitor List

```yaml
input_type: competitors
amazon_competitors:
  - asin: "B01DOIMC1E"  # Lillie's Q Gold
  - asin: "B004YVWO9Q"  # Cattlemen's Carolina
  - asin: "B00MRB9QP0"  # Bessinger's
off_amazon_competitors:
  - name: "Rufus Teague"
    url: "https://rufusteague.com"
  - name: "Stubbs"
    url: "https://stubbsbbq.com"
```

**What the system extracts (per competitor):**
- Full listing data (same as Input A extraction)
- Review corpus analysis (what customers love/hate about THEM)
- Price positioning
- Listing quality assessment
- Visual strategy analysis
- A+ content structure
- Keyword strategy reverse-engineering (from title, bullets, backend)

#### Input F: Brand Context / Facts

```yaml
input_type: brand_context
facts:
  - "Founded in 1953 by Maurice Bessinger"
  - "Recipe has never changed"
  - "Sauce is made in the same facility as the restaurants use"
  - "No HFCS, no preservatives, no artificial colors"
  - "They ship 50,000+ bottles per year"
certifications: ["USDA approved", "All-natural"]
brand_values: ["Authenticity", "Tradition", "Quality ingredients"]
```

### 3.2 Minimum Viable Input

The system can work with **any single input**, but quality scales with input richness:

| Input Combination | Output Quality | Recommended? |
|-------------------|---------------|--------------|
| User text only | Basic (60%) | For new products not yet listed |
| ASIN only | Good (75%) | Quick optimization of existing listing |
| ASIN + competitors | Better (85%) | Standard optimization |
| ASIN + competitors + D2C + user text | Best (95%) | Full listing optimization |
| All inputs | Maximum (100%) | Premium engagement |

---

## 4. Phase 1: Research & Discovery

### Purpose
Transform raw inputs into an **evidence corpus** — a structured collection of facts, observations, themes, and data points that all subsequent phases draw from.

### 4.1 Research Domains

The system researches across seven domains:

#### Domain 1: Product Deep-Dive
- What is this product at its most fundamental level?
- What category does it belong to? What adjacent categories exist?
- What is the manufacturing process? Does the process matter to buyers?
- What are the ingredients/materials/components?
- What certifications or standards does it meet?
- What is the shelf life, care instructions, or maintenance?

#### Domain 2: Cultural & Historical Context
- Does this product have regional/cultural significance?
- Is there a heritage or origin story?
- Is there a trend or movement this product connects to? (e.g., clean eating, artisan foods, sustainability)
- Are there cultural moments that spike demand? (holidays, events, seasons)
- Is there media coverage, celebrity usage, or cultural references?
- What does Reddit/social say about products in this category?

**Evidence Sources:**
- Reddit threads (r/BBQ, r/grilling, r/Cooking, r/hotsauce, etc.)
- Food blogs and review sites
- YouTube reviews and cooking channels
- News articles, listicles ("Best BBQ Sauces of 2026")
- Social media engagement patterns

#### Domain 3: Competitive Landscape
- Who are the direct competitors (same sub-category)?
- Who are the indirect competitors (adjacent products that solve the same problem)?
- What is each competitor's positioning? (premium, value, artisan, mass-market)
- What are competitors doing well? Doing poorly?
- Where is the white space — unaddressed needs or underserved segments?

**Analysis per competitor:**
```
Competitor: [Name]
├── Positioning: [premium/value/artisan/mass-market]
├── Price: [$X] ($Y/unit)
├── Rating: [X.X] ([N] reviews)
├── Listing Quality: [score]
│   ├── Title: [assessment]
│   ├── Images: [assessment]
│   ├── Bullets: [assessment]
│   └── A+ Content: [assessment]
├── Strengths: [what they do well]
├── Weaknesses: [what they do poorly]
├── Review Themes (Positive): [what customers love]
├── Review Themes (Negative): [what customers complain about]
└── Keyword Strategy: [inferred from title/bullets]
```

#### Domain 4: Review Mining
The most valuable evidence source. Reviews are **unfiltered customer voice**.

**Our own reviews:**
- What do 5-star reviewers specifically praise? (exact language matters)
- What do 1-3 star reviewers complain about?
- What expectations were unmet?
- What use cases do customers describe?
- What comparisons do customers make? ("better than X", "switched from Y")
- What questions do customers ask in Q&A?

**Competitor reviews:**
- What do their customers wish was different?
- What do their customers love that we also have?
- What gaps exist that we can address?
- What language do customers use to describe the category?

**Review mining outputs:**
```
Theme: [theme name]
├── Frequency: [how often mentioned across reviews]
├── Sentiment: [positive/negative/mixed]
├── Example Quotes: ["exact customer words"]
├── Implication for Us: [how this should inform our listing]
└── Where to Address: [title/bullet/image/A+]
```

#### Domain 5: Search Behavior
- What do people actually type when looking for this product?
- What are the auto-complete suggestions on Amazon?
- What are the "Customers also searched for" suggestions?
- What keywords do top competitors rank for?
- What is the search volume distribution? (head vs. long-tail)

#### Domain 6: Category Norms & Expectations
- What does a "standard" listing look like in this category?
- What image styles dominate? (lifestyle, studio, infographic)
- What price range is "normal"?
- What bullet point themes are common?
- What A+ content modules are popular?
- What review count is considered "established"?

#### Domain 7: Brand Ecosystem
- What else does this brand sell?
- How does this product fit in the brand portfolio?
- Is there cross-sell potential?
- What is the brand's overall Amazon reputation?
- What is the brand store like?

### 4.2 Research Output: Evidence Corpus

The research phase produces a structured evidence corpus that serves as the foundation for all subsequent phases:

```yaml
evidence_corpus:
  product_facts: []         # Verified facts about the product
  cultural_context: []      # Cultural/historical significance
  competitor_profiles: []   # Per-competitor analysis
  review_themes_own: []     # Themes from our reviews
  review_themes_comp: []    # Themes from competitor reviews
  search_terms: []          # Discovered search terms
  category_norms: []        # What's standard in the category
  brand_context: []         # Brand ecosystem facts
  external_evidence: []     # Reddit, blogs, media mentions
  quotes: []                # Direct customer quotes (verbatim)
```

### 4.3 Human Checkpoint: Research Review

Before proceeding, the AM reviews the evidence corpus:
- Are there missing competitors we should add?
- Is the cultural context accurate?
- Are there product facts the system got wrong?
- Are there insider insights to add?

---

## 5. Phase 2: Product Knowledge Base

### Purpose
Synthesize the evidence corpus into a **deep, structured understanding** of what this product truly is — not just its physical attributes, but its essence, its place in culture, and its hierarchy of value.

### 5.1 Product Essence

The most fundamental question: **What is this product, really?**

Not "it's a BBQ sauce." That's the category. The essence is deeper:

```
Product Essence Framework:
├── Category Definition
│   What is it? → "Mustard-based BBQ sauce"
│   What category? → "BBQ Sauces > Regional Styles > Carolina Mustard"
│
├── Functional Identity
│   What does it DO? → "Flavors and enhances grilled/smoked meats"
│   What problem does it solve? → "Access to authentic SC mustard BBQ outside the region"
│   What use cases? → "Grilling, dipping, marinating, gift-giving"
│
├── Emotional Identity
│   What does it MEAN? → "A taste of South Carolina BBQ heritage"
│   What feeling? → "Nostalgia, authenticity, discovery"
│   What identity does buying it signal? → "I appreciate real food, not mass-produced"
│
├── Cultural Identity
│   What cultural moment? → "SC mustard BBQ is a distinct tradition within American BBQ"
│   What movement? → "Artisan/heritage food, regional food tourism"
│   What tribe? → "BBQ enthusiasts, southern food lovers, foodies"
│
└── Origin Identity
    Where does it come from? → "Maurice's Piggie Park, SC, est. 1953"
    Why does origin matter? → "Recipe authenticity, 70+ year heritage, restaurant-proven"
    What's the story? → "Same sauce served in the restaurants since the 1950s"
```

### 5.2 Uniqueness Analysis

What makes this product different? Uniqueness can live in different places:

```
Uniqueness Matrix:
├── Recipe/Formulation Uniqueness
│   What's different about the product itself?
│   → "Mustard-based (vs tomato), all-natural, no HFCS, no preservatives"
│   Rarity: How rare is this? → "Only ~5% of BBQ sauces are mustard-based"
│
├── Process Uniqueness
│   Is the manufacturing process special?
│   → "Made in the same facility as restaurant sauces, same recipe since 1953"
│   Verifiability: Can the customer verify this? → "Restaurant chain validates"
│
├── Origin Uniqueness
│   Is where/who it comes from special?
│   → "From a legendary SC BBQ chain with 70+ year history"
│   Authenticity Signal: What proves this is real? → "Active restaurants you can visit"
│
├── Experience Uniqueness
│   Is the taste/feel/result different?
│   → "Tangy, vinegary, mustard-forward — nothing like Kansas City BBQ"
│   Describability: Can you describe it compellingly? → "Southern Gold"
│
├── Category Uniqueness
│   Is this product creating or defining a sub-category?
│   → "Yes — SC mustard BBQ is a sub-category most Americans don't know exists"
│   Education Required: Does the buyer need education? → "Yes — must learn what mustard BBQ is"
│
└── Value Uniqueness
    Is the value proposition itself unique?
    → "Only restaurant-heritage mustard BBQ sauce available nationally on Amazon"
    Defensibility: Can competitors copy this? → "Heritage can't be manufactured"
```

### 5.3 Value Proposition Hierarchy

Not all value propositions are equal. They form a hierarchy:

```
Value Proposition Hierarchy:
│
├── Core Value Proposition (the ONE thing)
│   "Authentic South Carolina mustard BBQ sauce from a legendary 70+ year restaurant chain"
│   Why this is core: It's the intersection of uniqueness + demand + defensibility
│
├── Primary Supporting Value Props (directly support the core)
│   1. "Restaurant heritage = proven, trusted recipe"
│   2. "Mustard-based = unique flavor you can't find elsewhere"
│   3. "All-natural ingredients, no HFCS"
│
├── Secondary Value Props (add value but aren't the reason to buy)
│   1. "2-pack format = better value for regular users"
│   2. "Versatile — works on pork, chicken, as a dip"
│   3. "Amazon's Choice badge = social proof"
│   4. "989 reviews at 4.7 stars = trusted"
│
└── Tertiary Value Props (nice-to-have, for specific segments)
    1. "Makes a great gift for BBQ lovers"
    2. "Prime shipping = fast delivery"
    3. "Multi-flavor variety available"
```

### 5.4 Product Knowledge Base Structure

The full Product Knowledge Base (PKB) is a structured document:

```yaml
product_knowledge_base:
  # Identity
  product_name: string
  asin: string
  category_path: string[]

  # Essence
  category_definition: string       # What is it?
  functional_identity: string       # What does it do?
  emotional_identity: string        # What does it mean?
  cultural_identity: string         # What cultural context?
  origin_identity: string           # Where does it come from?

  # Uniqueness
  uniqueness_dimensions:
    - dimension: string             # recipe, process, origin, experience, category, value
      claim: string                 # What's unique?
      evidence: string[]            # Proof points
      rarity: high | medium | low   # How rare is this?
      defensibility: high | medium | low  # Can competitors copy?
      buyer_perception: string      # Does the buyer actually care?

  # Value Propositions
  core_value_proposition: string
  primary_value_props: string[]
  secondary_value_props: string[]
  tertiary_value_props: string[]

  # Context
  cultural_context: string          # Heritage, trends, movements
  competitive_position: string      # premium/value/artisan/mass-market
  category_education_needed: boolean # Does buyer need to learn something?
  education_message: string         # What must they learn?

  # Constraints
  premium_justification: string     # Why it costs more (if applicable)
  known_objections: string[]        # Anticipated buyer pushback
  expectation_mismatches: string[]  # Where reality ≠ expectation
```

### 5.5 Human Checkpoint: Knowledge Base Review

The AM reviews the PKB for accuracy:
- Is the essence captured correctly?
- Are uniqueness claims actually true?
- Is the value prop hierarchy the right priority?
- Are there value props we're missing?
- Is the cultural context accurate?
- Does the client agree with this positioning?

---

## 6. Phase 3: Buyer Personas

### Purpose
Build **evidence-based buyer personas** that go beyond demographics into decision psychology — what drives them, what stops them, what they search for, and what they need to see to convert.

### 6.1 Persona Discovery

Personas emerge from evidence, not imagination. Sources:

| Source | What It Reveals |
|--------|----------------|
| Our reviews | Who actually buys, what they value, how they describe the product |
| Competitor reviews | Who buys alternatives, what they wish was different |
| Search terms | How different segments think about the category |
| Reddit/forums | Unfiltered discussions about purchase decisions |
| Q&A section | What specific concerns buyers have pre-purchase |
| Social media | Lifestyle context, usage occasions, demographics |

### 6.2 Persona Structure

Each persona has four dimensions:

```yaml
buyer_persona:
  id: string
  name: string                      # Descriptive name (e.g., "The BBQ Purist")
  estimated_traffic_share: percentage # What % of our traffic is this persona?

  # WHO they are
  demographics:
    age_range: string
    gender_skew: string             # male/female/balanced
    location_skew: string           # regional? national?
    income_level: string
    lifestyle: string               # brief description

  # HOW they search
  search_behavior:
    primary_keywords: string[]      # What they type into Amazon
    search_intent: string           # What they're trying to find
    intent_type: branded | category | problem_solving | comparison | impulse
    research_depth: quick_decider | moderate | deep_researcher
    funnel_stage: awareness | consideration | decision | purchase

  # WHAT they need to see to convert
  decision_drivers:
    - driver: string                # e.g., "Authentic heritage"
      importance: P0 | P1 | P2     # How critical to conversion
      where_to_address: string[]    # title, bullet, image, A+
      evidence: string              # Where we learned this matters

  # WHAT stops them from buying
  objections:
    - objection: string             # e.g., "Price seems high"
      severity: high | medium | low
      where_to_address: string[]    # Where in the listing to counter
      how_to_address: string        # The specific counter
      evidence: string

  # WHAT they don't search for but need to see
  unstated_needs:
    - need: string                  # e.g., "Want to know it's natural/clean label"
      importance: P0 | P1 | P2
      where_to_address: string[]
      evidence: string

  # HOW they use the product
  use_cases:
    - case: string                  # e.g., "Weekend grilling with friends"
      frequency: string
      context: string               # When/where/why
```

### 6.3 Persona-to-Listing Mapping

Each persona has different "hot zones" in the listing:

```
Listing Element Importance by Persona:
                        BBQ Purist    SC Nostalgic    Gift Buyer    Health-Conscious
Title                   ●●●●○         ●●●○○           ●●○○○         ●●●○○
Main Image              ●●●●○         ●●●●●           ●●●●●         ●●○○○
Bullet 1-2              ●●●●●         ●●●○○           ●●●○○         ●●●●●
Bullet 3-5              ●●●○○         ●●○○○           ●●○○○         ●●●●●
Secondary Images        ●●●●○         ●●●●●           ●●●●●         ●●●○○
A+ Content              ●●●○○         ●●●●●           ●●○○○         ●●●●○
Reviews Section         ●●●●●         ●●●○○           ●●●●○         ●●●●●
Price                   ●●○○○         ●●○○○           ●●●●●         ●●●○○

● = Importance level (5 = critical, 1 = minimal)
```

### 6.4 Human Checkpoint: Persona Validation

The AM reviews personas:
- Do these match who you actually see buying?
- Are we missing a segment?
- Are the traffic share estimates reasonable?
- Are the decision drivers and objections accurate?
- Does the client recognize these buyer types?

---

## 7. Phase 4: Product Attributes & Features

### Purpose
Create a **master list of every product attribute** that could matter to any buyer, tagged by type, priority, persona relevance, and evidence source. This list is the foundation for everything that follows — keywords, bullets, images, A+ content.

### 7.1 Attribute Taxonomy

Every attribute is tagged with a **type** that describes what kind of value it provides:

| Type Tag | Definition | Example |
|----------|-----------|---------|
| `feature` | A factual characteristic of the product | "Mustard-based recipe" |
| `uniqueness` | Something that differentiates from competitors | "70+ year restaurant heritage" |
| `problem_solver` | Addresses a specific customer pain point | "No HFCS — clean label for health-conscious buyers" |
| `trust_signal` | Builds credibility and reduces purchase risk | "989 reviews at 4.7 stars" |
| `experience` | Describes the outcome/feeling of using the product | "Tangy, bold flavor unlike any store-bought sauce" |
| `social_proof` | External validation (awards, media, reviews) | "Amazon's Choice badge" |
| `convenience` | Makes the buying/using experience easier | "2-pack so you don't run out mid-season" |
| `value` | Addresses price-to-worth perception | "Restaurant-quality sauce at grocery prices" |

### 7.2 Priority Classification

Each attribute gets a priority level:

| Priority | Definition | Implication |
|----------|-----------|-------------|
| **P0** — Must Show | If the customer doesn't see this, they won't buy. Core conversion drivers. | Must appear in title, first 2 bullets, AND images |
| **P1** — Should Show | Strengthens the purchase decision. Important for comparison shoppers. | Should appear in bullets, images, OR A+ content |
| **P2** — Nice to Show | Adds value for specific segments. Not a dealbreaker. | Can appear in A+, backend keywords, or later bullets |
| **P3** — Background | Supporting facts that may help with indexing or edge cases. | Backend keywords only, or buried in description |

### 7.3 Attribute Structure

```yaml
product_attribute:
  id: string
  attribute: string                     # The attribute itself
  description: string                   # Expanded explanation

  # Classification
  type: feature | uniqueness | problem_solver | trust_signal | experience | social_proof | convenience | value
  priority: P0 | P1 | P2 | P3

  # Evidence
  evidence_source: string               # Where we found this matters
  evidence_type: review | competitor_gap | search_data | cultural | user_input | product_fact
  evidence_strength: strong | moderate | weak
  supporting_quotes: string[]           # Verbatim customer quotes if available

  # Persona Mapping
  relevant_personas:
    - persona_id: string
      relevance: primary | secondary    # Is this a key driver or supporting?

  # Listing Placement
  recommended_placement:
    - element: title | bullet | image | a_plus | backend | description
      priority_in_element: number       # 1 = first/most prominent
      expression: string                # How to express this in that element

  # Competitive Context
  competitor_comparison:
    us: string                          # Our position on this attribute
    competitors:
      - name: string
        position: string                # Their position
        our_advantage: boolean          # Do we win on this?
```

### 7.4 Attribute Mapping Matrix

The master view that connects attributes to personas and listing elements:

```
                        PERSONAS                    LISTING PLACEMENT           COMPETITIVE
Attribute          Type      Pri   Purist  Nostalg  Gift  Health   Title  Bullet  Image  A+    Win?
─────────────────────────────────────────────────────────────────────────────────────────────────────
Mustard-based      feature    P0   ●pri    ●sec     -     -        ✓1     ✓1      ✓      ✓     ✓
SC heritage        unique     P0   ●sec    ●pri     ●sec  -        ✓2     ✓2      ✓      ✓     ✓
Restaurant recipe  trust      P0   ●pri    ●pri     ●sec  -        ✓3     ✓3      ✓      ✓     ✓
All-natural/NoHFCS problem    P1   ●sec    -        -     ●pri     ✓4     ✓1      ✓      ✓     ✓
Tangy bold flavor  experience P1   ●pri    ●pri     -     -        -      ✓2      ✓      ✓     =
2-pack format      convenience P1  ●sec    -        ●pri  -        ✓5     ✓4      -      -     =
4.7★ / 989 reviews trust      P1   ●sec    -        ●pri  ●sec     -      -       -      ✓     ✗
Versatile uses     feature    P2   ●sec    -        -     ●sec     -      ✓5      ✓      ✓     =
Gift-worthy        value      P2   -       -        ●pri  -        -      -       ✓      ✓     ✓
Amazon's Choice    social     P2   -       -        ●sec  -        -      -       -      -     =
Prime eligible     convenience P3  -       -        ●sec  -        -      -       -      -     =
```

### 7.5 Human Checkpoint: Attribute Review

The AM reviews the attribute list:
- Is the priority ranking correct? Should any P1 be P0?
- Are we missing any attributes?
- Is the persona mapping accurate?
- Does the competitive comparison reflect reality?
- Which attributes does the client want to emphasize most?

---

## 8. Phase 5: Keyword Strategy

### Purpose
Build a **categorized, prioritized keyword list** that maps every relevant search term to its type, intent, volume, and placement strategy.

### 8.1 Keyword Taxonomy

| Keyword Type | Definition | Example | Strategy |
|-------------|-----------|---------|----------|
| **Master Keyword** | The core product category term. Highest volume, highest competition. | "bbq sauce" | Title position 1-2, exact match campaigns |
| **Sub-Category Keyword** | More specific category term. Medium volume. | "mustard bbq sauce", "carolina bbq sauce" | Title, bullets, backend |
| **Long-Tail Keyword** | Specific multi-word phrases. Lower volume, higher intent. | "south carolina mustard bbq sauce for pulled pork" | Backend, bullets, A+ |
| **Branded Keyword** | Includes our brand name. | "maurice's bbq sauce", "piggie park sauce" | Title (brand first), brand defense campaigns |
| **Competitor Branded** | Includes competitor brand names. | "sweet baby ray's alternative" | Product targeting ads, comparison A+ |
| **Problem/Use-Case Keyword** | Describes a need, not a product. | "sauce for pulled pork", "best grilling sauce" | Bullets, A+, backend |
| **Question Keyword** | Framed as a question. | "what is mustard bbq sauce" | A+ content, Q&A, blog content |
| **Seasonal Keyword** | Time-bound relevance. | "bbq sauce for memorial day", "grilling season sauce" | Seasonal campaigns, temporary bullets |
| **Cross-Sell Keyword** | Adjacent product searches. | "bbq gift basket", "southern food box" | Gift-related bullets, A+, cross-campaigns |

### 8.2 Keyword Structure

```yaml
keyword_entry:
  keyword: string
  type: master | sub_category | long_tail | branded | competitor_branded | problem | question | seasonal | cross_sell
  priority: P0 | P1 | P2 | P3

  # Search Intelligence
  estimated_volume: high | medium | low   # Relative within category
  competition: high | medium | low
  conversion_likelihood: high | medium | low

  # Intent Classification
  search_intent: string                    # What the searcher wants
  intent_type: branded | category | problem_solving | comparison | impulse
  funnel_stage: awareness | consideration | decision | purchase
  primary_persona: string                  # Which persona uses this keyword

  # Listing Placement
  placement:
    title: boolean                         # Should this be in the title?
    title_position: number | null          # If yes, where? (1 = most prominent)
    bullets: boolean
    bullet_position: number | null
    backend: boolean
    a_plus_text: boolean
    description: boolean

  # Advertising Strategy
  ad_strategy:
    campaign_type: SP | SB | SD
    match_types: [exact, phrase, broad]
    target_acos: percentage
    bid_strategy: string
```

### 8.3 Keyword Priority Framework

```
P0 Keywords (Must Index + Must Rank):
├── Master keywords: "bbq sauce", "barbecue sauce"
├── Core sub-category: "mustard bbq sauce", "carolina bbq sauce"
├── Brand terms: "maurice's bbq sauce", "piggie park"
└── Rationale: These drive >60% of relevant traffic

P1 Keywords (Must Index + Should Rank):
├── Specific sub-category: "south carolina bbq sauce", "yellow bbq sauce", "gold bbq sauce"
├── High-intent long-tail: "mustard bbq sauce for pulled pork"
├── Use-case terms: "bbq dipping sauce", "grilling sauce"
└── Rationale: These drive 20-30% of traffic with higher conversion

P2 Keywords (Should Index):
├── Adjacent terms: "southern cooking sauce", "bbq gift", "meat marinade"
├── Problem terms: "sauce for smoker", "best sauce for ribs"
├── Question terms: "what is mustard bbq"
└── Rationale: Incremental traffic, some high-converting

P3 Keywords (Backend Only):
├── Misspellings: "barbeque", "bar-b-que", "bar b q"
├── Synonyms: "condiment", "table sauce"
├── Regional variations: "SC BBQ", "Southern Gold"
└── Rationale: Catch long-tail without cluttering listing
```

### 8.4 Keyword-to-Listing Placement Map

```
TITLE (150 chars, front-load important terms):
Position 1: Brand name → "Maurice's"
Position 2: Core product → "Southern Gold BBQ Sauce"
Position 3: Category keyword → "Carolina Mustard Barbecue Sauce"
Position 4: Key differentiator → "Original Restaurant Recipe"
Position 5: Format/Size → "18oz (Pack of 2)"

BULLETS (indexed, high weight):
Bullet 1: P0 feature keyword + benefit
Bullet 2: P0 uniqueness keyword + benefit
Bullet 3: P1 problem-solving keyword + benefit
Bullet 4: P1 use-case keyword + benefit
Bullet 5: P1 value/convenience keyword + benefit

BACKEND KEYWORDS (250 bytes, no repeats from title/bullets):
All P2 and P3 keywords not used in title or bullets
Misspellings, synonyms, regional variations

A+ CONTENT (indexed for alt-text only):
Question keywords → FAQ module
Comparison keywords → comparison chart module
Use-case keywords → lifestyle images with alt text
```

### 8.5 Human Checkpoint: Keyword Review

The AM reviews the keyword strategy:
- Are the priority rankings correct?
- Are we missing important keywords?
- Does the placement strategy make sense?
- Are there competitor keywords we should target?
- Does the client have keyword preferences or restrictions?

---

## 9. Phase 6: Listing Copy — Title & Bullets

### Purpose
Generate **multiple options** for the Amazon title and bullet points, each optimized for different trade-offs (keyword density vs. readability, feature-focused vs. emotion-focused, etc.).

### 9.1 Amazon Title Rules & Constraints

| Rule | Constraint |
|------|-----------|
| Max length | 200 characters (150 recommended for mobile) |
| Brand name | Must be first (Amazon policy for most categories) |
| No ALL CAPS | Title case or sentence case only |
| No promotional language | No "Best Seller", "Hot Item", "Free Shipping" |
| No special characters for decoration | No ★, ♥, etc. |
| Variant info | Size/color/count at end |
| Human readable | Must make sense as a sentence-like phrase |
| Keyword indexed | Amazon indexes every word in the title for search |
| Mobile truncation | First ~80 chars show on mobile search results |

### 9.2 Title Formula

```
[Brand] [Product Name] [Core Category Keyword] – [Key Differentiator/Benefit] – [Secondary Feature] – [Size/Format]
|_____________________________80 chars (mobile visible)_____________________________| |_________rest_________|
```

**The first 80 characters are the most critical** — they show on mobile search results and in the "above the fold" on desktop. Front-load the most important keywords and benefits.

### 9.3 Title Options (Multiple)

The system generates 3-5 title options with different strategies:

```
Option A: Keyword-Optimized (max indexing)
"Maurice's Southern Gold BBQ Sauce – Carolina Mustard Barbecue Sauce, All Natural, No HFCS – Authentic SC Restaurant Recipe – 18oz (Pack of 2)"

Strategy: Front-loads "BBQ Sauce" and "Carolina Mustard" for indexing.
Trade-off: Less emotional, more functional.
Keywords hit: bbq sauce, carolina mustard, barbecue sauce, all natural, no hfcs, restaurant recipe, 18oz, pack of 2

Option B: Heritage-Led (emotional + keyword)
"Maurice's Piggie Park Southern Gold – Authentic SC Mustard BBQ Sauce, Original Restaurant Recipe Since 1953 – All Natural, Bold Tangy Flavor – 18oz (Pack of 2)"

Strategy: Leads with brand heritage and authenticity.
Trade-off: Slightly fewer category keywords, more brand story.
Keywords hit: mustard bbq sauce, restaurant recipe, all natural, tangy flavor, 18oz, pack of 2

Option C: Benefit-Led (conversion-focused)
"Maurice's Southern Gold Mustard BBQ Sauce – Bold Tangy Flavor, All Natural, No Preservatives – Authentic Carolina Barbecue from a Legendary SC Restaurant – 2 Pack (18oz Each)"

Strategy: Leads with taste benefit and clean-label, then heritage.
Trade-off: Stronger for health-conscious and flavor-seeking segments.
Keywords hit: mustard bbq sauce, tangy flavor, all natural, no preservatives, carolina barbecue, restaurant, 2 pack, 18oz
```

Each option includes:
- The full title text
- Character count (total and mobile-visible portion)
- Strategy explanation
- Trade-off analysis
- Keywords indexed
- Which personas it best serves
- Mobile preview (first 80 chars)

### 9.4 Bullet Point Rules & Constraints

| Rule | Constraint |
|------|-----------|
| Number of bullets | 5 allowed |
| Max length per bullet | 500 characters (200-250 recommended) |
| Format | Start with CAPS phrase (key benefit), then supporting detail |
| No HTML | Plain text only (some categories allow basic formatting) |
| No promotional claims | No "Best", "Top-rated", "#1" unless verifiable |
| Keyword indexed | Yes — high weight for search ranking |
| Mobile visibility | Bullets 1-2 visible above fold on mobile; 3-5 require expansion |

### 9.5 Bullet Point Strategy

The 5 bullet slots must serve multiple purposes simultaneously:
1. **Convert** — address decision drivers and overcome objections
2. **Index** — include important keywords naturally
3. **Inform** — provide practical product information
4. **Differentiate** — show why this is better than alternatives

**Bullet Slot Strategy:**

| Slot | Purpose | Targets |
|------|---------|---------|
| Bullet 1 | **Core value prop + primary keyword** | All personas, highest-volume keyword |
| Bullet 2 | **Key differentiator + trust** | Comparison shoppers, purists |
| Bullet 3 | **Problem solver + clean label** | Health-conscious, ingredient-conscious |
| Bullet 4 | **Use cases + versatility** | New-to-category, gift buyers |
| Bullet 5 | **Format/value + practical info** | Price-conscious, convenience seekers |

### 9.6 Bullet Options (10 for 5 Slots)

The system generates **10 bullet options** (2 per slot), letting the AM choose the best combination:

```
SLOT 1 — Core Value Proposition
─────────────────────────────────

Option 1A (Heritage-Led):
AUTHENTIC SOUTH CAROLINA MUSTARD BBQ SAUCE – Maurice's Southern Gold is the
signature sauce from Piggie Park, one of South Carolina's most legendary BBQ
restaurants. This isn't imitation mustard BBQ — it's the original recipe served
in our restaurants for over 70 years. Bold, tangy, and utterly unique.

Option 1B (Flavor-Led):
BOLD TANGY MUSTARD BBQ UNLIKE ANYTHING IN YOUR PANTRY – Forget tomato-based
sauces. Maurice's Southern Gold is a true Carolina mustard BBQ sauce — tangy,
savory, and packed with flavor that's been perfected over 70+ years in our
South Carolina restaurants. One taste and you'll understand why people drive
hours for this sauce.


SLOT 2 — Key Differentiator
────────────────────────────

Option 2A (Heritage-Led):
THE REAL THING, NOT A KNOCKOFF – Unlike mass-market "carolina style" sauces,
Maurice's comes from an actual restaurant chain that's been serving mustard BBQ
since 1953. Same recipe, same facility, same taste that's made Piggie Park a
South Carolina institution. If you've been to SC, you know the name.

Option 2B (Quality-Led):
RESTAURANT-QUALITY SAUCE DELIVERED TO YOUR DOOR – This is the exact same sauce
served at Maurice's Piggie Park restaurants — not a watered-down retail version.
Made in our own facility using our original recipe, every bottle is restaurant-
grade barbecue sauce you'd pay $12 a plate for.


SLOT 3 — Clean Label / Health
──────────────────────────────

Option 3A (Ingredient-Focused):
ALL NATURAL, NO JUNK – Made with real mustard, vinegar, and spices. No high
fructose corn syrup, no artificial preservatives, no artificial colors. Just
honest ingredients that let the flavor shine. Check the label — you'll
recognize every ingredient.

Option 3B (Comparison-Focused):
CLEANER THAN THE BIG BRANDS – While most BBQ sauces load up on HFCS and
artificial additives, Maurice's Southern Gold keeps it simple: mustard, vinegar,
spices. All natural ingredients you can pronounce. Better for you, better
tasting — that's not a coincidence.


SLOT 4 — Use Cases & Versatility
─────────────────────────────────

Option 4A (Use-Case Rich):
ENDLESS WAYS TO USE IT – Perfect on pulled pork (the classic), but that's just
the start. Brush it on grilled chicken, drizzle over smoked ribs, use as a
dipping sauce for fries, mix into coleslaw, or marinate shrimp. One bottle
replaces three sauces in your kitchen.

Option 4B (Occasion-Focused):
YOUR SECRET WEAPON FOR COOKOUTS – Whether you're smoking a pork butt, grilling
chicken thighs, or hosting a backyard BBQ, Southern Gold is the sauce your
guests will ask about. Also makes an incredible gift for the BBQ lover in your
life — they've never tasted anything like it.


SLOT 5 — Format & Value
────────────────────────

Option 5A (Value-Focused):
STOCK UP WITH THE 2-PACK (36oz TOTAL) – Two 18oz bottles so you're set for
grilling season. At restaurant quality, this sauce goes fast — the 2-pack means
you won't run out mid-cookout. Store-bought BBQ sauce can't compete with the
real thing.

Option 5B (Satisfaction-Focused):
2-PACK OF 18oz BOTTLES – 36 ounces of authentic Southern Gold BBQ sauce.
Generously sized so you can sauce liberally without rationing. Nearly 1,000
customers and a 4.7-star rating — once you try it, you'll understand why people
order it by the case.
```

### 9.7 Backend Keyword Strategy

250-byte limit. Rules:
- Do NOT repeat words already in title or bullets
- Use single words or short phrases, separated by spaces
- Include misspellings, synonyms, regional terms
- No commas, semicolons, or ASINs
- No brand name (already in title)

```
Example backend keywords (not exceeding 250 bytes):
"barbeque bar-b-que bbq dipping pulled pork smoker ribs grilling marinade
condiment tangy vinegar southern gold SC regional specialty gourmet artisan
gift basket cookout tailgate summer picnic natural organic gluten free"
```

### 9.8 Human Checkpoint: Copy Review

The AM reviews title and bullet options:
- Which title option best represents the brand?
- Which bullet combinations work best together?
- Are there claims that need verification with the client?
- Does the tone match the brand voice?
- Are we making any promises we can't keep?

---

## 10. Phase 7: Design Brief — Images & A+ Content

### Purpose
Create comprehensive design briefs for product images (B+ images) and A+ Premium Content that visually communicate the most important attributes, threaded across all available slots for maximum conversion impact.

### 10.1 The Visual Conversion Framework

Every image and A+ module must earn its place by serving a strategic purpose:

```
Visual Strategy Hierarchy:
├── STOP the scroll (Main Image → CTR)
├── SHOW the product (Image 2-3 → Understanding)
├── PROVE the claims (Image 4-5 → Trust)
├── CLOSE the sale (Image 6-7 → Conversion)
└── DEEPEN the story (A+ Content → Brand + Education + Comparison)
```

### 10.2 Image Slot Strategy (7 Slots Available)

| Slot | Purpose | Type | Strategic Goal |
|------|---------|------|---------------|
| Main Image | Stop the scroll, earn the click | Product-only (white bg) | CTR — stand out in search results |
| Image 2 | Show what's inside / what it looks like | Product detail / close-up | Understanding — set flavor expectations |
| Image 3 | Show it in use / lifestyle | Lifestyle | Desire — "I want to cook with this" |
| Image 4 | Prove differentiation (infographic) | Infographic | Trust — "This is why it's better" |
| Image 5 | Show versatility / use cases | Infographic or lifestyle | Breadth — "I can use this for many things" |
| Image 6 | Address objections / build trust | Infographic | Overcome — "My concerns are answered" |
| Image 7 | Reinforce brand / close with CTA | Brand story or comparison | Close — "I'm confident buying this" |

### 10.3 Attribute-to-Image Mapping

Each attribute can be visually represented. Some attributes appear in multiple images (threading):

```
TRUST THREADING EXAMPLE:
"Restaurant heritage" appears in:
├── Main Image: Subtle restaurant logo integration on bottle
├── Image 3: Sauce being served in what looks like a restaurant setting
├── Image 5: "Since 1953" badge overlaid on lifestyle shot
├── Image 6: Restaurant storefront photo with "Same recipe" callout
├── A+ Module 1: Full heritage story with historical photos
└── A+ Module 4: "From our kitchen to yours" narrative

This attribute is "threaded" across 6 touchpoints rather than appearing
in a single "heritage slide." The buyer absorbs trust subconsciously.
```

### 10.4 Per-Attribute Visual Ideas

For each P0 and P1 attribute, the system generates multiple visual representation ideas:

```yaml
attribute_visual_brief:
  attribute: "Mustard-based (unique flavor)"
  type: feature + uniqueness
  priority: P0

  visual_ideas:
    - idea: "Color comparison"
      description: "Side-by-side of golden/yellow Maurice's vs red competitor sauces"
      placement: [image_4, a_plus_module_3]
      emotion: "Different, special, stands out"
      text_overlay: "Not Your Typical BBQ Sauce"

    - idea: "Ingredient beauty shot"
      description: "Whole grain mustard seeds, vinegar, spices artfully arranged around bottle"
      placement: [image_2]
      emotion: "Natural, quality, craftsmanship"
      text_overlay: "Real Mustard. Real Flavor."

    - idea: "Taste reaction"
      description: "Person at BBQ trying it for first time, expression of pleasant surprise"
      placement: [image_3, a_plus_module_2]
      emotion: "Discovery, excitement, sharing"
      text_overlay: "The Flavor You Didn't Know You Were Missing"


attribute_visual_brief:
  attribute: "Restaurant heritage (70+ years)"
  type: trust_signal + uniqueness
  priority: P0

  visual_ideas:
    - idea: "Then and now"
      description: "Split image — vintage B&W photo of original restaurant alongside current bottle"
      placement: [a_plus_module_1, image_6]
      emotion: "Authentic, time-tested, trustworthy"
      text_overlay: "Same Recipe Since 1953"

    - idea: "Restaurant to table"
      description: "Left side: restaurant kitchen/serving. Right side: bottle at home BBQ."
      placement: [image_6, a_plus_module_3]
      emotion: "Connection, access, special"
      text_overlay: "Restaurant-Famous BBQ Sauce, Now On Your Table"

    - idea: "Heritage badge"
      description: "Elegant badge/seal design: 'Est. 1953 • Maurice's Piggie Park • SC'"
      placement: [image_4_corner, image_6_corner, a_plus_module_1]
      emotion: "Credibility, tradition, pride"
      text_overlay: None (visual only — subtle trust signal)


attribute_visual_brief:
  attribute: "All-natural, no HFCS"
  type: problem_solver
  priority: P1

  visual_ideas:
    - idea: "Clean label callout"
      description: "Zoomed-in ingredient list with highlights + 'No HFCS' 'No Preservatives' badges"
      placement: [image_5, a_plus_module_4]
      emotion: "Transparency, health, honesty"
      text_overlay: "Nothing Artificial. Ever."

    - idea: "Ingredient lineup"
      description: "Beautiful flat-lay of actual ingredients: mustard, spices, vinegar — 'That's it.'"
      placement: [image_2_secondary, a_plus_module_2]
      emotion: "Simple, wholesome, trustworthy"
      text_overlay: "Just 7 Ingredients"

    - idea: "Label comparison"
      description: "Our ingredient list (short, clean) vs blurred 'typical BBQ sauce' ingredient list (long, chemicals)"
      placement: [image_5, a_plus_module_4]
      emotion: "Confidence, superiority, health"
      text_overlay: "Read the Label. You'll Feel Good About This One."
```

### 10.5 A+ Premium Content Module Strategy

Amazon Premium A+ Content allows up to 7 modules. Each module type serves a different strategic purpose:

| Module # | Module Type | Strategic Purpose | Attribute(s) Served |
|----------|------------|-------------------|-------------------|
| 1 | **Hero Image + Brand Story** | Emotional hook — set the stage | Heritage, authenticity, brand |
| 2 | **4-Image Grid with Text** | Show product versatility & use cases | Use cases, flavor, experience |
| 3 | **Feature Highlights** | Communicate key differentiators | Mustard-based, natural, SC origin |
| 4 | **Comparison Chart** | Win the comparison shopper | All attributes vs competitors |
| 5 | **Lifestyle Banner** | Show the experience of using it | Social occasions, grilling, family |
| 6 | **FAQ / Objection Handling** | Address pre-purchase concerns | Price, taste expectations, shipping |
| 7 | **Cross-Sell / Bundle** | Increase AOV, show full range | Other flavors, variety packs |

### 10.6 A+ Module Briefs

Each module gets a detailed design brief:

```yaml
a_plus_module_brief:
  module_number: 1
  module_type: "Hero Image + Brand Story"

  strategic_purpose: |
    This is the first thing the buyer sees when scrolling to A+ content.
    It must immediately communicate: "This is not just another BBQ sauce.
    This is something with history, soul, and authenticity."

  key_message: "Authentic SC BBQ Sauce from a Legendary Restaurant Since 1953"

  visual_direction:
    style: "Warm, rustic, inviting — think food photography meets heritage brand"
    mood: "Nostalgic but premium. Not cheap/kitschy, not cold/corporate."
    background: "Dark wood or rustic table, warm lighting"
    hero_element: "Bottle prominently placed with food in background"
    must_include:
      - Bottle (hero product shot)
      - Food in background (pulled pork platter or ribs)
      - Heritage visual element (vintage restaurant reference)
      - "Since 1953" or "70+ Year Recipe"
    must_avoid:
      - Stock photo feel
      - Overly modern/minimalist aesthetic
      - Generic BBQ imagery without brand identity
      - Cluttered text overlays

  text_content:
    headline: "The Sauce That Built a Southern Legend"
    body: |
      For over 70 years, Maurice's Piggie Park has been serving South Carolina's
      most famous mustard BBQ sauce. Now, the same recipe that made our restaurants
      legendary is in your hands. Bold, tangy, and utterly authentic — this is
      Carolina Gold at its finest.

  mobile_considerations: |
    Text must be legible on mobile. Headline should be 6 words or fewer.
    Image composition should work in both landscape (desktop) and
    portrait-cropped (mobile) formats.

  creative_options:
    - option_a: "Vintage approach — sepia-toned restaurant photo background with modern bottle overlay"
    - option_b: "Food-forward — beautiful platter of sauced pulled pork with bottle and restaurant story as text"
    - option_c: "Map approach — map of South Carolina with restaurant location marked, sauce bottle, heritage timeline"
```

### 10.7 Image Design Briefs

Each image slot gets a design brief:

```yaml
image_design_brief:
  slot: "Main Image"

  amazon_requirements:
    - White background (RGB 255, 255, 255)
    - Product fills 85%+ of frame
    - No text overlays
    - No logos or badges (unless on product packaging)
    - Minimum 1000x1000 pixels (1600x1600+ recommended)
    - JPEG or PNG format

  strategic_goal: |
    Win the click in search results. This image competes with 15-20 other
    products on the search results page. It must:
    1. Be immediately recognizable as BBQ sauce
    2. Stand out from red/brown tomato-based competitors (our yellow/gold is an advantage)
    3. Show premium quality (lighting, clarity, label readability)
    4. Show both bottles (2-pack = value signal)

  visual_direction:
    composition: "Two bottles, slight angle, hero lighting from upper left"
    background: "Pure white, professional studio shot"
    focus: "Label must be clearly readable"
    color_advantage: "Yellow/gold sauce POPS against competitors' red — lean into this"
    texture: "If possible, show sauce texture (drip, pour, or on cap)"

  creative_options:
    - option_a: "Classic studio — two bottles upright, hero lighting, clean and premium"
    - option_b: "Angled duo — two bottles at slight angle to each other, dynamic composition"
    - option_c: "3D render — hyper-clean, perfect lighting, maximizes label readability"

  differentiation_notes: |
    Most BBQ sauce main images are red/brown. Our gold/yellow sauce is a natural
    differentiator in search results. The main image should maximize this color
    contrast. Do NOT mute the gold color — let it be vibrant and eye-catching.
```

### 10.8 Trust Threading Across All Visual Elements

Trust isn't built in a single image. It's **threaded** across the entire visual experience:

```
TRUST THREAD MAP:

Signal: "This is authentic/real"
├── Main Image: Professional bottle shot (premium feel)
├── Image 3: Sauce on real food (it's actually used)
├── Image 6: Restaurant photo + "Same recipe" (institutional backing)
├── A+ Module 1: Heritage story (70+ years)
├── A+ Module 4: Comparison chart (stands up to scrutiny)
└── A+ Module 5: Lifestyle — real people, real BBQ (relatable)

Signal: "This is safe to buy"
├── Image 5: Ingredient callouts (nothing to hide)
├── Image 6: "989 reviews • 4.7★" badge (social proof)
├── A+ Module 4: Comparison chart (transparently better)
├── A+ Module 6: FAQ addressing concerns (we hear you)
└── Bullet 5: "Nearly 1,000 5-star reviews" (validation)

Signal: "This is worth the price"
├── Image 4: "Restaurant-grade" infographic (premium = justified)
├── Image 7: "2-Pack = $0.61/oz" value callout
├── A+ Module 1: Heritage story (you're paying for authenticity)
├── A+ Module 3: "All natural, no fillers" (quality ingredients cost more)
└── Bullet 2: "Same sauce served in restaurants" (restaurant markup = savings)
```

### 10.9 Human Checkpoint: Design Brief Review

The AM reviews the complete design brief:
- Does the visual strategy match the brand's aesthetic?
- Are the creative directions achievable? (does the client have these assets?)
- Is the trust threading strategy effective?
- Which creative options should we pursue for each slot?
- Does the client have any visual requirements or restrictions?
- What assets do we need from the client? (photos, logos, certifications)

---

## 11. Domain Model: Entities & Relationships

### 11.1 Entity Map

```
┌──────────────────────────────────────────────────────────────────────┐
│                    LISTING OPTIMIZATION SYSTEM                       │
│                                                                      │
│  ┌──────────────┐    ┌──────────────────┐    ┌───────────────────┐  │
│  │  InputBundle  │───►│  EvidenceCorpus   │───►│  ProductKnowledge │  │
│  │              │    │                  │    │  Base (PKB)       │  │
│  └──────────────┘    └────────┬─────────┘    └─────────┬─────────┘  │
│                               │                        │             │
│                               │    ┌───────────────────┤             │
│                               │    │                   │             │
│                               ▼    ▼                   ▼             │
│                      ┌──────────────────┐    ┌───────────────────┐  │
│                      │  BuyerPersona     │    │  ProductAttribute  │  │
│                      │  (multiple)       │◄──►│  (master list)    │  │
│                      └────────┬─────────┘    └─────────┬─────────┘  │
│                               │                        │             │
│                               │    ┌───────────────────┤             │
│                               │    │                   │             │
│                               ▼    ▼                   ▼             │
│                      ┌──────────────────┐    ┌───────────────────┐  │
│                      │  KeywordEntry     │    │  ListingCopy      │  │
│                      │  (categorized)    │───►│  (title + bullets) │  │
│                      └────────┬─────────┘    └───────────────────┘  │
│                               │                        │             │
│                               ▼                        ▼             │
│                      ┌──────────────────┐    ┌───────────────────┐  │
│                      │  DesignBrief      │◄──┤  AttributeVisual   │  │
│                      │  (images + A+)    │    │  Brief             │  │
│                      └──────────────────┘    └───────────────────┘  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

### 11.2 Entity Definitions

#### InputBundle
The collection of all inputs provided for a listing optimization engagement.

```yaml
InputBundle:
  id: uuid
  product_asin: string
  brand_id: reference → Brand
  created_at: datetime
  created_by: string                    # AM who initiated

  inputs:
    existing_listing: ExistingListingInput | null
    d2c_website: D2CWebsiteInput | null
    social_media: SocialMediaInput[] | null
    user_text: UserTextInput | null
    competitors: CompetitorInput[] | null
    brand_context: BrandContextInput | null

  status: draft | researching | in_review | approved | generating | complete
```

#### EvidenceCorpus
Structured collection of all research findings.

```yaml
EvidenceCorpus:
  id: uuid
  input_bundle_id: reference → InputBundle

  product_facts: EvidenceItem[]
  cultural_context: EvidenceItem[]
  competitor_profiles: CompetitorProfile[]
  review_themes_own: ReviewTheme[]
  review_themes_competitor: ReviewTheme[]
  search_terms_discovered: SearchTerm[]
  category_norms: CategoryNorm[]
  brand_ecosystem: BrandEcosystemFact[]
  external_evidence: ExternalEvidence[]    # Reddit, blogs, media

  completeness_score: number               # 0-100, how complete is the research
  gaps_identified: string[]                # What we couldn't find

  human_validated: boolean
  validation_notes: string
```

#### EvidenceItem
A single piece of evidence with provenance.

```yaml
EvidenceItem:
  id: uuid
  claim: string                            # The fact or observation
  source_type: review | competitor | d2c | social | user_input | web_research | amazon_data
  source_url: string | null
  source_description: string
  confidence: high | medium | low
  relevance: high | medium | low
  extracted_at: datetime
  verbatim_quote: string | null           # Exact words if from a human source
```

#### ProductKnowledgeBase
Deep structured understanding of the product.

```yaml
ProductKnowledgeBase:
  id: uuid
  evidence_corpus_id: reference → EvidenceCorpus
  product_asin: string

  # Essence
  category_definition: string
  functional_identity: string
  emotional_identity: string
  cultural_identity: string
  origin_identity: string

  # Uniqueness
  uniqueness_dimensions: UniquenessDimension[]

  # Value Propositions
  core_value_proposition: string
  primary_value_props: ValueProposition[]
  secondary_value_props: ValueProposition[]
  tertiary_value_props: ValueProposition[]

  # Context
  competitive_position: premium | value | artisan | mass_market | niche
  category_education_needed: boolean
  education_message: string | null
  premium_justification: string | null
  known_objections: Objection[]

  human_validated: boolean
  validation_notes: string
```

#### UniquenessDimension
```yaml
UniquenessDimension:
  dimension: recipe | process | origin | experience | category | value
  claim: string
  evidence: reference → EvidenceItem[]
  rarity: high | medium | low
  defensibility: high | medium | low
  buyer_perception: string
```

#### BuyerPersona
```yaml
BuyerPersona:
  id: uuid
  pkb_id: reference → ProductKnowledgeBase

  name: string
  description: string
  estimated_traffic_share: percentage

  demographics:
    age_range: string
    gender_skew: string
    location_skew: string
    income_level: string
    lifestyle: string

  search_behavior:
    primary_keywords: string[]
    search_intent: string
    intent_type: branded | category | problem_solving | comparison | impulse
    research_depth: quick_decider | moderate | deep_researcher
    funnel_stage: awareness | consideration | decision | purchase

  decision_drivers: DecisionDriver[]
  objections: PersonaObjection[]
  unstated_needs: UnstatedNeed[]
  use_cases: UseCase[]

  listing_element_importance:
    title: 1-5
    main_image: 1-5
    bullets_1_2: 1-5
    bullets_3_5: 1-5
    secondary_images: 1-5
    a_plus_content: 1-5
    reviews_section: 1-5
    price: 1-5

  human_validated: boolean
```

#### ProductAttribute
```yaml
ProductAttribute:
  id: uuid
  pkb_id: reference → ProductKnowledgeBase

  attribute: string
  description: string

  # Classification
  type: feature | uniqueness | problem_solver | trust_signal | experience | social_proof | convenience | value
  priority: P0 | P1 | P2 | P3

  # Evidence
  evidence_items: reference → EvidenceItem[]
  evidence_strength: strong | moderate | weak
  supporting_quotes: string[]

  # Persona Mapping
  persona_relevance: PersonaRelevance[]

  # Placement
  recommended_placements: PlacementRecommendation[]

  # Competitive
  competitive_position:
    our_position: string
    competitors: CompetitorPosition[]
    we_win: boolean

  human_validated: boolean
```

#### KeywordEntry
```yaml
KeywordEntry:
  id: uuid
  product_asin: string

  keyword: string
  type: master | sub_category | long_tail | branded | competitor_branded | problem | question | seasonal | cross_sell
  priority: P0 | P1 | P2 | P3

  estimated_volume: high | medium | low
  competition: high | medium | low
  conversion_likelihood: high | medium | low

  search_intent: string
  intent_type: branded | category | problem_solving | comparison | impulse
  funnel_stage: awareness | consideration | decision | purchase
  primary_persona: reference → BuyerPersona

  placement: KeywordPlacement
  ad_strategy: KeywordAdStrategy | null

  source: discovered | user_provided | competitor_reverse_engineered | auto_suggest
```

#### ListingCopyOption
```yaml
ListingCopyOption:
  id: uuid
  product_asin: string

  # Title
  title_options: TitleOption[]
  selected_title: reference → TitleOption | null

  # Bullets
  bullet_options_by_slot:
    slot_1: BulletOption[]     # 2-3 options per slot
    slot_2: BulletOption[]
    slot_3: BulletOption[]
    slot_4: BulletOption[]
    slot_5: BulletOption[]
  selected_bullets: reference → BulletOption[5] | null

  # Backend
  backend_keywords: string     # 250-byte string

  human_validated: boolean
```

#### TitleOption
```yaml
TitleOption:
  id: uuid
  title_text: string
  character_count: number
  mobile_visible_text: string          # First ~80 chars
  strategy: string                     # e.g., "keyword-optimized", "heritage-led"
  trade_off: string
  keywords_indexed: string[]
  personas_served: reference → BuyerPersona[]
```

#### BulletOption
```yaml
BulletOption:
  id: uuid
  slot_number: 1 | 2 | 3 | 4 | 5
  bullet_text: string
  character_count: number
  strategy: string
  attributes_covered: reference → ProductAttribute[]
  keywords_included: string[]
  personas_targeted: reference → BuyerPersona[]
```

#### AttributeVisualBrief
```yaml
AttributeVisualBrief:
  id: uuid
  attribute_id: reference → ProductAttribute

  visual_ideas: VisualIdea[]
```

#### VisualIdea
```yaml
VisualIdea:
  id: uuid
  name: string
  description: string
  placement: string[]                  # Which image slots or A+ modules
  emotion_evoked: string
  text_overlay: string | null
  creative_direction: string
  reference_images: string[]           # URLs or descriptions of reference images
```

#### DesignBrief
```yaml
DesignBrief:
  id: uuid
  product_asin: string
  type: product_images | a_plus_content | both

  # Image Strategy
  image_slot_briefs: ImageSlotBrief[7]

  # A+ Strategy
  a_plus_module_briefs: APlusModuleBrief[7]

  # Trust Threading
  trust_threads: TrustThread[]

  # Overall Visual Direction
  brand_aesthetic: string
  mood: string
  color_palette: string[]
  typography_style: string
  photography_style: string

  # Assets Needed
  assets_from_client: string[]
  assets_to_create: string[]

  human_validated: boolean
```

#### TrustThread
```yaml
TrustThread:
  signal: string                       # What trust signal
  touchpoints:
    - element: string                  # image_1, a_plus_module_3, etc.
      how: string                      # How it appears in this element
      subtlety: overt | integrated | subtle
```

### 11.3 Entity Relationships

```
InputBundle ──1:1──► EvidenceCorpus ──1:1──► ProductKnowledgeBase
                         │                         │
                         │                    ┌────┴────┐
                         │                    │         │
                         ▼                    ▼         ▼
                    EvidenceItem[]      BuyerPersona[]  UniquenessDimension[]
                         ▲                    │
                         │              ┌─────┴─────┐
                         │              ▼           │
                    ProductAttribute ◄──┘           │
                         │                          │
                    ┌────┴────┐                     │
                    │         │                     │
                    ▼         ▼                     ▼
              KeywordEntry  AttributeVisualBrief  ListingCopyOption
                    │              │                    │
                    │              ▼                    │
                    │         VisualIdea[]              │
                    │              │                    │
                    └──────┐  ┌───┘                    │
                           ▼  ▼                        │
                        DesignBrief ◄──────────────────┘
```

**Key relationship rules:**
- One InputBundle produces one EvidenceCorpus
- One EvidenceCorpus produces one ProductKnowledgeBase
- One PKB produces multiple BuyerPersonas
- One PKB produces multiple ProductAttributes
- ProductAttributes are mapped to BuyerPersonas (many-to-many)
- ProductAttributes produce both KeywordEntries and AttributeVisualBriefs
- KeywordEntries + ProductAttributes produce ListingCopyOptions
- AttributeVisualBriefs + all upstream entities produce the DesignBrief

---

## 12. System Architecture

### 12.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        PRESENTATION LAYER                            │
│                                                                      │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────────────────┐  │
│  │ Chat (CLI)   │  │ Web UI       │  │ API                        │  │
│  │ (current)    │  │ (future)     │  │ (future)                   │  │
│  └──────┬───────┘  └──────┬───────┘  └──────┬─────────────────────┘  │
│         └─────────────────┼─────────────────┘                        │
│                           ▼                                          │
├─────────────────────────────────────────────────────────────────────┤
│                     ORCHESTRATION LAYER                               │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │                   Pipeline Orchestrator                        │   │
│  │                                                                │   │
│  │  Manages the 7-phase pipeline                                  │   │
│  │  Tracks state: which phase, what's validated, what's pending   │   │
│  │  Handles human-in-the-loop checkpoints                         │   │
│  │  Manages rollback (re-run phase if inputs change)              │   │
│  │                                                                │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                           │                                          │
├─────────────────────────────────────────────────────────────────────┤
│                       PHASE ENGINES                                  │
│                                                                      │
│  ┌────────────┐ ┌─────────┐ ┌─────────┐ ┌──────────┐ ┌─────────┐  │
│  │ Research    │ │ PKB     │ │ Persona │ │ Attribute│ │ Keyword │  │
│  │ Engine     │ │ Engine  │ │ Engine  │ │ Engine   │ │ Engine  │  │
│  └────────────┘ └─────────┘ └─────────┘ └──────────┘ └─────────┘  │
│  ┌────────────┐ ┌─────────┐                                        │
│  │ Copy       │ │ Design  │                                        │
│  │ Engine     │ │ Brief   │                                        │
│  │            │ │ Engine  │                                        │
│  └────────────┘ └─────────┘                                        │
│                                                                      │
├─────────────────────────────────────────────────────────────────────┤
│                       DATA LAYER                                     │
│                                                                      │
│  ┌────────────┐ ┌─────────────┐ ┌─────────────┐ ┌──────────────┐  │
│  │ Evidence   │ │ Entity      │ │ File System │ │ Analytics    │  │
│  │ Store      │ │ Store       │ │ (markdown)  │ │ (MCP)        │  │
│  │ (reviews,  │ │ (PKB,       │ │ (current)   │ │              │  │
│  │  research) │ │  personas)  │ │             │ │              │  │
│  └────────────┘ └─────────────┘ └─────────────┘ └──────────────┘  │
│                                                                      │
├─────────────────────────────────────────────────────────────────────┤
│                     EXTERNAL SERVICES                                │
│                                                                      │
│  ┌────────────┐ ┌─────────────┐ ┌─────────────┐ ┌──────────────┐  │
│  │ Amazon     │ │ Web         │ │ Reddit      │ │ Social       │  │
│  │ (listings, │ │ Scraping    │ │ API         │ │ Media APIs   │  │
│  │  reviews)  │ │ (D2C, blogs)│ │             │ │              │  │
│  └────────────┘ └─────────────┘ └─────────────┘ └──────────────┘  │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### 12.2 Pipeline State Machine

```
                    ┌──────────┐
                    │  START   │
                    └────┬─────┘
                         │ collect inputs
                         ▼
                    ┌──────────┐     ✋ checkpoint
         ┌────────►│ Phase 1   │────────────────────┐
         │         │ Research   │                     │ AM adds inputs
         │         └────┬──────┘◄────────────────────┘
         │              │ evidence corpus ready
         │              ▼
         │         ┌──────────┐     ✋ checkpoint
         │    ┌───►│ Phase 2   │────────────────────┐
         │    │    │ PKB       │                     │ AM corrects
         │    │    └────┬──────┘◄────────────────────┘
         │    │         │
         │    │         ▼
         │    │    ┌──────────┐     ✋ checkpoint
         │    │    │ Phase 3   │────────────────────┐
         │    │    │ Personas  │                     │ AM adjusts
         │    │    └────┬──────┘◄────────────────────┘
         │    │         │
         │    │         ▼
         │    │    ┌──────────┐     ✋ checkpoint
         │    │    │ Phase 4   │────────────────────┐
         │    │    │ Attributes│                     │ AM re-prioritizes
         │    │    └────┬──────┘◄────────────────────┘
         │    │         │
         │    │         ▼
  new    │    │    ┌──────────┐
  inputs │    │    │ Phase 5   │ (keyword strategy — no checkpoint, feeds Phase 6)
         │    │    │ Keywords  │
         │    │    └────┬──────┘
         │    │         │
         │    │         ▼
         │    │    ┌──────────┐     ✋ checkpoint
         │    │    │ Phase 6   │────────────────────┐
         │    │    │ Copy      │                     │ AM picks options
         │    │    └────┬──────┘◄────────────────────┘
         │    │         │
         │    │         ▼
         │    │    ┌──────────┐     ✋ checkpoint
         │    └────│ Phase 7   │────────────────────┐
         │         │ Design    │                     │ AM selects directions
         └─────────│ Brief     │◄────────────────────┘
                   └────┬──────┘
                        │
                        ▼
                   ┌──────────┐
                   │ COMPLETE  │
                   └──────────┘
```

**Key architectural principles:**
1. **Each phase is idempotent** — re-running with same inputs produces same outputs
2. **Upstream changes cascade** — if AM changes PKB, Phases 3-7 re-run
3. **Checkpoints are blocking** — pipeline doesn't advance without human validation
4. **Evidence is immutable** — new research creates new evidence items, never edits old ones
5. **Everything is versioned** — each phase output is versioned, enabling rollback

### 12.3 Current Implementation (File-Based)

In the current markdown + git system, the pipeline maps to files:

```
brands/{brand}/listing-optimization/{asin}/
├── 00-inputs.md                    # InputBundle
├── 01-evidence-corpus.md           # EvidenceCorpus
├── 02-product-knowledge-base.md    # ProductKnowledgeBase
├── 03-buyer-personas.md            # BuyerPersona[]
├── 04-product-attributes.md        # ProductAttribute[]
├── 05-keyword-strategy.md          # KeywordEntry[]
├── 06-listing-copy.md              # ListingCopyOption
├── 07-design-brief-images.md       # DesignBrief (images)
├── 07-design-brief-aplus.md        # DesignBrief (A+)
└── STATUS.md                       # Pipeline state, checkpoints, version history
```

### 12.4 Future Implementation (Database + UI)

```
Database Schema (simplified):
├── listing_optimization_projects    # One per ASIN optimization
├── input_bundles                    # Inputs for a project
├── evidence_items                   # Individual evidence pieces
├── evidence_corpuses                # Aggregated evidence
├── product_knowledge_bases          # PKB entities
├── uniqueness_dimensions            # PKB sub-entity
├── value_propositions               # PKB sub-entity
├── buyer_personas                   # Persona entities
├── decision_drivers                 # Persona sub-entity
├── product_attributes               # Attribute master list
├── attribute_persona_mappings       # Many-to-many
├── keyword_entries                   # Keyword master list
├── listing_copy_options             # Title + bullet options
├── design_briefs                    # Image + A+ briefs
├── visual_ideas                     # Per-attribute visual ideas
├── trust_threads                    # Cross-element trust mapping
├── pipeline_checkpoints             # Human validation records
└── pipeline_versions                # Version history per phase

UI Screens:
├── Project Dashboard                # Overview of all listing optimization projects
├── Input Collection                 # Wizard for collecting inputs
├── Research Review                  # Review evidence corpus, add/edit
├── PKB Editor                       # Edit product knowledge base
├── Persona Cards                    # Visual persona cards with evidence
├── Attribute Matrix                 # Sortable, filterable attribute table
├── Keyword Planner                  # Keyword list with volume, placement
├── Copy Workshop                    # Side-by-side title/bullet options
├── Design Brief Builder             # Visual brief with mood boards
└── Export / Handoff                 # Generate briefs for designers, copywriters
```

---

## 13. Human-in-the-Loop Checkpoints

### 13.1 Checkpoint Design

Each checkpoint follows the same pattern:

```
1. PRESENT — Show the AI's output for this phase
2. VALIDATE — AM confirms accuracy ("Is this correct?")
3. ADJUST — AM makes corrections or additions
4. APPROVE — AM explicitly approves to proceed
5. LOG — Record the validation (who, when, what changed)
```

### 13.2 Checkpoint Details

| Checkpoint | After Phase | What AM Reviews | Common Adjustments |
|-----------|-------------|----------------|-------------------|
| Research Review | Phase 1 | Evidence corpus completeness | Add competitors, correct facts, add insider knowledge |
| PKB Validation | Phase 2 | Product essence, uniqueness, value props | Re-prioritize value props, correct cultural context |
| Persona Validation | Phase 3 | Personas, traffic shares, decision drivers | Add/remove personas, adjust importance ratings |
| Attribute Review | Phase 4 | Attribute list, priorities, persona mapping | Re-prioritize attributes, add missing ones |
| Copy Selection | Phase 6 | Title options, bullet options | Pick winning options, request revisions |
| Design Approval | Phase 7 | Image briefs, A+ module briefs | Select creative directions, add brand constraints |

### 13.3 Escalation Paths

Sometimes the AM can't validate alone:

| Situation | Escalation |
|-----------|-----------|
| Product claims need verification | → Client check (send PKB to client for review) |
| Competitive data seems wrong | → Manual verification (check Amazon listing) |
| Brand voice uncertainty | → Client brand guidelines review |
| Legal/compliance concern | → Agency legal review |
| Photography/asset availability | → Client creative team |

---

## 14. Example Flow: Maurice's Piggie Park BBQ Sauce

### The Setup

**AM:** "I need to optimize the listing for Maurice's Original 2-Pack BBQ Sauce (B07P15CB6X)."

**Inputs provided:**
- ASIN: B07P15CB6X (existing listing)
- D2C: https://www.piggiepark.com/
- Competitors: B01DOIMC1E (Lillie's Q), B004YVWO9Q (Cattlemen's)
- User text: "Maurice's is a legendary SC BBQ restaurant chain since the 1950s. Their mustard-based sauce is what SC BBQ is known for. Most people outside the south don't know mustard BBQ even exists. The recipe has never changed."

---

### Phase 1 Output: Evidence Corpus (Abbreviated)

```
PRODUCT FACTS:
• Mustard-based BBQ sauce (South Carolina regional style)
• 2-pack format, 36oz total (2 × 18oz)
• $22-24 price point ($0.61/oz)
• 4.7 stars, 989 reviews, Amazon's Choice badge
• All-natural, no HFCS, no preservatives
• From Maurice's Piggie Park restaurant chain (est. 1953)
• Same recipe served in restaurants for 70+ years

CULTURAL CONTEXT:
• SC mustard BBQ is one of four regional BBQ sauce styles in the US
  (KC tomato, TX vinegar-pepper, NC vinegar, SC mustard)
• Most Americans default to tomato-based BBQ = education opportunity
• Reddit r/BBQ: "Mustard BBQ is the most underrated sauce style" (frequent theme)
• Food blogs consistently list Maurice's as a top mustard BBQ sauce
• Southern food tourism trend = growing interest in regional authenticity

COMPETITOR INSIGHTS:
• Cattlemen's: Mass-market, 1/3 our price, "good enough" for casual users
  - Reviews praise: price, availability, "decent flavor"
  - Reviews complain: "generic", "not authentic", "watery"
• Lillie's Q: Artisan positioning, Chicago-based (not SC)
  - Reviews praise: "complex flavor", "great brand"
  - Reviews complain: "expensive for size", "too sweet"

OUR REVIEW THEMES:
• Positive: "Best mustard BBQ", "just like the restaurant", "perfect on pulled pork"
• Negative: "Too tangy for some", "expected more heat", "price seems high"
• Customers say: "Worth every penny" and "I order this every few months"

SEARCH BEHAVIOR:
• "bbq sauce" — 100K+ searches/mo (master keyword)
• "mustard bbq sauce" — 5-10K searches/mo
• "carolina bbq sauce" — 3-5K searches/mo
• "maurice's bbq sauce" — 1-2K searches/mo (branded)
• "south carolina bbq sauce" — 1-2K searches/mo
• "yellow bbq sauce" — 500-1K (emerging term)
```

**Checkpoint: AM adds** "Bessinger's is actually the main SC competitor — same region, similar heritage claim. Also, the sauce is sold in Walmart in SC, so there's offline brand recognition."

---

### Phase 2 Output: Product Knowledge Base (Abbreviated)

```
PRODUCT ESSENCE:
Category: Mustard-based BBQ sauce (South Carolina regional style)
Functional: Flavors and enhances grilled/smoked meats with a unique tangy profile
Emotional: A taste of South Carolina BBQ heritage — authenticity you can't fake
Cultural: Represents one of America's four great BBQ traditions (the most underrated one)
Origin: Same recipe served at Maurice's Piggie Park restaurants since 1953

CORE VALUE PROPOSITION:
"The authentic South Carolina mustard BBQ sauce from a legendary 70-year restaurant chain"

UNIQUENESS DIMENSIONS:
1. Recipe: Mustard-based (only ~5% of BBQ sauces) — HIGH rarity, HIGH defensibility
2. Origin: From an actual legendary restaurant chain — HIGH rarity, HIGH defensibility
3. Process: Same recipe, same facility as restaurants — MEDIUM rarity, HIGH defensibility
4. Experience: Tangy, bold, unlike any tomato-based sauce — HIGH rarity, LOW defensibility
5. Category: Defining product in an underrepresented sub-category — HIGH rarity, HIGH defensibility

EDUCATION NEEDED: Yes — many buyers don't know mustard BBQ exists as a style.
Must teach: "This is not a regular BBQ sauce. It's a completely different tradition."
```

**Checkpoint: AM confirms, adds** "Client loves the 'legendary restaurant' angle — lean into it hard."

---

### Phase 3 Output: Buyer Personas (Abbreviated)

```
PERSONA 1: "The BBQ Purist" (35% of traffic)
Searches: "mustard bbq sauce", "carolina bbq sauce", "best bbq sauce for pulled pork"
Intent: Category search — knows what mustard BBQ is, looking for the best one
Decision drivers: Flavor authenticity (P0), ingredient quality (P1), reviews (P1)
Objection: "Is this actually authentic or just marketing?"
Unstated need: Wants to see real food photography, real BBQ usage

PERSONA 2: "The SC Nostalgic" (25% of traffic)
Searches: "maurice's bbq sauce", "piggie park sauce", "south carolina bbq"
Intent: Branded/regional — has eaten at the restaurant or knows the brand
Decision drivers: "Is this the real thing?" (P0), heritage/story (P0), taste accuracy (P1)
Objection: None (high intent) — but price sensitivity on repeat orders
Unstated need: Wants to see the restaurant connection (photos, heritage)

PERSONA 3: "The Curious Foodie" (20% of traffic)
Searches: "bbq sauce", "unique bbq sauce", "gourmet bbq sauce"
Intent: Category/exploration — browsing, doesn't know mustard BBQ
Decision drivers: Uniqueness (P0), social proof (P0), taste description (P1)
Objection: "I've never tried mustard BBQ — will I like it?" + "Why so expensive?"
Unstated need: Needs education on what mustard BBQ is and reassurance via reviews

PERSONA 4: "The Gift Buyer" (10% of traffic)
Searches: "bbq gift", "bbq sauce gift set", "southern food gift"
Intent: Problem-solving — looking for a gift, not for personal use
Decision drivers: Presentation/giftability (P0), uniqueness (P0), brand story (P1)
Objection: "Will the recipient like it?"
Unstated need: Wants to see gift-worthy packaging and a compelling story to share

PERSONA 5: "The Health-Conscious Cook" (10% of traffic)
Searches: "natural bbq sauce", "bbq sauce no hfcs", "healthy bbq sauce"
Intent: Problem-solving with qualifier — wants BBQ sauce that meets dietary criteria
Decision drivers: Ingredients (P0), "no [X]" claims (P0), taste (P1)
Objection: "Natural usually means bad taste"
Unstated need: Wants ingredient transparency and flavor reassurance
```

**Checkpoint: AM confirms** "These are spot-on. The SC Nostalgic is probably higher — maybe 30%. And yes, the Curious Foodie needs education."

---

### Phase 4 Output: Product Attributes (Abbreviated Top 10)

```
ID | Attribute                    | Type         | Pri | Purist | Nostalg | Foodie | Gift | Health | Placement
──────────────────────────────────────────────────────────────────────────────────────────────────────────────
A1 | Mustard-based recipe         | feature      | P0  | pri    | sec     | pri    | -    | -      | Title, B1, Img4, A+3
A2 | 70+ year restaurant heritage | uniqueness   | P0  | sec    | pri     | sec    | sec  | -      | Title, B2, Img6, A+1
A3 | Same recipe as restaurants   | trust        | P0  | pri    | pri     | -      | -    | -      | B2, Img6, A+1
A4 | All-natural, no HFCS         | problem      | P1  | -      | -       | -      | -    | pri    | Title, B3, Img5, A+4
A5 | Bold tangy flavor            | experience   | P1  | pri    | pri     | sec    | -    | -      | B1, Img3, A+2
A6 | 2-pack format (36oz)         | convenience  | P1  | -      | sec     | -      | pri  | -      | Title, B5, -
A7 | 4.7★ with 989 reviews        | social_proof | P1  | sec    | -       | pri    | pri  | sec    | B5, A+4
A8 | Versatile (pork, chicken...)  | feature      | P2  | sec    | -       | sec    | -    | -      | B4, Img5, A+2
A9 | SC BBQ tradition             | uniqueness   | P2  | sec    | pri     | sec    | sec  | -      | A+1, A+5, Backend
A10| Makes a great gift           | value        | P2  | -      | -       | -      | pri  | -      | B5, Img7, A+7
```

**Checkpoint: AM adjusts** "Move A4 (all-natural) to P0 — the client insists on this being front and center."

---

### Phase 6 Output: Selected Title & Bullets

**Selected Title (Option B, modified per AM feedback):**
```
Maurice's Piggie Park Southern Gold – Authentic SC Mustard BBQ Sauce,
All Natural, No HFCS – Original Restaurant Recipe Since 1953 – 18oz (Pack of 2)
```
*148 characters. Mobile preview: "Maurice's Piggie Park Southern Gold – Authentic SC Mustard BBQ Sauce, All Nat..."*

**Selected Bullets:**
```
Bullet 1 (1B — Flavor-Led):
BOLD TANGY MUSTARD BBQ UNLIKE ANYTHING IN YOUR PANTRY – Forget tomato-based
sauces. Maurice's Southern Gold is a true Carolina mustard BBQ sauce — tangy,
savory, and packed with flavor that's been perfected over 70+ years in our
South Carolina restaurants. One taste and you'll understand.

Bullet 2 (2A — Heritage-Led):
THE REAL THING, NOT A KNOCKOFF – Unlike mass-market "carolina style" sauces,
Maurice's comes from an actual restaurant chain that's been serving mustard BBQ
since 1953. Same recipe, same facility, same taste that's made Piggie Park
a South Carolina institution.

Bullet 3 (3A — Ingredient-Focused):
ALL NATURAL, NO JUNK – Made with real mustard, vinegar, and spices. No high
fructose corn syrup, no artificial preservatives, no artificial colors. Just
honest ingredients that let the flavor shine.

Bullet 4 (4A — Use-Case Rich):
ENDLESS WAYS TO USE IT – Perfect on pulled pork (the classic), but that's just
the start. Brush it on grilled chicken, drizzle over smoked ribs, use as a
dipping sauce, mix into coleslaw, or marinate shrimp.

Bullet 5 (5B — Satisfaction-Focused):
2-PACK OF 18oz BOTTLES (36oz TOTAL) – Generously sized so you can sauce
liberally without rationing. Nearly 1,000 customers and a 4.7-star rating —
once you try it, you'll understand why people order it by the case.
```

---

### Phase 7 Output: Design Brief Summary

```
IMAGE STRATEGY:
Slot 1 (Main): Two bottles, gold sauce prominent, professional studio, white bg
Slot 2: Sauce close-up — golden color, texture visible, "not red" contrast
Slot 3: Lifestyle — sauce being brushed on pulled pork at backyard BBQ
Slot 4: Infographic — "What Makes Southern Gold Different" (mustard-based, heritage, natural)
Slot 5: Infographic — Use cases grid (6 foods with sauce)
Slot 6: Heritage story — restaurant photo + "Same Recipe Since 1953"
Slot 7: 2-pack value + review highlights

A+ STRATEGY (7 Modules):
Module 1: Hero banner — "The Sauce That Built a Southern Legend" + restaurant imagery
Module 2: 4-image grid — Four use cases (pork, chicken, ribs, dipping)
Module 3: Feature highlights — Mustard-based, all-natural, SC heritage (3 pillars)
Module 4: Comparison chart — Maurice's vs Cattlemen's vs Lillie's Q
Module 5: Lifestyle banner — Backyard BBQ party scene, warm lighting
Module 6: FAQ module — "What is mustard BBQ?", "Is it spicy?", "Why the premium?"
Module 7: Cross-sell — Other flavors (Hot, Hickory, Sweet) + variety pack

TRUST THREADING:
"Authentic" threaded across: Main Image (label), Image 6 (restaurant), A+ M1, A+ M3, Bullet 2
"Clean/Natural" threaded across: Image 4, Image 6 (ingredients), A+ M3, A+ M4, Bullet 3
"Worth the Price" threaded across: Image 7 (value), A+ M4 (comparison), A+ M1 (heritage), Bullet 5
```

---

## Appendix A: Integration with Existing System

### How This Connects to the Domain Model (DOMAIN-MODEL.md Part 8)

This system **implements and extends** the entities defined in Part 8:

| Domain Model Entity | This System's Entity | Extension |
|--------------------|---------------------|-----------|
| CustomerSegment | BuyerPersona | Adds evidence, unstated needs, search behavior, listing element importance |
| KeywordIntent | KeywordEntry | Adds keyword type taxonomy, priority, placement map |
| ListingElement | ListingCopyOption + DesignBrief | Splits into copy and visual with multiple options |
| DesignBrief | DesignBrief + AttributeVisualBrief | Adds per-attribute visual ideas, trust threading |
| ListingAudit | (Becomes input to Phase 1) | Existing listing assessment feeds the pipeline |

### How This Connects to Existing Agents

| Agent | Role in Listing Optimization |
|-------|------------------------------|
| Research Agent | Executes Phase 1 (research & discovery) |
| Knowledge Agent | Provides frameworks during all phases |
| Data Agent | Provides performance data as input context |
| Onboarding Agent | Can trigger listing optimization as part of product onboarding |
| Logging Agent | Logs listing optimization decisions and changes |
| Memory Agent | Tracks listing optimization outcomes in brand memory |

### New Agent Needed: Listing Optimization Agent

A new agent that orchestrates the full pipeline:
- Accepts inputs and manages the InputBundle
- Coordinates with Research Agent for Phase 1
- Runs Phases 2-7 with checkpoint management
- Produces all output artifacts
- Handles re-runs when upstream changes occur

---

## Appendix B: Metrics for Measuring Listing Optimization Success

| Metric | Baseline (Before) | Target (After) | Measurement Window |
|--------|-------------------|---------------|-------------------|
| CTR (organic) | Current | +15-30% | 14-30 days |
| CVR | Current | +5-15% | 14-30 days |
| Sessions | Current | +10-25% (from better indexing) | 30-60 days |
| BSR | Current | Improvement proportional to sales lift | 30-60 days |
| ACOS | Current | Decrease (same spend, better conversion) | 14-30 days |
| Organic Sales Share | Current | Increase (better organic rank) | 60-90 days |

---

## Appendix C: Glossary

| Term | Definition |
|------|-----------|
| **PKB** | Product Knowledge Base — the structured understanding of what the product is |
| **P0/P1/P2/P3** | Priority levels: Must Show / Should Show / Nice to Show / Background |
| **Trust Threading** | The strategy of weaving a trust signal across multiple listing elements |
| **Unstated Need** | Something the buyer needs to see to convert but didn't explicitly search for |
| **Evidence Corpus** | The structured collection of all research findings |
| **Attribute** | A product characteristic that could matter to a buyer |
| **Master Keyword** | The core category search term with highest volume |
| **B+ Images** | The 7 product image slots on an Amazon listing |
| **A+ Content** | Amazon's enhanced brand content area below the fold |
| **Premium A+** | Extended A+ format with more modules and design options |
| **LQS** | Listing Quality Score — quantitative assessment of listing quality |
| **MYE** | Manage Your Experiments — Amazon's A/B testing platform |

---

*End of Listing Optimization System Design*
