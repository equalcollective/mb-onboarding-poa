# Amazon Seller Analytics API Reference

## Overview

This API provides access to Amazon seller analytics data including sales metrics, advertising performance, traffic analysis, and year-over-year comparisons. The data is sourced from Metabase and processed through a metrics engine.

**Base URL**: `https://mb-onboarding.replit.app/api`

## Authentication

API requests require authentication via either:
- Session cookie (for web users)
- API key header: `X-API-Key: <your-api-key>`

---

## ASIN Data Model

Every API response that returns product-level data includes consistent ASIN identifier fields:

### ASIN Identifier Fields

| Field | Type | Description | Aggregation Levels |
|-------|------|-------------|-------------------|
| `child_asin` | string | Amazon child ASIN (e.g., B07ABC1234) | child only |
| `parent_asin` | string | Parent ASIN identifier | child, parent |
| `product_name` | string | Normalized product name (human-readable) | child, parent |
| `variant_name` | string | Variant description (e.g., "6mm - 10 Pack") | child only |

**Field Availability by Aggregation Level:**
- `child` level: All 4 fields (child_asin, parent_asin, product_name, variant_name)
- `parent` level: 2 fields (parent_asin, product_name)
- `account` level: No ASIN fields (aggregated totals)

---

## Endpoints

### 1. List Sellers

Get all available Amazon sellers in the system.

```
GET /api/sellers
```

