# Brand Onboarding SOP

## Purpose

Create comprehensive documentation when onboarding a new client/brand to ensure all basics are covered and serve as the source of truth for the account.

## When to Use

- New client signed
- Taking over an existing account
- Major account restructure

## How to Onboard

### Step 1: Initiate Onboarding

Tell Claude:

> "Let's onboard a new brand called [Brand Name]"

Claude will:
1. Create the brand folder structure
2. Guide you through each section interactively
3. Create all necessary documents

### Step 2: Basic Information

Claude will ask for:

| Information | Example |
|-------------|---------|
| Brand Name | Acme Widgets |
| Account Manager | John Smith |
| Start Date | 2024-01-15 |
| Amazon Storefront Link | amazon.com/acme |
| DTC Website | acmewidgets.com |
| Instagram | @acmewidgets |
| TikTok | @acmewidgets |
| Creative Assets Drive | drive.google.com/... |

### Step 3: Brand Understanding

Claude will guide you through documenting:

- **About the Brand**: History, positioning, brand story
- **Target Customer**: Demographics, psychographics, buyer persona
- **Value Propositions**: What makes them unique
- **Client Goals**: Revenue targets, growth objectives, priorities

### Step 4: Report Analysis

Work through each analysis (can be done over multiple sessions):

1. **SQP Analysis** - Search query performance
2. **Business Report** - Traffic and sales analysis
3. **Data Dive** - Market and product deep dive
4. **Ad Report** - Advertising performance
5. **Competitor Analysis** - Competitive landscape
6. **Inventory Analysis** - Stock and fulfillment

For each, Claude provides a template and guides you through filling it.

### Step 5: Priority Products

Based on analysis, identify 3-5 priority products:

| Priority | ASIN | Product | Reason |
|----------|------|---------|--------|
| 1 | B00XXX | Widget Pro | Top revenue, optimization potential |
| 2 | B00YYY | Widget Mini | Growth opportunity |
| 3 | B00ZZZ | Widget Bundle | High margin |

### Step 6: Onboarding Checklist

Claude will create the checklist. Work through it to ensure nothing is missed:

#### Access & Permissions
- [ ] Seller Central access
- [ ] Advertising Console access
- [ ] Brand Registry access
- [ ] A+ Content permissions
- [ ] Brand Analytics access

#### Client Data
- [ ] COGS data received
- [ ] Design assets received
- [ ] Brand guidelines received

#### Client Understanding
- [ ] Customer understanding documented
- [ ] Client goals defined
- [ ] Success metrics agreed
- [ ] Communication cadence set

#### Operations Check
- [ ] Asset structure audit
- [ ] Buy Box analysis
- [ ] Pricing check
- [ ] Listing quality audit
- [ ] Backend keywords review

#### Analysis Complete
- [ ] SQP Analysis
- [ ] Data Dive
- [ ] Business Report
- [ ] Ad Report
- [ ] Competitor Analysis
- [ ] Inventory Analysis

#### Planning
- [ ] Priority products selected
- [ ] Product briefs created
- [ ] Account-level POA created
- [ ] Trackers set up

### Step 7: Initial Plan of Action

Document the first POA:

> "Based on the onboarding analysis, let's create the initial plan of action for [Brand Name]"

This becomes the first strategic log entry.

---

## Files Created During Onboarding

```
/brands/{brand-name}/
  README.md                      # Brand overview
  /onboarding/
    account.md                   # Full onboarding doc
    checklist.md                 # Checklist status
    /reports/
      sqp-analysis.md
      business-report.md
      data-dive.md
      ad-report.md
      competitor-analysis.md
      inventory-analysis.md
    /products/
      {asin-1}.md               # Product onboarding docs
  /logs/
    YYYY-MM-DD-{author}-poa.md  # Initial POA
```

---

## Timeline Expectation

| Phase | Duration | Activities |
|-------|----------|------------|
| Setup | Day 1 | Access, folder creation, basic info |
| Analysis | Days 2-5 | All report analyses |
| Product Onboarding | Days 3-7 | Priority product deep dives |
| Planning | Day 5-7 | POA creation, tracker setup |
| Review | Day 7 | Final review, handoff if needed |

---

## Checklist Quick Reference

Tell Claude:

> "Show me the onboarding checklist for [Brand Name]"

To update a checklist item:

> "Mark 'SQP Analysis' as complete on [Brand Name] checklist"
