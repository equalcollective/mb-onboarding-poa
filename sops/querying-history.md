# Querying History SOP

## Purpose

Learn how to ask Claude about past logs, find specific information, and understand what's happened on an account.

## How Claude Detects Query Intent

Claude recognizes you want historical information when:

- You ask questions about past activity ("what happened", "when did")
- You use retrieval language ("show me", "find")
- You reference historical periods ("last month", "previous")
- You want to recall decisions or context

**Examples that trigger historical search:**
- "Show me recent logs for Acme"
- "What happened with inventory last month?"
- "When did we change the bid strategy?"
- "Find any logs mentioning competitor X"
- "What did we decide about pricing?"

## How Context Works

Each Claude session starts fresh. When you ask about history, Claude will:

1. Read the brand's MEMORY.md (current context summary)
2. Search relevant log files
3. Check onboarding docs if needed
4. Summarize findings with source citations

**You don't need to tell Claude where to look** - just ask your question.

---

## Types of Queries

### View Recent Activity

Just ask about recent events:

- "What's happened with Acme lately?"
- "Recent activity for KetoGoods"
- "What did we work on last week for Sunrise?"

### Find Specific Information

Ask about specific topics or events:

- "What advertising changes did we make for Acme?"
- "Find any mentions of B00ABC123"
- "When did we last update the listing?"

### Review Plans of Action

Ask about strategic plans:

- "What's the current POA for Acme?"
- "What strategic decisions have we made?"
- "What's the plan for Q1?"

### Check Onboarding Status

Ask about onboarding progress:

- "What's the onboarding status for Sunrise?"
- "Which reports are still pending?"
- "What's left on the checklist?"

### Understand Context Before Logging

Before logging, ask about previous state:

- "What did I log last time for Acme?"
- "What were the open items from last week?"
- "Catch me up on KetoGoods"

### Cross-Brand Queries

Ask about multiple brands:

- "Show me all my recent logs"
- "Which brands have inventory issues?"
- "What accounts need attention?"

---

## Query Examples with Responses

### Example 1: Recent Activity

**Query:**
> "What's happened with Acme this week?"

**Claude's Response:**
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
> "What advertising changes have we made on Acme this month?"

**Claude's Response:**
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
> "I'm about to log for Acme. What did I log last time?"

**Claude's Response:**
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

## Tips for Effective Queries

### Be Specific About Brand
Claude can handle multiple brands, but specifying helps:
- "Show me logs for Acme Widgets"
- "What happened with KetoGoods?"

### Specify Time Range if Needed
For focused results:
- "What happened in December?"
- "Last 3 logs for Acme"
- "Activity since January 1st"

### Mention ASINs When Relevant
For product-specific queries:
- "Activity related to B00ABC123"
- "What have we done for the Widget Pro?"

### Ask Follow-Up Questions
After Claude summarizes:
- "Show me the full log from Jan 15"
- "What were the specific keywords we paused?"
- "Was the inventory issue resolved?"

---

## What Claude Searches

| Query Type | Primary Source | Secondary Source |
|------------|----------------|------------------|
| Current state | MEMORY.md | Recent 2 logs |
| Historical action | Logs (filtered) | MEMORY decisions |
| Performance data | Logs | Reports |
| Brand overview | README.md | MEMORY.md |
| Decision rationale | Logs | MEMORY decisions |
| Onboarding status | checklist.md | account.md |
| Competitive intel | Research briefs | Logs |
| Product details | Product docs | Research briefs |

---

## No Results Found

If Claude can't find information:

```
I couldn't find any information about {topic} for {brand}.

This could mean:
- It hasn't been logged yet
- It was logged under a different topic
- The date range doesn't include it

Would you like me to:
1. Search a wider date range?
2. Check a different topic?
3. Look in onboarding docs?
```
