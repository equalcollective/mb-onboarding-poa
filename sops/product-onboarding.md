# Product Onboarding SOP

## Purpose

Create detailed documentation for priority products within a brand, including product understanding, competitive analysis, and product-specific POA.

## When to Use

- During brand onboarding (for priority products)
- When adding a new product to focus on
- When a product needs dedicated attention/turnaround

## How to Onboard a Product

### Step 1: Initiate Product Onboarding

Tell Claude:

> "Let's onboard product [ASIN] for [Brand Name]"

Claude will create the product onboarding document and guide you through it.

### Step 2: Product Identification

Provide basic product data:

| Field | Description |
|-------|-------------|
| ASIN | Amazon Standard Identification Number |
| SKU | Seller SKU (if different) |
| Category | Amazon category and subcategory |
| Sales Rank (BSR) | Current Best Seller Rank |
| Current Price | Listing price |
| Data Dive Link | Link to Helium10/Jungle Scout analysis |

### Step 3: Product Understanding

Claude will guide you through these questions:

#### What is this product?
*Describe the product in plain terms*

#### What does it do?
*Functionality and use cases*

#### Who is the target customer?
*Demographics, psychographics, when/why they buy*

#### What problem does it solve?
*Pain points addressed*

#### What is the value proposition?
*Why should a customer choose this?*

#### How is it unique?
*Differentiation from competitors*

#### Key Features & Benefits
| Feature | Benefit to Customer |
|---------|---------------------|
| | |

#### What do customers care about when buying?
*Purchase decision factors*

### Step 4: Current Performance

Document current state:

| Metric | Value |
|--------|-------|
| Monthly Units | |
| Monthly Revenue | |
| Sessions | |
| Conversion Rate | |
| Buy Box % | |
| Reviews | |
| Rating | |

### Step 5: Review Analysis

Analyze reviews for insights:

**Positive Themes:**
- What do customers love?
- What gets mentioned repeatedly?

**Negative Themes:**
- What complaints come up?
- What's missing or disappointing?

**Actionable Insights:**
- What can we improve based on reviews?

### Step 6: Competitor Analysis

Identify top 3-5 competitors:

#### On-Amazon Competitors
| Competitor | ASIN | Price | Rating | Reviews | Key Differentiator |
|------------|------|-------|--------|---------|-------------------|
| | | | | | |

#### Off-Amazon Competitors
| Competitor | Link | Price | Notes |
|------------|------|-------|-------|
| | | | |

**Competitive Positioning:**
*Where do we stand vs competition?*

### Step 7: Listing Audit

Review current listing quality:

| Element | Status | Notes |
|---------|--------|-------|
| Title | Good/Needs Work | |
| Bullet Points | Good/Needs Work | |
| Description | Good/Needs Work | |
| A+ Content | Yes/No/Needs Update | |
| Main Image | Good/Needs Work | |
| Secondary Images | Good/Needs Work | |
| Video | Yes/No | |
| Backend Keywords | Optimized/Needs Work | |

**Improvement Opportunities:**
-
-

### Step 8: Product Plan of Action

Based on all above, create product-specific POA:

#### Immediate (Week 1-2)
| Action | Priority | Owner |
|--------|----------|-------|
| | | |

#### Short-term (Week 3-4)
| Action | Priority | Owner |
|--------|----------|-------|
| | | |

#### Ongoing
| Action | Frequency |
|--------|-----------|
| | |

---

## Example: Product Onboarding

### Input

> "Let's onboard B00ABC123 for Acme Widgets - it's their pro widget, sells about 500 units a month at $29.99, it's a premium kitchen gadget"

### Claude's Questions

1. *"What does the Widget Pro actually do? What's its primary function?"*
2. *"Who typically buys this - home cooks, professional chefs, gift buyers?"*
3. *"What's the current rating and review count?"*
4. *"Do you know the main competitor products?"*
5. *"Have you looked at the reviews - any patterns in positive or negative feedback?"*

### Resulting Document

Claude creates `/brands/acme-widgets/onboarding/products/B00ABC123.md` with all structured information.

---

## Quick Commands

**Start product onboarding:**
> "Onboard product [ASIN] for [Brand Name]"

**Update existing product doc:**
> "Update the competitor section for [ASIN]"

**Add POA to product:**
> "Add these actions to the POA for [ASIN]: [actions]"

**Review product status:**
> "Show me the onboarding doc for [ASIN]"
