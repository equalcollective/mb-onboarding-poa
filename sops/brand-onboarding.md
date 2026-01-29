# Brand Onboarding SOP

## Purpose

Create comprehensive documentation when onboarding a new client/brand to ensure all basics are covered and serve as the source of truth for the account.

## When Claude Starts Brand Onboarding

Claude automatically detects brand onboarding intent when:

- You mention a brand name that doesn't exist in the system
- You use introduction language ("new client", "just signed")
- You express creation intent ("set up", "onboard") for an unknown entity

**Examples that trigger brand onboarding:**
- "New client called Sunrise Foods"
- "We just signed a brand called XYZ"
- "Let's get started with a new brand - Acme Widgets"
- "Starting work with a new account - Fresh Foods Co"

## When to Onboard

- New client signed
- Taking over an existing account
- Major account restructure

## How to Onboard

### Step 1: Mention the New Brand

Just mention the new brand naturally. Claude will recognize it's unknown and start onboarding:

> "New client called Sunrise Foods, they sell organic snacks on Amazon"

Claude will:
1. Confirm the brand is new
2. Create the folder structure
3. Guide you through each section interactively

### Step 2: Basic Information

Claude will ask for information in conversational chunks (not all at once):

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

Claude creates and tracks the checklist:

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

Document the first POA based on onboarding analysis:

> "Based on everything we've learned, here's our plan for Sunrise Foods..."

This becomes the first strategic log entry.

---

## Files Created During Onboarding

```
/brands/{brand-name}/
  README.md                      # Brand overview
  MEMORY.md                      # Rolling context
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

## Checking Onboarding Status

Just ask about the brand's onboarding:

- "What's the onboarding status for Sunrise Foods?"
- "Show me the checklist for Acme"
- "What reports are still pending?"

Claude will pull the current state and summarize progress.

---

## Updating Checklist Items

Just describe what you've completed:

- "I got Seller Central access for Sunrise Foods"
- "The SQP analysis is done for Acme"
- "Client sent over the brand guidelines"

Claude will update the checklist automatically.
