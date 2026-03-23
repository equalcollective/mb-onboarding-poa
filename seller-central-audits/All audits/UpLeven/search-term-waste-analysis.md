# UpLeven (Uplivin UK) — Search Term Waste Analysis

**Date:** 2026-02-26
**Period:** August 1, 2025 – February 24, 2026
**Data Source:** Search term split on campaign level (806 rows)

---

## The Core Problem

Out of 806 search term rows across all campaigns, only **17 are generating orders**. That's **2.1%**.

| | Search Terms | Spend (£) | Sales (£) | Orders | ROAS |
|---|---|---|---|---|---|
| **Generating orders** | 17 (2.1%) | 42.12 (5.2%) | 6,482.17 (100%) | 21 | 153.90 |
| **Not generating orders** | 789 (97.9%) | 767.38 (94.8%) | 0 (0%) | 0 | 0 |
| **Total** | 806 | 809.50 | 6,482.17 | 21 | 8.01 |

**£42 in spend is driving 100% of the £6,482 in sales. The other £767 — 94.8% of total ad spend — is completely wasted.**

---

## What's Actually Converting (The £42 That Matters)

| Search Term | Campaign | Orders | Spend (£) | Sales (£) | ROAS |
|---|---|---|---|---|---|
| b0f99tmcb5 (ASIN targeting) | UPA2BK-UK-SP-A2 | 3 | 10.58 | 987.00 | 93.29 |
| b0f99xxxx9 (ASIN targeting) | ???-DW | 3 | 6.74 | 925.00 | 137.24 |
| all terrain rollator | ????-hand-?? | 1 | 7.00 | 329.00 | 47.00 |
| rollators 4 wheel with seat folding lightweight | UPA1GY-FBA-SP-A4 | 1 | 1.20 | 329.00 | 274.17 |
| b0cs3q97lg (ASIN targeting) | UPA2BK-UK-SP-A2 | 1 | 1.14 | 329.00 | 288.60 |
| rollator walker upliving | UPA2BK-UK-SP-A2 | 1 | 0.45 | 329.00 | 731.11 |
| rollators 4 wheel with seat folding lightweight | ????-hand-?? | 1 | 5.40 | 298.00 | 55.19 |
| b0f99wkks8 (ASIN targeting) | ???-DW | 1 | 2.77 | 298.00 | 107.58 |
| b0f99tqlrs (ASIN targeting) | ???-DW | 1 | 1.61 | 298.00 | 185.09 |
| rollators | ????-hand-?? | 1 | 0.61 | 298.00 | 488.52 |
| carbon ultralight mobility walker with seat | ????-hand-?? | 1 | 0.60 | 298.00 | 496.67 |
| b085lhpy74 (ASIN targeting) | ???-DW | 1 | 0.55 | 298.00 | 541.82 |
| mobility walker | ???-DW | 1 | 0.55 | 298.00 | 541.82 |
| lightweight rollator walker with seat | ???-DW | 1 | 0.55 | 298.00 | 541.82 |
| b0f99wkks8 (ASIN targeting) | ???-?? | 1 | 0.55 | 298.00 | 541.82 |
| three wheel rollator | ???-DW | 1 | 0.55 | 298.00 | 541.82 |
| rollators 4 wheel with seat folding lightweight all terrain | UPA2BK-UK-SP-A2 | 1 | 1.27 | 274.17 | 215.88 |

---

## Why This Is Happening

All their keyword match types are set to **BROAD** and **PHRASE**. This creates a fundamental structural problem:

1. **The converting search terms are very specific** — "rollators 4 wheel with seat folding lightweight", "all terrain rollator", branded ASIN targets. These are high-intent, purchase-ready queries.

2. **But the broad/phrase match types cast a massive net** — the same campaign is triggering on 163 irrelevant search terms (in the manual campaign alone), spending £245 to generate just 4 orders.

3. **We never actually know what's working** — because broad/phrase match sends budget to hundreds of search terms, there's no way to isolate and scale the winners. The keywords that convert get the same treatment as the hundreds that don't.

**The right approach:** The search terms that are actually converting should be extracted as EXACT match keywords in their own campaigns with dedicated budgets. Broad/phrase should only be used for discovery, with tight budgets and regular search term harvesting.

---

## Campaign-Level Breakdown

| Campaign | Search Terms | Spend (£) | Sales (£) | Orders | ROAS | Wasted Spend (£) |
|---|---|---|---|---|---|---|
| ????-hand-?? (manual) | 163 | 258.66 | 1,223.00 | 4 | 4.73 | 245.05 (94.7%) |
| ???-DW (auto/display) | 260 | 222.65 | 2,713.00 | 9 | 12.19 | 199.77 (89.7%) |
| UPA2BK-UK-SP-A2 (auto) | 217 | 196.83 | 1,919.17 | 6 | 9.75 | 183.38 (93.2%) |
| ???-?? (auto) | 100 | 66.00 | 298.00 | 1 | 4.52 | 63.25 (95.8%) |
| UPA2BK-UK-SP-E1 (exact) | 16 | 34.26 | 0.00 | 0 | 0.00 | 34.26 (100%) |
| UPA1GY-FBA-SP-A4 (auto) | 20 | 18.99 | 329.00 | 1 | 17.32 | 17.79 (93.7%) |
| Others (7 campaigns) | 30 | 12.11 | 0.00 | 0 | 0.00 | 12.11 (100%) |

**Every single campaign has 89–100% wasted spend.** The EXACT match campaign (UPA2BK-UK-SP-E1) has spent £34.26 with zero orders — meaning even their exact match keywords are wrong.

---

## Audit Notes (For Presentation)

### Wasted Ad Spend

> Out of £809.50 in total ad spend, only **£42.12 (5.2%)** is generating sales. The remaining **£767.38 (94.8%) is wasted** — going to 789 search terms that have produced zero orders over 7 months.

### Structural Issue

> The brand has 806 search term entries across their campaigns, but only 17 are converting (2.1%). This is a direct result of running BROAD and PHRASE match types without proper search term harvesting or negative keyword management. The campaigns are paying for hundreds of irrelevant impressions and clicks while the actual converting terms get no dedicated budget or attention.

### What Needs to Change

> The 15 unique search terms that are actually converting need to be isolated into EXACT match campaigns with dedicated budgets. Broad/phrase campaigns should be restructured as low-budget discovery campaigns with aggressive negative keyword lists and regular search term mining. This would allow us to scale spend on what's proven to work while cutting the £767/month in waste.
