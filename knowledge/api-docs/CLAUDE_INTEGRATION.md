# Claude Code Integration Guide

This guide explains how to integrate Claude Code with the Amazon Seller Analytics API to build automated routines, sub-agents, and intelligent data analysis workflows.

## Architecture Overview

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  User (Chat)    │────▶│  Claude Code     │────▶│  Analytics API  │
│                 │◀────│  (Tool Use)      │◀────│  (FastAPI)      │
└─────────────────┘     └──────────────────┘     └─────────────────┘
```

1. User asks a question about Amazon seller data
2. Claude uses tool calls to fetch relevant data from the API
3. Claude analyzes the data and provides insights
4. User receives an intelligent, contextualized response

---

## Getting Started

### Step 1: Fetch Available Tools

```bash
curl -H "X-API-Key: YOUR_API_KEY" https://<your-api-url>/api/claude/tools
```

This returns:
- `tools`: Array of tool definitions in Claude tool-use format
- `system_prompt`: Suggested system prompt for the AI

### Step 2: Configure Claude with Tools

Use the tools and system prompt in your Claude API call:

```python
import anthropic
import requests

client = anthropic.Anthropic()

response = requests.get(
    "https://<your-api>/api/claude/tools",
    headers={"X-API-Key": "YOUR_API_KEY"}
)
tools_config = response.json()

message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=4096,
    system=tools_config["system_prompt"],
    tools=tools_config["tools"],
    messages=[
        {"role": "user", "content": "What are Utah Pneumatics' best selling products?"}
    ]
)
```

### Step 3: Execute Tool Calls

When Claude returns a tool call, execute it:

```python
tool_use = message.content[0]

result = requests.post(
    "https://<your-api>/api/claude/execute",
    headers={"X-API-Key": "YOUR_API_KEY"},
    json={
        "tool_name": tool_use.name,
        "parameters": tool_use.input
    }
)

messages.append({"role": "assistant", "content": message.content})
messages.append({
    "role": "user",
    "content": [{
        "type": "tool_result",
        "tool_use_id": tool_use.id,
        "content": json.dumps(result.json())
    }]
})