**Response:**
```json
[
  {
    "seller_id": 123,
    "seller_name": "Utah Pneumatics",
    "marketplace": "US",
    "agency_name": "SELF",
    "asin_count": 45,
    "parent_count": 12,
    "product_count": 12
  }
]
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| seller_id | integer | Unique identifier |
| seller_name | string | Display name (use this for other API calls) |
| marketplace | string | Amazon marketplace (US, UK, CA, etc.) |
| agency_name | string | Managing agency (SELF = self-managed) |
| asin_count | integer | Number of child ASINs |
| parent_count | integer | Number of parent ASINs |
| product_count | integer | Number of unique products |

---

### 2. Get Seller ASIN Hierarchy

Get the product catalog structure for a seller.

```
GET /api/seller/{seller_name}/asins
```

**Path Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| seller_name | string | Yes | Name of the seller |

**Response:**
```json
[
  {
    "parent_name": "Pneumatic Push Connector Kit",
    "parent_asin": "B07XYZ9999",
    "child_count": 3,
    "children": [
      {
        "child_asin": "B07ABC1234",
        "variant_name": "6mm - 10 Pack",
        "title": "Pneumatic Push Connect..."
      }
    ]
  }
]
```

---

### 3. Get Metrics

Get detailed time-series metrics with flexible filtering.

```
POST /api/seller/{seller_name}/metrics
```

**Request Body:**
```json
{
  "parent_asins": ["Product Name 1", "Product Name 2"],
  "child_asins": ["B07ABC1234"],
  "start_date": "2024-12-01",
  "end_date": "2025-01-15",
  "aggregation_level": "child",
  "granularity": "weekly",
  "include_comparison": true
}
```

**Parameters:**
| Field | Type | Default | Description |
|-------|------|---------|-------------|
| parent_asins | string[] | null | Filter by product names |
| child_asins | string[] | null | Filter by specific ASINs |
| start_date | date | null | Start of date range (YYYY-MM-DD) |
| end_date | date | null | End of date range (YYYY-MM-DD) |
| aggregation_level | string | "account" | Grouping: child, parent, account, custom |
| granularity | string | "weekly" | Time period: weekly, monthly |
| include_comparison | boolean | false | Include WoW/MoM change columns |

**Response (aggregation_level: "child"):**
```json
{
  "seller_name": "Utah Pneumatics",
  "aggregation_level": "child",
  "granularity": "weekly",
  "data": [
    {
      "child_asin": "B07ABC1234",
      "parent_asin": "B07XYZ9999",
      "product_name": "Pneumatic Push Connector Kit",
      "variant_name": "6mm - 10 Pack",
      "period_start_date": "2025-01-06",
      "total_sales": 15234.50,
      "units": 892,
      "sessions": 4521,
      "cvr_pct": 19.7,
      "ad_spend": 1250.00,
      "ad_sales": 5200.00,
      "roas": 4.16,
      "acos_pct": 24.0,
      "tacos_pct": 8.2,
      "organic_sales": 10034.50,
      "organic_pct": 65.9,
      "impressions": 125000,
      "clicks": 3500,
      "ctr_pct": 2.8,
      "buy_box_pct": 98.5,
      "total_sales_wow_pct": 12.5
    }
  ],
  "count": 24,
  "columns": ["child_asin", "parent_asin", "product_name", "variant_name", "period_start_date", "total_sales", ...]
}
```

---

### 4. Get Product Cumulative Data

Get aggregated metrics summed over a time range, with products as rows.

```
POST /api/seller/{seller_name}/product-data/cumulative
```

**Request Body:**
```json
{
  "start_date": "2024-12-01",
  "end_date": "2025-01-15",
  "aggregation_level": "child",
  "granularity": "weekly",
  "parent_asins": [],
  "child_asins": []
}
```

**Response (aggregation_level: "child"):**
```json
{
  "seller_name": "Utah Pneumatics",
  "aggregation_level": "child",
  "granularity": "weekly",
  "columns": ["child_asin", "parent_asin", "product_name", "variant_name", "total_sales", "units", "sessions", ...],
  "data": [
    {
      "child_asin": "B07ABC1234",
      "parent_asin": "B07XYZ9999",
      "product_name": "Pneumatic Push Connector Kit",
      "variant_name": "6mm - 10 Pack",
      "total_sales": 45678.90,
      "units": 2534,
      "sessions": 12500,
      "cvr_pct": 20.3,
      "ad_spend": 4500.00,
      "ad_sales": 18000.00,
      "roas": 4.0,
      "acos_pct": 25.0,
      "tacos_pct": 9.8,
      "organic_sales": 27678.90,
      "organic_pct": 60.6,
      "period_start": "2024-12-01",
      "period_end": "2025-01-12",
      "periods_count": 7
    }
  ],
  "count": 45
}
```

---

### 5. Get WoW/MoM Trends

Get period-by-period comparison with metrics as columns for each period.

```
POST /api/seller/{seller_name}/product-data/trends
```

**Request Body:**
```json
{
  "start_date": "2024-12-01",
  "end_date": "2025-01-15",
  "aggregation_level": "child",
  "granularity": "weekly",
  "num_periods": 8
}
```

**Response:**
```json
{
  "seller_name": "Utah Pneumatics",
  "aggregation_level": "child",
  "granularity": "weekly",
  "periods": ["2024-12-02", "2024-12-09", "2024-12-16", "2024-12-23", "2024-12-30", "2025-01-06"],
  "period_labels": ["Dec_02", "Dec_09", "Dec_16", "Dec_23", "Dec_30", "Jan_06"],
  "metrics": ["total_sales", "units", "sessions", "ad_spend", "ad_sales", "roas", "acos_pct", "tacos_pct", "cvr_pct", "organic_sales", "organic_pct"],
  "data": [
    {
      "child_asin": "B07ABC1234",
      "parent_asin": "B07XYZ9999",
      "product_name": "Pneumatic Push Connector Kit",
      "variant_name": "6mm - 10 Pack",
      "Jan_06_total_sales": 5234.50,
      "Jan_06_units": 312,
      "Jan_06_sessions": 1580,
      "Jan_06_cvr_pct": 19.7,
      "Dec_30_total_sales": 4890.00,
      "Dec_30_units": 287,
      "Dec_30_sessions": 1420,
      "Dec_30_cvr_pct": 20.2
    }
  ],
  "count": 45
}
```

---

### 6. Get Year-over-Year Comparison

Compare a date range with the same period from the previous year.

```
POST /api/seller/{seller_name}/product-data/yoy-range
```

**Request Body:**
```json
{
  "start_date": "2025-01-01",
  "end_date": "2025-01-31",
  "aggregation_level": "child",
  "granularity": "weekly"
}
```

**Response:**
```json
{
  "seller_name": "Utah Pneumatics",
  "aggregation_level": "child",
  "granularity": "weekly",
  "current_range": {"start": "2025-01-01", "end": "2025-01-31"},
  "prior_range": {"start": "2024-01-01", "end": "2024-01-31"},
  "data": [
    {
      "child_asin": "B07ABC1234",
      "parent_asin": "B07XYZ9999",
      "product_name": "Pneumatic Push Connector Kit",
      "variant_name": "6mm - 10 Pack",
      "total_sales": 15234.50,
      "total_sales_prior": 12450.00,
      "total_sales_yoy_change": 2784.50,
      "total_sales_yoy_pct": 22.4,
      "units": 892,
      "units_prior": 734,
      "units_yoy_change": 158,
      "units_yoy_pct": 21.5,
      "sessions": 4500,
      "sessions_prior": 3800,
      "sessions_yoy_change": 700,
      "sessions_yoy_pct": 18.4,
      "cvr_pct": 19.8,
      "cvr_pct_prior": 19.3,
      "cvr_pct_yoy_change": 0.5,
      "cvr_pct_yoy_pct": 2.6,
      "roas": 4.2,
      "roas_prior": 3.8,
      "roas_yoy_change": 0.4,
      "roas_yoy_pct": 10.5
    }
  ],
  "count": 45,
  "columns": ["child_asin", "parent_asin", "product_name", "variant_name", "total_sales", "total_sales_prior", ...]
}
```

---

### 7. Get Data Coverage

Get summary of available data periods.

```
GET /api/seller/{seller_name}/coverage
```

**Response:**
```json
{
  "seller_name": "Utah Pneumatics",
  "coverage": {
    "min_date": "2023-01-01",
    "max_date": "2026-01-11",
    "weeks_count": 157,
    "months_count": 37
  },
  "weeks_available": ["2026-01-11", "2026-01-04", "2025-12-28", ...],
  "months_available": ["2026-01-01", "2025-12-01", "2025-11-01", ...]
}
```

---

### 8. Get Data Gaps

Detect missing data periods.

```
GET /api/seller/{seller_name}/gaps?granularity=weekly
```

**Response:**
```json
{
  "seller_name": "Utah Pneumatics",
  "granularity": "weekly",
  "gaps": [
    {
      "period_start": "2024-08-05",
      "period_end": "2024-08-19",
      "missing_weeks": 2
    }
  ],
  "count": 1
}
```

---

### 9. Get Pivot Table

Get pivot table with date-labeled columns for reporting.

```
POST /api/seller/{seller_name}/pivot
```

**Request Body:**
```json
{
  "aggregation_level": "parent",
  "granularity": "weekly",
  "metric_preset": "sales_overview",
  "include_totals": true,
  "period_order": "recent_first"
}
```

**Available Metric Presets:**
| Preset | Metrics Included |
|--------|-----------------|
| `sales_overview` | total_sales, units, sessions, cvr_pct |
| `advertising` | ad_spend, ad_sales, roas, acos_pct, tacos_pct |
| `traffic` | sessions, page_views, impressions, clicks, ctr_pct |
| `efficiency` | cvr_pct, roas, acos_pct, tacos_pct, avg_price |

**Response:**
```json
{
  "seller_name": "Utah Pneumatics",
  "aggregation_level": "parent",
  "granularity": "weekly",
  "periods": ["2026-01-11", "2026-01-04", "2025-12-28"],
  "metrics": ["total_sales", "units", "sessions", "cvr_pct"],
  "columns": ["product_name", "parent_asin", "Jan_11_total_sales", "Jan_11_units", "Jan_04_total_sales", ...],
  "data": [...],
  "count": 12
}
```

---

### 10. Get SOS (Critical Issues) ASINs

Get ASINs with critical performance issues.

```
GET /api/seller/{seller_name}/sos
```

**Response:**
```json
{
  "data": [
    {
      "child_asin": "B07ABC1234",
      "parent_name": "Pneumatic Push Connector Kit",
      "variant_name": "6mm - 10 Pack",
      "buybox_pct": 35.0,
      "current_sessions": 120,
      "previous_sessions": 450,
      "session_change_pct": -73.3,
      "is_low_buybox": true,
      "is_declining_sessions": true,
      "total_sales": 1234.50,
      "units": 45
    }
  ],
  "summary": {
    "total_asins": 3,
    "low_buybox": 2,
    "declining_sessions": 2
  },
  "period": {
    "current_week": "2025-01-06",
    "previous_week": "2024-12-30"
  }
}
```

**SOS Criteria:**
- `is_low_buybox`: Buybox percentage < 50% in current week
- `is_declining_sessions`: Sessions decreased by more than 50% vs previous week

---

## Metrics Reference

### Sales Metrics
| Metric | Field | Description | Formula |
|--------|-------|-------------|---------|
| Total Sales | `total_sales` | Revenue from ordered products | Sum of ordered_product_sales |
| Units Sold | `units` | Number of units ordered | Sum of units_ordered |
| Organic Sales | `organic_sales` | Sales not from ads | total_sales - ad_sales |
| Organic % | `organic_pct` | Percentage of sales that are organic | (organic_sales / total_sales) * 100 |
| Units Refunded | `units_refunded` | Total units refunded | Sum of refunded units |

### Traffic Metrics
| Metric | Field | Description | Formula |
|--------|-------|-------------|---------|
| Sessions | `sessions` | Total sessions from business report | Sum of sessions |
| Mobile Sessions | `mobile_sessions` | Sessions from mobile app | Sum of mobile sessions |
| Desktop Sessions | `desktop_sessions` | Sessions from browser/desktop | Sum of desktop sessions |
| Page Views | `page_views` | Total page views | Sum of page_views |
| Conversion Rate | `cvr_pct` | Purchase conversion rate | (units / sessions) * 100 |
| Buy Box % | `buy_box_pct` | Buy box win percentage | Average of buy_box_percentage |

### Advertising Metrics
| Metric | Field | Description | Formula |
|--------|-------|-------------|---------|
| Ad Spend | `ad_spend` | Total advertising spend | Sum of ad spend |
| Ad Sales | `ad_sales` | Sales attributed to ads (7-day) | Sum of 7-day attributed sales |
| Ad Orders | `ad_orders` | Orders attributed to ads | Sum of 7-day attributed orders |
| Ad Units | `ad_units` | Units sold attributed to ads | Sum of 7-day attributed units |
| Ad Sales % | `ad_sales_pct` | Percentage of sales from ads | (ad_sales / total_sales) * 100 |
| Impressions | `impressions` | Total ad impressions | Sum of impressions |
| Clicks | `clicks` | Total ad clicks | Sum of clicks |
| CTR | `ctr_pct` | Click-through rate | (clicks / impressions) * 100 |
| CPC | `cpc` | Cost Per Click | ad_spend / clicks |
| Ad CVR | `ad_cvr_pct` | Ad conversion rate | (ad_orders / clicks) * 100 |

### Efficiency Metrics
| Metric | Field | Description | Formula |
|--------|-------|-------------|---------|
| AOV | `avg_price` | Average Order Value | total_sales / units |
| ROAS | `roas` | Return on Ad Spend | ad_sales / ad_spend |
| ACoS | `acos_pct` | Ad Cost of Sales | (ad_spend / ad_sales) * 100 |
| TACoS | `tacos_pct` | Total ACoS | (ad_spend / total_sales) * 100 |

### Comparison Metrics (when include_comparison=true)
| Metric | Field | Description |
|--------|-------|-------------|
| WoW Change | `{metric}_wow_change` | Absolute change vs prior period |
| WoW % | `{metric}_wow_pct` | Percentage change vs prior period |

---

## Aggregation Levels

| Level | Description | ASIN Fields Included |
|-------|-------------|---------------------|
| `account` | All products combined per period | None (aggregated totals) |
| `parent` | Grouped by product (parent ASIN) per period | parent_asin, product_name |
| `child` | Individual ASIN per period | child_asin, parent_asin, product_name, variant_name |
| `custom` | Single aggregated row across all filters | None (aggregated totals) |

---

## Error Handling

All endpoints return standard HTTP status codes:

| Code | Description |
|------|-------------|
| 200 | Success |
| 400 | Bad request (invalid parameters) |
| 401 | Unauthorized (missing or invalid authentication) |
| 404 | Resource not found |
| 500 | Internal server error |

Error response format:
```json
{
  "detail": "Error message describing what went wrong"
}
```

---

## Claude Code Integration

This API is designed for integration with Claude Code to build automated routines and sub-agents. See `CLAUDE_INTEGRATION.md` for:

- Tool definitions in Claude tool-use format
- System prompts for analytics agents
- Example workflows and conversation patterns
- Best practices for building sub-agents

### Quick Start for Claude Code

1. Fetch tools: `GET /api/claude/tools`
2. Execute tool calls: `POST /api/claude/execute`

The `/api/claude/tools` endpoint returns all available tools with their schemas in Claude-compatible format.
