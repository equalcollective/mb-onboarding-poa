# Common Knowledge Base

> Distilled wisdom synthesized from all knowledge sources.
> This is the "always reference" document for core principles.

**Last Synthesized:** 2026-01-28
**Source Documents:** 1

---

## Universal Principles

### 1. Never Conclude Without Context
Same metric can mean different things in different contexts. Always establish benchmarks, historical trends, and segmentation before diagnosing.

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### 2. Data Sources Capture Different Things
Different Amazon reports (SQP, Ads, Business) capture different slices of data. Understanding what each report includes/excludes is critical to avoid misinterpretation.

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### 3. Historical Performance Matters
Before pausing or cutting anything, check if it ever performed well. Amazon's algorithm remembers past performance.

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### 4. Optimization is Iterative
Test, measure, learn, repeat. Small improvements compound over time. Don't expect perfect results immediately.

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

---

## Core Frameworks

### CTR Diagnosis Framework
**Use when:** CTR appears low and you need to determine root cause

1. Check industry/category benchmark
2. Check account-level CTR trends
3. Check branded vs non-branded split
4. Check placement-specific CTR (search vs product pages)
5. Check campaign-type breakdown
6. Check historical trends
7. THEN evaluate creative if still unexplained

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### New Account Analysis Framework
**Use when:** First looking at any new account

1. **Lay of the Land (15-30 min):** Products, campaigns, spend, ROAS, organic/paid split
2. **Top Performers (15 min):** Best campaigns, best ROAS, top search terms
3. **Problem Areas (30 min):** Losing money, wasted budget, low volume
4. **Form Hypotheses (15 min):** Why winners win, why losers lose
5. **Prioritize Actions (15 min):** Biggest impact, immediate vs needs approval

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

### Scale or Cut Decision Framework
**Use when:** Deciding whether to increase budget or pause a campaign

**Scale if:** Good ROAS 30+ days, reasonable volume, budget room, no cannibalization
**Cut if:** Poor ROAS 30+ days, high spend, no improving trend, no fixable issue
**Hold if:** Recent changes need time, borderline performance, testing in progress

**Sources:** [amazon-ads-analysis-28jan-ketogoods-call](./amazon-ads-analysis-28jan-ketogoods-call.md)

---

## Essential Checklists

### Before Analyzing Any Campaign
- [ ] Verify date range is correct
- [ ] Check lifetime performance (not just recent)
- [ ] Understand placement distribution
- [ ] Separate branded vs non-branded
- [ ] Check if budget/bids are constraining

### Before Making Bid/Budget Changes
- [ ] Sufficient data to judge (min $100-300 spend)
- [ ] Checked historical performance
- [ ] Understand placement impact
- [ ] Have hypothesis for why current state exists

### Before Presenting to Client
- [ ] Cite specific data points
- [ ] Acknowledge uncertainties
- [ ] Provide context (benchmarks, history)
- [ ] Clear recommendations with expected outcomes

---

## Key Metrics & Benchmarks

| Metric | Good | Average | Poor | Notes |
|--------|------|---------|------|-------|
| CTR (Top of Search) | >2% | 1-2% | <1% | Category dependent |
| CTR (Product Pages) | >0.5% | 0.2-0.5% | <0.2% | Always lower than search |
| ACOS (Brand Defense) | <15% | 15-25% | >25% | Should be very efficient |
| ACOS (SP Auto) | <30% | 30-40% | >40% | Discovery, can run higher |
| ACOS (SP Exact) | <25% | 25-35% | >35% | Highest intent |
| Min spend for decision | $100-300 | - | - | Statistical significance |

---

## Common Pitfalls

| Pitfall | Why It's Wrong | What To Do Instead |
|---------|----------------|-------------------|
| Daily-level pivot analysis | Fragments performance, hides overall trends | Group by week/month minimum |
| Concluding without checking history | Misses that campaign worked before | Always check lifetime data |
| Comparing CTR across categories | Different products have different expectations | Use category benchmarks |
| Pausing low-spend campaigns | Not enough data to judge | Wait for $100+ spend |
| Ignoring placement mix | Product pages drag down averages | Segment by placement |

---

## Report Interpretation Quick Reference

| Report | Includes | Does NOT Include |
|--------|----------|------------------|
| SQP (Search Query Performance) | Search-based traffic only | Product page impressions |
| Ads/Search Term Report | All ad impressions including product pages | Organic traffic |
| Business Report | All traffic (organic + paid) | Ad-specific metrics |
| Placement Report | Placement distribution | Search term details |

---

*This document is automatically updated as new knowledge is ingested.*