final_response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=4096,
    system=tools_config["system_prompt"],
    tools=tools_config["tools"],
    messages=messages
)
```

---

## ASIN Data Model

All product-level data includes these standardized identifier fields:

| Field | Type | Description | Aggregation Levels |
|-------|------|-------------|-------------------|
| `child_asin` | string | Amazon child ASIN (B07ABC1234) | child only |
| `parent_asin` | string | Parent ASIN identifier | child, parent |
| `product_name` | string | Normalized product name | child, parent |
| `variant_name` | string | Variant description | child only |

**Example Response Data:**
```json
{
  "child_asin": "B07ABC1234",
  "parent_asin": "B07XYZ9999",
  "product_name": "Pneumatic Push Connector Kit",
  "variant_name": "6mm - 10 Pack",
  "total_sales": 15234.50,
  "units": 892,
  "sessions": 4521,
  "cvr_pct": 19.7
}
```

---

## Available Tools

### Discovery Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `list_sellers` | Get all available sellers | Start of any analysis to understand available data |
| `get_seller_asins` | Get product catalog structure | Understanding product hierarchy |
| `get_data_coverage` | Check available date ranges | Before querying to ensure data exists |
| `get_data_gaps` | Detect missing data periods | Data quality assessment |

### Analysis Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `get_metrics` | Time-series data with period comparisons | Trend analysis, performance tracking |
| `get_cumulative_metrics` | Aggregated totals over a range | Period summaries, overall performance |
| `get_pivot_table` | Formatted data for reporting | Creating reports, exports |
| `get_yoy_comparison` | Year-over-year analysis | Seasonal comparisons, growth tracking |

### Product Data Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `get_product_cumulative` | Products in rows, metrics summed | Product rankings, comparisons |
| `get_product_trends` | Products with period-by-period columns | WoW/MoM trend visualization |
| `get_product_yoy` | Products with YoY comparison | YoY product performance |

### Alert Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `get_sos_asins` | Get critical issue ASINs | Identifying problems to address |

---

## Complete Tool Schemas (Claude Format)

### list_sellers

```json
{
  "name": "list_sellers",
  "description": "List all available Amazon sellers in the system. Call this first to discover available sellers.",
  "input_schema": {
    "type": "object",
    "properties": {},
    "required": []
  }
}
```

**Returns:** Array of seller objects with seller_id, seller_name, marketplace, asin_count, parent_count, product_count

---

### get_seller_asins

```json
{
  "name": "get_seller_asins",
  "description": "Get the ASIN hierarchy for a seller including all products and their variants. Returns parent_name, parent_asin, and array of children with child_asin, variant_name, title.",
  "input_schema": {
    "type": "object",
    "properties": {
      "seller_name": {
        "type": "string",
        "description": "Name of the seller (e.g., 'Utah Pneumatics')"
      }
    },
    "required": ["seller_name"]
  }
}
```

---

### get_metrics

```json
{
  "name": "get_metrics",
  "description": "Get detailed time-series metrics with flexible filtering. Returns data with all 4 ASIN identifiers (child_asin, parent_asin, product_name, variant_name) plus all metrics.",
  "input_schema": {
    "type": "object",
    "properties": {
      "seller_name": {
        "type": "string",
        "description": "Name of the seller"
      },
      "parent_asins": {
        "type": "array",
        "items": {"type": "string"},
        "description": "Filter by product names (optional)"
      },
      "child_asins": {
        "type": "array",
        "items": {"type": "string"},
        "description": "Filter by specific child ASINs (optional)"
      },
      "start_date": {
        "type": "string",
        "description": "Start date in YYYY-MM-DD format (optional)"
      },
      "end_date": {
        "type": "string",
        "description": "End date in YYYY-MM-DD format (optional)"
      },
      "aggregation_level": {
        "type": "string",
        "enum": ["child", "parent", "account", "custom"],
        "description": "How to group results. 'child' includes all 4 ASIN fields, 'parent' includes parent_asin and product_name."
      },
      "granularity": {
        "type": "string",
        "enum": ["weekly", "monthly"],
        "description": "Time period granularity"
      },
      "include_comparison": {
        "type": "boolean",
        "description": "Include WoW/MoM change columns"
      }
    },
    "required": ["seller_name"]
  }
}
```

**Response includes:**
- ASIN fields: child_asin, parent_asin, product_name, variant_name (based on aggregation_level)
- Sales metrics: total_sales, units, organic_sales, organic_pct
- Traffic metrics: sessions, page_views, cvr_pct, buy_box_pct, mobile_sessions, desktop_sessions
- Advertising metrics: ad_spend, ad_sales, ad_orders, ad_units, impressions, clicks, ctr_pct, cpc, ad_cvr_pct
- Efficiency metrics: avg_price, roas, acos_pct, tacos_pct, ad_sales_pct

---

### get_product_cumulative

```json
{
  "name": "get_product_cumulative",
  "description": "Get cumulative product data with ASINs in rows and aggregated metrics over a time range. Ideal for product rankings and comparisons. Returns all 4 ASIN identifiers for child level.",
  "input_schema": {
    "type": "object",
    "properties": {
      "seller_name": {
        "type": "string",
        "description": "Name of the seller"
      },
      "start_date": {
        "type": "string",
        "description": "Start date in YYYY-MM-DD format"
      },
      "end_date": {
        "type": "string",
        "description": "End date in YYYY-MM-DD format"
      },
      "aggregation_level": {
        "type": "string",
        "enum": ["child", "parent"],
        "description": "'child' returns all 4 ASIN fields, 'parent' returns parent_asin and product_name"
      },
      "granularity": {
        "type": "string",
        "enum": ["weekly", "monthly"]
      },
      "parent_asins": {
        "type": "array",
        "items": {"type": "string"},
        "description": "Filter by product names (optional)"
      },
      "child_asins": {
        "type": "array",
        "items": {"type": "string"},
        "description": "Filter by specific child ASINs (optional)"
      }
    },
    "required": ["seller_name"]
  }
}
```

---

### get_product_trends

```json
{
  "name": "get_product_trends",
  "description": "Get WoW/MoM trends with ASINs in rows and period-labeled metric columns. Returns columns like 'Jan_06_total_sales', 'Jan_06_units', 'Dec_30_total_sales', etc.",
  "input_schema": {
    "type": "object",
    "properties": {
      "seller_name": {
        "type": "string",
        "description": "Name of the seller"
      },
      "start_date": {"type": "string", "description": "Start date YYYY-MM-DD"},
      "end_date": {"type": "string", "description": "End date YYYY-MM-DD"},
      "aggregation_level": {
        "type": "string",
        "enum": ["child", "parent"]
      },
      "granularity": {
        "type": "string",
        "enum": ["weekly", "monthly"]
      },
      "num_periods": {
        "type": "integer",
        "description": "Number of periods to include (default: 8)"
      }
    },
    "required": ["seller_name"]
  }
}
```

---

### get_product_yoy

```json
{
  "name": "get_product_yoy",
  "description": "Get Year-over-Year comparison for products. Returns current metrics, prior year metrics, and YoY change/percentage for each metric.",
  "input_schema": {
    "type": "object",
    "properties": {
      "seller_name": {"type": "string", "description": "Name of the seller"},
      "start_date": {"type": "string", "description": "Start date YYYY-MM-DD"},
      "end_date": {"type": "string", "description": "End date YYYY-MM-DD"},
      "aggregation_level": {
        "type": "string",
        "enum": ["child", "parent"]
      },
      "granularity": {
        "type": "string",
        "enum": ["weekly", "monthly"]
      }
    },
    "required": ["seller_name", "start_date", "end_date"]
  }
}
```

**Response columns include:**
- ASIN identifiers (based on aggregation_level)
- Current metrics: total_sales, units, sessions, ad_spend, ad_sales, etc.
- Prior metrics: total_sales_prior, units_prior, sessions_prior, etc.
- YoY changes: total_sales_yoy_change, total_sales_yoy_pct, etc.
- Derived metrics for both periods: cvr_pct, cvr_pct_prior, roas, roas_prior, etc.

---

### get_data_coverage

```json
{
  "name": "get_data_coverage",
  "description": "Get data coverage summary including available date ranges, weeks, and months. Call before querying to understand data availability.",
  "input_schema": {
    "type": "object",
    "properties": {
      "seller_name": {"type": "string", "description": "Name of the seller"}
    },
    "required": ["seller_name"]
  }
}
```

---

### get_sos_asins

```json
{
  "name": "get_sos_asins",
  "description": "Get SOS (critical issues) ASINs. Returns ASINs with buybox < 50% or sessions declined > 50% week-over-week.",
  "input_schema": {
    "type": "object",
    "properties": {
      "seller_name": {"type": "string", "description": "Name of the seller"}
    },
    "required": ["seller_name"]
  }
}
```

---

## Building Sub-Agents

### Sub-Agent Pattern 1: Performance Reporter

```python
PERFORMANCE_REPORTER_PROMPT = """
You are a Performance Reporter sub-agent. Your job is to:
1. Fetch cumulative metrics for the specified time range
2. Identify top 5 products by total_sales
3. Identify bottom 5 products by total_sales
4. Calculate account-level totals
5. Format a clear performance summary

Always include all 4 ASIN identifiers in your analysis:
- child_asin (for specific variant identification)
- parent_asin (for product grouping)
- product_name (for human-readable naming)
- variant_name (for variant specification)

Output format:
- Account Summary: total_sales, units, sessions, cvr_pct, roas
- Top 5 Products: table with product_name, variant_name, total_sales, units, cvr_pct
- Bottom 5 Products: table with same fields
- Key Insights: 2-3 actionable observations
"""
```

### Sub-Agent Pattern 2: Trend Analyzer

```python
TREND_ANALYZER_PROMPT = """
You are a Trend Analyzer sub-agent. Your job is to:
1. Fetch product trends data for the last 8 weeks
2. Calculate WoW growth rates for key metrics
3. Identify products with consistent growth (3+ weeks)
4. Identify products with declining trends
5. Flag any sudden changes (>50% WoW)

Use aggregation_level='child' to get all 4 ASIN identifiers.

For each product, track:
- child_asin, parent_asin, product_name, variant_name
- Weekly total_sales trajectory
- CVR trend (improving/declining)
- ROAS trend

Output format:
- Growth Leaders: products with 3+ weeks of growth
- Declining Products: products needing attention
- Anomalies: sudden changes requiring investigation
"""
```

### Sub-Agent Pattern 3: YoY Comparator

```python
YOY_COMPARATOR_PROMPT = """
You are a YoY Comparison sub-agent. Your job is to:
1. Fetch YoY data for the specified date range
2. Calculate growth/decline percentages
3. Identify biggest winners (highest YoY growth)
4. Identify underperformers (negative YoY)
5. Analyze seasonal patterns

Use aggregation_level='child' for detailed analysis.

For each product, report:
- All 4 ASIN identifiers
- Current vs Prior year metrics
- YoY percentage changes
- Contributing factors (sessions, CVR, ad performance)

Output format:
- YoY Summary: account-level growth metrics
- Top Growers: products with highest YoY improvement
- Declining Products: products performing worse than last year
- Recommendations: actions to improve YoY performance
"""
```

---

## Recommended Workflows

### Workflow 1: Daily Performance Check

```python
async def daily_performance_check(seller_name: str):
    # 1. Check data coverage
    coverage = await call_tool("get_data_coverage", {"seller_name": seller_name})
    latest_week = coverage["weeks_available"][0]

    # 2. Get SOS ASINs (critical issues)
    sos = await call_tool("get_sos_asins", {"seller_name": seller_name})

    # 3. Get cumulative performance for last 2 weeks
    metrics = await call_tool("get_product_cumulative", {
        "seller_name": seller_name,
        "aggregation_level": "child",  # Get all 4 ASIN fields
        "granularity": "weekly"
    })

    return {
        "latest_data": latest_week,
        "critical_issues": sos["summary"],
        "top_products": sorted(metrics["data"], key=lambda x: x["total_sales"], reverse=True)[:5]
    }
