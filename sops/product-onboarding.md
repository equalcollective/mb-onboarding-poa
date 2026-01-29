# Product Onboarding SOP

## Purpose

Create detailed documentation for priority products within a brand, including product understanding, competitive analysis, and product-specific POA.

## When Claude Starts Product Onboarding

Claude detects product onboarding intent when:

- You mention an ASIN that doesn't have a product doc
- You want to focus on a specific product
- You request product research or analysis for a new product

**Examples that trigger product onboarding:**
- "Need to add B00ABC1234 to the Acme account"
- "Let's focus on the new connector kit for Utah Pneumatics"
- "This product needs its own doc - B00XYZ789"
- "Research and onboard the Widget Pro"

## When to Onboard Products

- During brand onboarding (for priority products)
- When adding a new product to focus on
- When a product needs dedicated attention/turnaround

## How to Onboard a Product

### Step 1: Mention the Product

Just reference the product naturally. Claude will recognize it needs a doc:

> "Let's focus on B00ABC1234 for Acme - it's their best seller but needs attention"

Claude will:
1. Verify the brand exists
2. Check if product doc already exists
3. Create the product document
4. Guide you through sections

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

Claude offers two options:

**Option A: Quick Manual Entry**
Provide competitor info you already have:
- ASIN, price, rating, review count, key differentiator

**Option B: Full Research Brief (Recommended)**
Say "research this" and Claude will:
- Fetch and analyze the Amazon listing
- Identify 5-8 competitors (you'll confirm which to include)
- Analyze D2C competitors
- Generate a comprehensive research brief

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

### Natural Input

> "Need to add the Widget Pro to Acme's account - it's B00ABC123, sells about 500 units a month at $29.99, premium kitchen gadget"

### Claude's Response

Claude will:
1. Create the product doc
2. Ask clarifying questions:
   - *"What does the Widget Pro actually do? What's its primary function?"*
   - *"Who typically buys this - home cooks, professional chefs, gift buyers?"*
   - *"What's the current rating and review count?"*
   - *"Do you know the main competitor products?"*
   - *"Have you looked at the reviews - any patterns in positive or negative feedback?"*

3. Create `/brands/acme-widgets/onboarding/products/B00ABC123.md` with structured information

---

## Checking Product Status

Just ask about the product:

- "What's the status on B00ABC123?"
- "Show me the Widget Pro doc"
- "What's the POA for the connector kit?"

Claude will pull and summarize the current state.

---

## Updating Product Docs

Describe what needs updating:

- "Update competitor section - their price dropped to $24.99"
- "Add this action to the Widget Pro POA: submit new images"
- "The review count is now at 500 for B00ABC123"

Claude will make the appropriate updates.
