# Querying History SOP

## Purpose

Learn how to ask Claude about past logs, find specific information, and understand what's happened on an account.

## How Context Works

Each Claude session starts fresh. To get context, Claude will:

1. Read the brand's README (brand overview)
2. Read recent log files for that brand
3. Read relevant onboarding docs if needed

**You don't need to manually tell Claude to do this** - just ask your question and Claude will gather context.

---

## Common Queries

### View Recent Activity

> "Show me recent logs for [Brand Name]"

> "What happened on [Brand Name] in the last 2 weeks?"

> "Summarize recent activity for [Brand Name]"

### Find Specific Information

> "What did we do with advertising on [Brand Name] last month?"

> "Find any logs mentioning [ASIN] for [Brand Name]"

> "When did we last update the listing for [ASIN]?"

### Review Plans of Action

> "Show me the current POA for [Brand Name]"

> "What strategic decisions have we made for [Brand Name]?"

> "What's the plan for [ASIN]?"

### Check Onboarding Status

> "What's the onboarding status for [Brand Name]?"

> "Show me the checklist for [Brand Name]"

> "What reports are still pending for [Brand Name]?"

### Understand Context Before Logging

> "Before I log, show me what's happened on [Brand Name] recently"

> "What did [AM Name] last log for [Brand Name]?"

### Cross-Brand Queries

> "Show me all my recent logs across all brands"

> "Which brands have POAs that need review?"

---

## Query Examples with Responses

### Example 1: Recent Activity

**Query:**
> "What happened on Acme Widgets in the last week?"

**Claude's Process:**
1. Reads `brands/acme-widgets/README.md`
2. Lists and reads recent files in `brands/acme-widgets/logs/`
3. Summarizes findings

**Response:**
> "Based on the logs for Acme Widgets from the past week:
>
> **Jan 15 (John - Weekly):**
> - Reduced bids on auto campaign by 20% due to high ACOS
> - Flagged inventory issue on B00ABC123 (~10 days left)
> - Noted shipping damage reviews on B00XYZ789
>
> **Jan 12 (John - POA):**
> - Created Q1 advertising strategy
> - Set target ACOS at 30%
> - Prioritized 3 ASINs for optimization
>
> Would you like me to show the full details of any of these?"

### Example 2: Specific Topic

**Query:**
> "What advertising changes have we made on Acme Widgets this month?"

**Response:**
> "Advertising changes for Acme Widgets in January:
>
> - **Jan 15:** Reduced bids 20% on auto and brand defense campaigns (ACOS was 45%)
> - **Jan 10:** Paused 5 underperforming keywords in exact match campaign
> - **Jan 8:** Added 12 new keywords from SQP analysis to broad match
> - **Jan 5:** Increased daily budget from $50 to $75 on top performing campaign
>
> Current target ACOS: 30% (per Jan 12 POA)"

### Example 3: Before Logging

**Query:**
> "I'm about to log my weekly update for Acme Widgets. Show me what I logged last time."

**Response:**
> "Your last log for Acme Widgets was on Jan 15:
>
> **Actions Taken:**
> - Reduced bids on auto and brand defense campaigns by 20%
> - Contacted client about inventory reorder for B00ABC123
>
> **Open Next Steps (from that log):**
> - [ ] Analyze SQP data for new keyword opportunities
> - [ ] Monitor ACOS after bid changes (check in 3-5 days)
> - [ ] Investigate shipping damage issue for B00XYZ789
> - [ ] Follow up with client on restock timeline
>
> Ready to log your update? I'll check if these items were addressed."

---

## Tips for Better Queries

### Be Specific About Brand
- ✅ "Show me logs for Acme Widgets"
- ❌ "Show me the logs" (which brand?)

### Specify Time Range if Needed
- ✅ "What happened in December?"
- ✅ "Last 3 logs for..."
- ❌ "What happened before?" (when?)

### Mention ASINs When Relevant
- ✅ "Activity related to B00ABC123"
- ❌ "Activity on the main product"

### Ask Follow-Up Questions
After Claude summarizes:
- "Show me the full log from Jan 15"
- "What were the specific keywords we paused?"
- "Was the inventory issue resolved?"

---

## Quick Commands

| Need | Say |
|------|-----|
| Recent brand activity | "Show me recent logs for [Brand]" |
| Specific topic | "Find [topic] activity for [Brand]" |
| Current POA | "What's the plan for [Brand]?" |
| Checklist status | "Show checklist for [Brand]" |
| Before logging | "What did I last log for [Brand]?" |
| Full log details | "Show the full log from [date]" |