```

### Workflow 2: Weekly Trend Analysis

```python
async def weekly_trend_analysis(seller_name: str):
    # Get 8-week trends at child level for full ASIN detail
    trends = await call_tool("get_product_trends", {
        "seller_name": seller_name,
        "aggregation_level": "child",
        "granularity": "weekly",
        "num_periods": 8
    })

    # Analyze each product's trajectory using all 4 identifiers
    for product in trends["data"]:
        print(f"Product: {product['product_name']}")
        print(f"  Child ASIN: {product['child_asin']}")
        print(f"  Parent ASIN: {product['parent_asin']}")
        print(f"  Variant: {product['variant_name']}")
        # ... analyze trend data
```

### Workflow 3: Monthly YoY Review

```python
async def monthly_yoy_review(seller_name: str, month: str):
    yoy = await call_tool("get_product_yoy", {
        "seller_name": seller_name,
        "start_date": f"{month}-01",
        "end_date": f"{month}-31",
        "aggregation_level": "child"  # Full ASIN detail
    })

    # Identify growth and decline
    growers = [p for p in yoy["data"] if p.get("total_sales_yoy_pct", 0) > 20]
    decliners = [p for p in yoy["data"] if p.get("total_sales_yoy_pct", 0) < -20]

    return {
        "growers": growers,
        "decliners": decliners,
        "overall_yoy": sum(p["total_sales"] for p in yoy["data"]) / sum(p["total_sales_prior"] for p in yoy["data"])
    }
