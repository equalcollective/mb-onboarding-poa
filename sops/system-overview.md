# System Overview

## What is This System?

This is a file-based Amazon Brand Management System powered by Claude Code. It helps account managers:

1. **Log weekly actions and observations** on client accounts
2. **Onboard new brands** with structured documentation
3. **Onboard new products** with detailed analysis
4. **Track plans of action** (POA) for accounts and products
5. **Query historical context** to understand what's happened

## How It Works

```
Account Manager speaks/types
        ↓
Claude Code receives input
        ↓
Claude structures it, asks clarifying questions
        ↓
Creates/updates markdown files
        ↓
AM commits and creates PR
        ↓
Manager reviews and approves
```

## File Structure

```
/brands/
  /{brand-name}/
    README.md                    # Brand overview & quick reference
    /onboarding/
      account.md                 # Account-level onboarding doc
      checklist.md               # Onboarding checklist
      /reports/                  # All analysis reports
      /products/                 # Per-product onboarding
    /logs/                       # All log entries

/templates/                      # Templates for all doc types
/sops/                           # These instructions
```

## Key Concepts

### Log Entry Types

| Type | Tag | Use For |
|------|-----|---------|
| `weekly` | Regular updates | Weekly actions, observations, routine work |
| `poa` | Strategic input | Plan of action, strategic decisions |
| `action` | Specific action | Single significant action taken |
| `observation` | Insight/finding | Something noticed that needs recording |

### Metadata in Every Log

Every log entry contains:
- **Brand** - Which brand this relates to
- **Date** - When it was logged
- **Logged By** - Who logged it
- **Type** - Log type (weekly, poa, action, observation)
- **Products** - ASINs mentioned, or "account-level"

### How Claude Helps

When you provide input, Claude will:

1. **Structure your input** into readable markdown
2. **Ask clarifying questions** as an Amazon marketing expert
3. **Help you recall context** from previous logs
4. **Ensure completeness** by prompting for missing details
5. **Maintain consistency** in formatting and metadata

### Clarifying Questions

Claude asks questions to make logs more useful in the future. Examples:

- *"You mentioned adjusting bids - which campaigns and what were the changes?"*
- *"You said sales dropped - do you know the percentage or have a hypothesis?"*
- *"You referenced 'that keyword issue' - can you specify which keywords?"*

You can always say **"skip questions, log as is"** to proceed without clarification.

## Context & Memory

### How Claude Remembers

Claude doesn't have persistent memory across sessions. Each time you start:

1. Claude reads the brand's README for context
2. Claude reads recent log files (last 5-10 entries)
3. Claude can read onboarding docs if needed

### Best Practices for Context

- **Reference ASINs** not just product names
- **Be specific** about campaigns, keywords, numbers
- **Mention dates** when referencing past events
- **Link related logs** when continuing previous work

## Git Workflow (Multi-User)

```
1. Pull latest main
2. Create feature branch: yourname/brand-update-date
3. Make your changes
4. Commit with descriptive message
5. Push and create PR
6. Manager reviews and merges
```

See [Git Workflow SOP](./git-workflow.md) for details.
