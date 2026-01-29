# System Overview

## What is This System?

This is a file-based Amazon Brand Management System powered by Claude Code. It helps account managers:

1. **Log weekly actions and observations** on client accounts
2. **Onboard new brands** with structured documentation
3. **Onboard new products** with detailed analysis
4. **Track plans of action** (POA) for accounts and products
5. **Query historical context** to understand what's happened
6. **Get real-time metrics** from seller analytics

## How It Works

```
Account Manager speaks/types naturally
        ↓
Claude detects intent and context
        ↓
Routes to appropriate workflow
        ↓
Claude structures it, asks clarifying questions
        ↓
Creates/updates markdown files
        ↓
AM commits and creates PR
        ↓
Manager reviews and approves
```

## Natural Language Interaction

You don't need specific commands or trigger phrases. Claude detects what you need through:

- **What you describe** (actions, questions, requests)
- **How you describe it** (past tense = logging, questions = querying)
- **What entities you mention** (known brands vs new brands)

### Examples of Natural Requests

| You Say | Claude Understands | Action |
|---------|-------------------|--------|
| "This week I adjusted bids for Acme" | Recording past activity | Creates log entry |
| "New client called Sunrise Foods" | New entity to set up | Starts brand onboarding |
| "What happened with inventory last month?" | Historical query | Searches and summarizes logs |
| "ACOS jumped, not sure why" | Analysis + uncertainty | Provides diagnostic framework |
| "How are sales this week?" | Real-time data need | Fetches current metrics |
| "Commit these changes" | Version control | Stages and commits files |

---

## File Structure

```
/brands/
  /{brand-name}/
    README.md                    # Brand overview & quick reference
    MEMORY.md                    # Rolling context (updated every 4-5 logs)
    /onboarding/
      account.md                 # Account-level onboarding doc
      checklist.md               # Onboarding checklist
      /reports/                  # All analysis reports
      /products/                 # Per-product onboarding
    /logs/                       # All log entries

/templates/                      # Templates for all doc types
/sops/                           # These instructions
/agents/                         # Agent system documentation
/knowledge/                      # Expert frameworks and learnings
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

1. **Detect your intent** from natural language
2. **Structure your input** into readable markdown
3. **Ask clarifying questions** as an Amazon marketing expert
4. **Help you recall context** from previous logs
5. **Ensure completeness** by prompting for missing details
6. **Maintain consistency** in formatting and metadata

### Clarifying Questions

Claude asks questions to make logs more useful in the future. Examples:

- *"You mentioned adjusting bids - which campaigns and what were the changes?"*
- *"You said sales dropped - do you know the percentage or have a hypothesis?"*
- *"You referenced 'that keyword issue' - can you specify which keywords?"*

You can always say **"skip questions, log as is"** to proceed without clarification.

## Context & Memory

### How Claude Remembers

Claude doesn't have persistent memory across sessions. Each time you start:

1. Claude reads the brand's MEMORY.md for current context
2. Claude reads recent log files (last 2-3 entries)
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