```

---

## System Prompt Template

Use this system prompt when configuring Claude:

```
You are an Amazon Seller Analytics assistant with access to comprehensive seller performance data.

CAPABILITIES:
You can analyze sales, traffic, advertising, and efficiency metrics at multiple levels:
- Account level: Overall seller performance
- Parent level: Product family performance (includes parent_asin, product_name)
- Child level: Individual variant performance (includes all 4 ASIN fields)

ASIN DATA MODEL:
All product-level responses include these standardized fields:
- child_asin: Amazon child ASIN (e.g., B07ABC1234)
- parent_asin: Parent ASIN identifier
- product_name: Human-readable product name
- variant_name: Variant specification (e.g., "6mm - 10 Pack")

IMPORTANT GUIDELINES:
1. Always start by checking which sellers are available if not specified
2. Use get_data_coverage to understand the available date range before querying
3. Use aggregation_level='child' when you need specific variant detail
4. Use aggregation_level='parent' for product-family comparisons
5. Default to weekly granularity for trend analysis
6. When comparing periods, use include_comparison=true

METRICS TO KNOW:
- ROAS > 3 is generally good, > 5 is excellent
- TACoS < 15% is healthy for most categories
- CVR varies by category, compare to seller's historical average
- ACoS should decrease as products mature
- Buybox < 50% is a critical issue requiring immediate attention

