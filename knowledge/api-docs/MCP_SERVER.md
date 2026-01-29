# MCP Server for Seller Analytics

This document explains how to use the Model Context Protocol (MCP) server to access the Seller Analytics API.

## Overview

The MCP server exposes the Seller Analytics API through the Model Context Protocol, allowing AI assistants (Claude Code, Claude Desktop, Cursor) to access your seller data directly without browser-based domain restrictions.

## Server URL

**Production URL** (always available):
```
https://mb-onboarding.replit.app/mcp/sse
```

**Development URL** (only when Replit workspace is running):
```
https://296b25f1-da65-430c-a0b7-cdd4893e2007-00-1tn8mrrizmps2.pike.replit.dev/mcp/sse
```

The MCP server is integrated into the main application on port 5000, so it's accessible through the same domain as the dashboard.

## Available Tools

The MCP server provides these tools:

| Tool | Description |
|------|-------------|
| `list_sellers` | List all available Amazon sellers |
| `get_seller_asins` | Get ASIN hierarchy for a seller |
| `get_metrics` | Get detailed metrics with filtering |
| `get_cumulative_metrics` | Get aggregated totals |
| `get_pivot_table` | Get pivot table data |
| `get_yoy_comparison` | Year-over-Year comparison |
| `get_data_coverage` | Check data availability |
| `get_data_gaps` | Detect missing data periods |
| `get_filter_options` | List available filters |

## Connecting from Claude Code (CLI)

Run this command in your terminal:

```bash
claude mcp add --transport sse seller-analytics https://mb-onboarding.replit.app/mcp/sse
```

Verify the connection:
```bash
claude mcp list
```

## Connecting from Claude Desktop

Claude Desktop requires `mcp-remote` as a bridge for remote SSE servers.

Add this configuration to your Claude Desktop config file:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "seller-analytics": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mb-onboarding.replit.app/mcp/sse"
      ]
    }
  }
}
```

**Requirements**: Node.js must be installed on your computer.

After saving the config, restart Claude Desktop completely.

## Connecting from Cursor

Add to your `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "seller-analytics": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mb-onboarding.replit.app/mcp/sse"
      ]
    }
  }
}
```

## Tool Usage Examples

### List Sellers
```
list_sellers()
```
Returns all available sellers with their IDs and ASIN counts.

### Get Metrics
```
get_metrics(
  seller_name="YourSeller",
  start_date="2025-01-01",
  end_date="2025-01-31",
  aggregation_level="parent",
  granularity="weekly"
)
```

### Get Year-over-Year Comparison
```
get_yoy_comparison(
  seller_name="YourSeller",
  month="2025-01-01",
  aggregation_level="account"
)
```

## Data Available

- **Sales Metrics**: total_sales, total_units, avg_price
- **Traffic Metrics**: sessions, page_views, page_views_per_session
- **Conversion Metrics**: cvr_pct, unit_session_pct, total_order_items
- **Advertising Metrics**: ad_spend, ad_sales, roas, acos_pct, impressions, clicks, ctr_pct, cpc

## Aggregation Levels

- `account` - Single row per period for entire seller
- `parent` - Group by product/parent ASIN
- `child` - Group by individual ASIN variant

## Troubleshooting

### Connection Issues
- Production URL (`mb-onboarding.replit.app`) is always available
- Development URL requires the Replit workspace to be running
- For Claude Desktop, make sure Node.js is installed for `mcp-remote`

### No Data Returned
- Use `list_sellers()` first to confirm available sellers
- Use `get_data_coverage()` to check date ranges
- Ensure you're using the correct `seller_name` from the list

## Architecture Notes

The MCP server is integrated into the main FastAPI application rather than running on a separate port. This ensures:
- The MCP endpoints are accessible through the same domain as the dashboard
- No port configuration issues in deployed environments
- Simplified infrastructure with a single server process
