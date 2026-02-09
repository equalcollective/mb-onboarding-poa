# Listing Optimization System — Overview

> **Full system:** [LISTING-OPTIMIZATION-SYSTEM.md](./LISTING-OPTIMIZATION-SYSTEM.md)

---

## The Problem

Most Amazon listings **describe** the product. They don't **sell** it. Listings are built from guesswork — someone writes bullets based on what they think matters, picks images based on what looks nice, and stuffs keywords based on what seems relevant. The result is a product spec sheet, not a sales machine.

## The Solution

A **10-phase pipeline** that transforms raw inputs (an ASIN, a website, competitor links, a brand story) into a fully optimized listing strategy — where every element is evidence-backed, persona-targeted, and AM-approved.

---

## Pipeline at a Glance

```
 UNDERSTAND                    STRATEGIZE                     EXECUTE
 ─────────────────────         ─────────────────────          ─────────────────────
 1. Research & Discovery       6. Product Attributes          9.  Listing Copy
 2. Brand Understanding        7. Keyword Strategy            10. Design Brief
 3. Product Knowledge          8. Real Estate Allocation
 4. Positioning
 5. Buyer Personas

 Each phase feeds the next. The AM validates at 8 checkpoints.
```

---

## What Each Phase Does

| # | Phase | Key Question Answered | Output |
|---|-------|-----------------------|--------|
| 1 | **Research** | What does the evidence say? | Evidence corpus from 7 domains (product, cultural, competitive, reviews, search, category, brand ecosystem) + SQP/ads data |
| 2 | **Brand** | Who is this brand? | Brand identity, founder story, voice & tone, values |
| 3 | **Product** | What is this product *really*? | Product Identity Map, value prop hierarchy, subjective/objective classification, education needs |
| 4 | **Positioning** | How should we frame this in the market? | Positioning statements (P0 selected), competitor map, mass vs niche |
| 5 | **Personas** | Who buys this and why? | 4-6 evidence-based personas with decision drivers, objections, and unstated needs |
| 6 | **Attributes** | What matters, to whom, and are they aware of it? | Master attribute list tagged by type, priority, persona, and **aware/unaware** |
| 7 | **Keywords** | What should we rank for? | Prioritized keywords (P0-P3) traced to upstream phases + SQP/ads data |
| 8 | **Allocation** | What goes where on the listing? | Product-specific map assigning attributes to title, bullets, images, A+, Brand Story |
| 9 | **Copy** | What do we say? | 3 title options, 18 bullet options (from 10 frames), backend keywords — AM picks the combo |
| 10 | **Design Brief** | What do we show? | Image slot briefs, A+ module briefs, Brand Story carousel plan, trust threading map |

---

## Key Concepts That Make This Different

### Aware vs. Unaware Attributes

The single most important framework in the system.

- **Aware** attributes are what buyers search for — they drive clicks. Place them high (title, bullets 1-2, main image).
- **Unaware** attributes are what buyers need to see but didn't search for — they drive conversions. Place them deep (bullets 3-5, secondary images, A+).

A listing with only Aware attributes attracts but doesn't convert. A listing with only Unaware attributes converts but nobody finds it. The system maps every attribute to its awareness level and places it accordingly.

### The Subjective–Objective Spectrum

Products range from purely functional (USB cable) to deeply personal (perfume). Where a product sits on this spectrum changes everything about how we sell it — objective products need "tell" (specs, data); subjective products need "show" (lifestyle, story, emotion). The system classifies each product and adjusts allocation strategy accordingly.

### Trust Threading

Trust isn't built in one bullet or one image. It's woven across the entire listing. The system identifies 3-4 trust signals per product (e.g., "this is authentic," "this is safe to buy," "this is worth the price") and threads each one across 5+ touchpoints — so by the time the buyer reaches "Add to Cart," every signal is deeply embedded.

### Every Phase Feeds the Next

Keywords aren't researched in a vacuum — they emerge from knowing who the buyers are (Phase 5), what attributes matter (Phase 6), how the product is positioned (Phase 4), and what cultural language surrounds it (Phase 2). Every phase is richer because of the phases before it.

---

## How the AM Interacts

The system does the heavy lifting. The AM validates and steers at 8 checkpoints:

```
 SYSTEM gathers + thinks → SYSTEM produces artifact → AM reviews
                                                        ├─ ✓ Approve → next phase
                                                        ├─ ✎ Modify → system revises
                                                        ├─ ← Add context → system enriches
                                                        └─ ↺ Regenerate → system tries again
```

The AM never writes from scratch. They react, refine, and approve.

---

## Inputs

Minimum: **one ASIN**. The system can start with just a listing URL and build from there. More inputs produce richer outputs:

| Input | Value |
|-------|-------|
| ASIN / Amazon listing | Current listing, reviews, Q&A, competitors |
| D2C website | Brand story, richer descriptions |
| Social media | Brand voice, UGC, lifestyle context |
| SQP data | Click share, conversion share gaps |
| Amazon Ads data | Actual converting search terms |
| AM/client knowledge | Insider context the internet doesn't know |

---

## Outputs

The pipeline produces a complete listing strategy:

- **Brand & product knowledge base** — reusable across future optimizations
- **Positioning & competitor map** — who we compete with and how we're different
- **Buyer personas** — evidence-based, not demographic guesswork
- **Prioritized attribute list** — tagged by type, awareness, persona, placement
- **Keyword strategy** — traced to upstream intelligence, AM-approved
- **Allocation map** — what goes where on the listing and why
- **Multiple title & bullet options** — with keyword coverage tracking
- **Design briefs** — for every image slot, A+ module, and Brand Story card
- **Compliance checklist** — to prevent suppression before going live

---

## Compliance Guardrails

The system includes built-in compliance checks (Jan 2025 rules):

| Area | Key Rules |
|------|-----------|
| **Title** | 200 char max, 70 char mobile cutoff, no word repeated >2x, 14-day auto-override grace |
| **Bullets** | 500 char/bullet, 1,000 byte indexing limit (front-load keywords), no full ALL CAPS |
| **Backend** | 249 bytes (1 byte over = entire field de-indexed), no brand names, fill Subject/Intended Use/Target Audience |
| **Images** | White background main image, 85%+ fill, 1000px minimum, up to 9 slots |
| **A+** | 5 modules standard, body text NOT indexed (alt text IS), comparison chart own products only |

---

## What This Is NOT

- Not a one-shot "generate a listing" tool
- Not a keyword stuffing system
- Not a template that works the same for every product

It's a **structured thinking framework** that produces artifacts at each stage. The AI does the research, synthesis, and generation. The AM validates and steers. The result is a listing strategy that's both deeply informed and human-approved.

---

*For the full system with examples, frameworks, and detailed phase documentation, see [LISTING-OPTIMIZATION-SYSTEM.md](./LISTING-OPTIMIZATION-SYSTEM.md).*
