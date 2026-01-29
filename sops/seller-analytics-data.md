# Seller Analytics Data SOP

## Overview

This document covers how to query seller analytics data correctly, including date alignment requirements for weekly and monthly granularity.

## Data Source

The seller analytics MCP server provides access to:
- **Business Report data** (sales, sessions, page views, conversion, Buy Box %)
- **Advertising data** (spend, sales, ROAS, ACOS, impressions, clicks)

## Granularity Options

| Granularity | Period Start | Use For |
|-------------|--------------|---------|
| `weekly` | **Sunday** | Week-over-week analysis, recent trends |
| `monthly` | **1st of month** | Month-over-month analysis, YoY comparisons |

---

## Date Alignment Requirements

### Weekly Data

**Week starts on Sunday.** When querying weekly data:

1. **Preferred:** Use Sunday-aligned dates
   - Example: `2025-12-07` (Sunday) to `2025-12-21` (Sunday)

2. **If non-Sunday provided:** Auto-snap to previous Sunday
   - Query with `2025-12-10` (Wednesday) → Snap to `2025-12-07` (Sunday)

**Auto-snap formula:**
```
days_since_sunday = (date.weekday() + 1) % 7
aligned_date = date - days_since_sunday
```

**Common Sunday dates reference:**
| Month | Sundays |
|-------|---------|
| Dec 2025 | 7, 14, 21, 28 |
| Jan 2026 | 4, 11, 18, 25 |

### Monthly Data

**Month starts on the 1st.** When querying monthly data:

1. **Preferred:** Use 1st of month dates
   - Example: `2025-12-01` to `2026-01-01`

2. **If non-1st provided:** Auto-snap to 1st of that month
   - Query with `2025-12-15` → Snap to `2025-12-01`

**Auto-snap formula:**
```
aligned_date = date.replace(day=1)
```

---

## API Filtering Behavior

The MCP filters data where:
```
period_start_date >= start_date AND period_start_date <= end_date
```

This means:
- It does **NOT** include partial periods
- It only returns periods whose start date falls within the range
- If your date range doesn't include any period start dates, you get empty results

### Example

Query: `start_date=2025-12-03, end_date=2025-12-20, granularity=weekly`

| Period Start | Included? |
|--------------|-----------|
| 2025-11-30 | No (before range) |
| 2025-12-07 | Yes |
| 2025-12-14 | Yes |
| 2025-12-21 | No (after range) |

---

## Available Tools

### list_sellers
Get all available sellers with ASIN counts.

### get_seller_asins
Get ASIN hierarchy (parent → child) for a seller.

### get_data_coverage
Check date ranges and period counts for a seller's data.

### get_metrics
Query time-series data with filters:
- `seller_name` (required)
- `granularity`: `weekly` or `monthly`
- `aggregation_level`: `account`, `parent`, or `child`
- `start_date`, `end_date`: Date range (align to period boundaries!)
- `parent_asins`, `child_asins`: Filter to specific products

### get_cumulative_metrics
Get totals across a date range (single aggregated record).

### get_pivot_table
Get data formatted with date-labeled columns for analysis.

### get_yoy_comparison
Compare a month to same month prior year.

---

## Best Practices

1. **Always check data coverage first** before querying metrics
   ```
   get_data_coverage(seller_name="Brand Name")
   ```

2. **Use aligned dates** to avoid empty results or confusion

3. **Start with account-level** aggregation, then drill into parent/child

4. **For recent data**, remember:
   - Current week may be incomplete
   - Current month may be incomplete
   - Ads data may lag 1-2 days behind business report

5. **When comparing periods**, ensure both periods are complete

---

## Metric Presets

Quick access to grouped metrics:

| Preset | Metrics Included |
|--------|------------------|
| `sales_overview` | total_sales, units, organic_sales, organic_pct, sessions, cvr_pct |
| `traffic` | sessions, mobile/desktop sessions, page_views, cvr_pct, buy_box_pct |
| `advertising` | ad_spend, ad_sales, roas, acos_pct, tacos_pct, impressions, clicks, ctr_pct |
| `efficiency` | roas, acos_pct, tacos_pct, cvr_pct, cpc, avg_price |

---

## Troubleshooting

### Empty results?
1. Check date range includes period start dates
2. Verify seller name is exact (use `list_sellers` to confirm)
3. Confirm data exists for that period (use `get_data_coverage`)

### Unexpected values?
1. Check aggregation level (account vs parent vs child)
2. Verify granularity matches your analysis needs
3. Remember: rates (CVR, ACOS) are calculated, not summed

---

*Last Updated: 2026-01-29*