RESPONSE FORMAT:
- Lead with the key insight or answer
- Include specific ASIN identifiers when discussing products
- Support with specific numbers from the data
- Suggest next steps or deeper analysis when relevant
- Use tables for multi-product comparisons
```

---

## Error Handling

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| "Seller not found" | Invalid seller_name | Use list_sellers first |
| "No data for range" | Date range has no data | Check get_data_coverage |
| "Invalid date format" | Wrong date format | Use YYYY-MM-DD |
| "Unauthorized" | Missing or invalid API key | Check X-API-Key header |

### Graceful Degradation

```python
result = await call_tool(tool_name, params)
if "error" in result:
    return f"I couldn't fetch that data: {result['error']}. Let me try a different approach..."
```

---

## Performance Tips

1. **Use appropriate aggregation_level**: Only use 'child' when you need variant-level detail
2. **Limit date ranges**: Large ranges are slower; use specific dates when possible
3. **Batch Questions**: Gather all needed data in parallel before analysis
4. **Cache Coverage**: Store coverage data to avoid repeated calls
5. **Use Presets**: Use metric_preset for common analysis types (faster pivot tables)

---

## Example Conversations

### Example 1: Product Detail Query

**User**: "Show me the performance of our pneumatic connectors"

**Claude**:
1. Calls `get_product_cumulative` with aggregation_level="child"
2. Returns:
```
Here's the performance of your Pneumatic Push Connector products:

| Product Name | Variant | Child ASIN | Sales | Units | CVR |
|--------------|---------|------------|-------|-------|-----|
| Pneumatic Push Connector Kit | 6mm - 10 Pack | B07ABC1234 | $15,234 | 892 | 19.7% |
| Pneumatic Push Connector Kit | 8mm - 10 Pack | B07ABC1235 | $12,450 | 734 | 18.2% |
| Pneumatic Push Connector Kit | 6mm - 25 Pack | B07ABC1236 | $8,900 | 412 | 21.5% |

The 6mm - 25 Pack variant has the highest conversion rate at 21.5%.
```

### Example 2: Critical Issues Alert

**User**: "Are there any problems I should know about?"

**Claude**:
1. Calls `get_sos_asins`
2. Returns:
```
I found 3 ASINs requiring attention:

CRITICAL - Low Buybox:
- B07XYZ1234 (Fitting Set - 12mm) - Buybox at 35% (should be >90%)

WARNING - Declining Sessions:
- B07ABC5678 (Connector Kit - 4mm) - Sessions down 73% week-over-week
- B07DEF9012 (Tube Cutter) - Sessions down 55% week-over-week

Recommendations:
1. Check pricing on B07XYZ1234 - competitors may be undercutting
2. Investigate traffic sources for declining products
```
