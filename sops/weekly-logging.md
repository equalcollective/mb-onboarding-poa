# Weekly Logging SOP

## Purpose

Document weekly actions, observations, and next steps for each brand in a structured, queryable format.

## When to Log

- **Weekly** - At minimum, log once per week per active brand
- **After significant actions** - Major campaign changes, listing updates, etc.
- **After discoveries** - Important findings from data analysis
- **Strategic decisions** - Use `type: poa` for plans of action

## How to Log

### Step 1: Tell Claude What You Want to Log

Start by telling Claude:

> "I want to log my weekly update for [Brand Name]"

Or if you have voice-to-text transcription ready:

> "Here's my weekly update for [Brand Name]: [paste transcription]"

### Step 2: Provide Your Input

Speak or type your update. Include whatever comes to mind:
- What you did this week
- What you observed
- What the data showed
- What needs to happen next
- Any concerns or blockers

**Don't worry about structure** - Claude will organize it.

### Step 3: Answer Clarifying Questions (Optional)

Claude will ask questions like:
- *"You mentioned adjusting bids - which campaigns specifically?"*
- *"What was the result of that change?"*
- *"Can you provide the ASIN for that product?"*

You can:
- **Answer** to add more detail
- **Say "skip"** to proceed without answering
- **Say "log as is"** to finalize immediately

### Step 4: Review and Confirm

Claude will show you the structured entry. Review and confirm.

### Step 5: Commit and PR

After confirmation:
```bash
git add .
git commit -m "Add weekly log for [Brand] - [Date]"
git push origin your-branch
# Create PR for review
```

---

## Log Entry Structure

Every log entry follows this structure:

```markdown
# Log Entry

---
**Brand:** Brand Name
**Date:** YYYY-MM-DD
**Logged By:** Your Name
**Type:** weekly
**Products:** B00XXXXXX, B00YYYYYY (or "account-level")
---

## Summary
One-paragraph summary of the week.

## Details
Detailed information organized by topic.

## Actions Taken
- Specific actions completed this week

## Observations
- Things noticed, data insights, concerns

## Next Steps
- What needs to happen next

## Questions / Blockers
- Any open questions or blockers
```

---

## Example Input → Structured Output

### Raw Input (Voice-to-Text)

> "Okay so this week for Acme Brand I looked at the ads and the ACOS was really high so I reduced bids on the main campaign by about 20%. Also the main product B00ABC123 was running low on stock so I told the client to reorder. I also noticed that the listing for the new product has some bad reviews coming in about shipping damage, might need to look into that. Next week I want to focus on the SQP data and see if we can find new keywords."

### Clarifying Questions from Claude

1. *"Which campaign did you reduce bids on? (name or type)"*
   > "The main auto campaign and the brand defense campaign"

2. *"What was the ACOS before and after the change?"*
   > "It was around 45%, want to get it to 30%"

3. *"For the stock issue, how many days of inventory are left?"*
   > "About 10 days I think, skip this one"

4. *"The new product with shipping damage reviews - what's the ASIN?"*
   > "B00XYZ789"

### Structured Output

```markdown
# Log Entry

---
**Brand:** Acme Brand
**Date:** 2024-01-15
**Logged By:** John
**Type:** weekly
**Products:** B00ABC123, B00XYZ789
---

## Summary
Addressed high ACOS by reducing bids on auto and brand defense campaigns. Flagged low inventory on main product. Identified quality concern with new product reviews.

## Details

### Advertising
- Reviewed ad performance; ACOS at ~45%, target is 30%
- Reduced bids by 20% on:
  - Main auto campaign
  - Brand defense campaign

### Inventory
- Main product (B00ABC123) running low on stock (~10 days)
- Notified client to reorder

### Listing Quality
- New product (B00XYZ789) receiving negative reviews about shipping damage
- Needs investigation into packaging/fulfillment

## Actions Taken
- Reduced bids on auto and brand defense campaigns by 20%
- Contacted client about inventory reorder for B00ABC123

## Observations
- ACOS trending high across campaigns
- New product launch facing shipping damage complaints

## Next Steps
- [ ] Analyze SQP data for new keyword opportunities
- [ ] Monitor ACOS after bid changes (check in 3-5 days)
- [ ] Investigate shipping damage issue for B00XYZ789
- [ ] Follow up with client on restock timeline

## Questions / Blockers
- Need to determine if shipping damage is FBA issue or product packaging issue
```

---

## Tips for Better Logs

### Be Specific
- ❌ "Adjusted the ads"
- ✅ "Reduced bids 20% on auto campaign, paused 3 low-performing keywords"

### Use ASINs
- ❌ "The main product"
- ✅ "B00ABC123 (Widget Pro)"

### Include Numbers
- ❌ "Sales were down"
- ✅ "Sales down 15% WoW, from $5,000 to $4,250"

### Reference Dates
- ❌ "Last week I..."
- ✅ "On Jan 8, I..."

---

## Logging a Plan of Action (POA)

For strategic inputs and plans, use:

> "I want to log a plan of action for [Brand Name]"

Claude will prompt for:
- Strategic context
- Goals
- Specific action items with owners/timelines
- Success metrics

POA entries get `type: poa` tag for easy filtering.
