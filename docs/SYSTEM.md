# Amazon Brand Management System

## Overview

This is a **file-based knowledge management system** powered by Claude Code, designed to help account managers (AMs) at an Amazon marketing agency manage their client brands. The system transforms natural language input into structured, queryable markdown documentation.

### What This System Does

- **Logs weekly activities** - Transforms voice/text notes into structured log entries
- **Onboards brands** - Guides new client setup with templates and checklists
- **Onboards products** - Creates detailed product documentation with competitive research
- **Tracks context** - Maintains rolling memory of account state and decisions
- **Queries history** - Searches and summarizes past activities
- **Analyzes data** - Interprets reports and generates insights
- **Manages version control** - Handles git operations transparently

### Core Philosophy

**Need-based activation** - Users don't need specific commands or trigger phrases. Claude detects intent through natural language signals and routes to the appropriate workflow automatically.

---

## System Goals

| Goal | Description |
|------|-------------|
| **Reduce friction** | AMs speak naturally; the system structures |
| **Preserve knowledge** | Decisions, patterns, and learnings are captured |
| **Enable querying** | Historical context is searchable and summarizable |
| **Maintain consistency** | Templates enforce structure across all brands |
| **Support collaboration** | Multiple AMs can work with proper git workflow |

---

## Agent Architecture

The system uses a **multi-agent architecture** with autonomous routing. Rather than keyword matching, Claude analyzes context signals to determine what the user needs.

### How Routing Works

```
User Input → Intent Classifier → Context Analyzer → Signal Detector → Agent Selector
```

1. **Intent Classifier** - Categorizes the request type (record, create, understand, etc.)
2. **Context Analyzer** - Loads relevant brand memory and recent logs
3. **Signal Detector** - Identifies linguistic cues (tense, temporal references, question types)
4. **Agent Selector** - Routes to appropriate specialized agent

### Confidence-Based Routing

| Confidence | Behavior |
|------------|----------|
| **>90%** | Route directly to agent |
| **70-90%** | Route with clarification note |
| **<70%** | Ask single focused question first |

### Specialized Agents

| Agent | Purpose | Activation Signals |
|-------|---------|-------------------|
| **Router** | Entry point; coordinates all others | Every request |
| **Logging** | Structures activity into logs | Past tense verbs, temporal refs ("this week", "yesterday") |
| **Onboarding** | Guides brand/product setup | Unknown entity + creation language ("new client") |
| **Analysis** | Interprets data, generates reports | Questions about metrics, "analyze", "run report" |
| **Research** | Competitive analysis via web | Competitor focus, market analysis requests |
| **Memory** | Maintains rolling context | Auto-trigger after 4+ logs |
| **Query** | Searches historical information | "show me", "what happened", historical refs |
| **Knowledge** | Retrieves expert frameworks | Uncertainty ("not sure", "how should I") |
| **Data** | Real-time seller analytics | Current metrics, "how are sales" |
| **Git** | Version control operations | "commit", "push", "create PR" |

### Intent Categories

| Intent | Maps To | Example Trigger |
|--------|---------|-----------------|
| `RECORD` | Logging Agent | "This week I adjusted bids..." |
| `CREATE` | Onboarding Agent | "New client called Sunrise Foods" |
| `UNDERSTAND` | Analysis Agent | "What does this ACOS trend mean?" |
| `RECALL` | Query Agent | "Show me last few logs for Acme" |
| `RESEARCH` | Research Agent | "How are competitors priced?" |
| `MAINTAIN` | Memory Agent | [Auto after 4+ logs] |
| `MEASURE` | Data Agent | "How are sales this week?" |
| `LEARN` | Knowledge Agent | "Not sure why ACOS jumped" |
| `PUBLISH` | Git Agent | "Commit these changes" |

### Parallel Execution

Some agents run alongside primary agents without blocking:

| Primary | Parallel | When |
|---------|----------|------|
| Logging | Knowledge | Uncertainty detected in input |
| Logging | Data | Metrics need validation |
| Analysis | Knowledge | Generating insights |
| Analysis | Data | Real-time validation needed |

---

## Standard Operating Procedures (SOPs)

The system is governed by 7 SOPs in `/sops/`:

### 1. System Overview (`system-overview.md`)
- How intent detection works
- File structure organization
- Git workflow basics
- Multi-user collaboration

### 2. Weekly Logging (`weekly-logging.md`)
- When to log (weekly minimum, after major actions/discoveries)
- Log entry structure and types
- Clarifying question templates by topic
- Voice input → structured output examples

### 3. Brand Onboarding (`brand-onboarding.md`)
- 7-step onboarding process
- Interactive information gathering
- File creation and registration
- Checklist tracking (Access, Data, Understanding, Operations, Analysis, Planning)

### 4. Product Onboarding (`product-onboarding.md`)
- 8-step product documentation
- Performance analysis sections
- Competitive research integration
- Product-specific POA generation

### 5. Querying History (`querying-history.md`)
- Query types (metrics, timeline, topic, cross-brand)
- Source citation format
- Drill-down options

### 6. Git Workflow (`git-workflow.md`)
- Multi-user collaboration process
- Branch naming: `yourname/brand-task-date`
- Pull → Branch → Work → Commit → Push → PR flow

### 7. Seller Analytics Data (`seller-analytics-data.md`)
- Date alignment rules
- Granularity specifications
- Data reconciliation warnings

---

## Repository Structure

```
/
├── CLAUDE.md                     # AI instructions
├── accounts.md                   # Central registry of brands/AMs
│
├── /brands/
│   └── {brand-slug}/             # e.g., "acme-kitchenware"
│       ├── README.md             # Brand overview (static)
│       ├── MEMORY.md             # Rolling context (updated every 4-5 logs)
│       ├── /onboarding/
│       │   ├── account.md        # Full onboarding documentation
│       │   ├── checklist.md      # Onboarding progress
│       │   ├── /reports/         # Analysis reports
│       │   └── /products/        # Per-product docs (ASIN.md)
│       └── /logs/                # YYYY-MM-DD-author-type.md
│
├── /templates/                   # All document templates
│   ├── log-entry.md
│   ├── brand-readme.md
│   ├── brand-memory.md
│   ├── account-onboarding.md
│   ├── onboarding-checklist.md
│   ├── product-onboarding.md
│   └── /reports/                 # 6 report templates
│
├── /agents/                      # Agent specifications
│   ├── ARCHITECTURE.md           # Detailed architecture
│   ├── TRIGGERS.md               # Activation criteria
│   └── {agent-name}.md           # Individual agent specs
│
├── /knowledge/                   # Expert frameworks
│   ├── README.md                 # Index by topic/situation
│   ├── COMMON.md                 # Universal principles
│   └── {topic}-{date}-{source}.md
│
├── /sops/                        # Standard operating procedures
│   └── *.md                      # 7 SOPs as listed above
│
└── /docs/                        # System documentation
    └── SYSTEM.md                 # This file
```

---

## Key Concepts

### Log Types

| Type | Purpose | When to Use |
|------|---------|-------------|
| `weekly` | Regular summary | Weekly round-up of activities |
| `poa` | Strategic planning | Plans of action, quarterly planning |
| `action` | Single action | Significant standalone action |
| `observation` | Finding | Discovery or insight (no action yet) |

### Memory System

`MEMORY.md` files maintain rolling context for each brand:

- **Current State** - Focus, challenges, account health
- **Active Goals** - Targets with status
- **Key Decisions** - Important decisions with rationale
- **Patterns & Learnings** - Insights that inform future
- **Recent Activity** - Last 30 days + upcoming items
- **Flags & Concerns** - Items needing attention

Memory updates automatically after every 4-5 logs.

### Context Hierarchy

When responding, agents load context in this order:

1. `MEMORY.md` - Current state (always first)
2. Recent logs - Last 2-5 entries
3. `README.md` - Static brand info (if needed)
4. Onboarding docs - Deep context (for specific queries)
5. Research briefs - Competitive intelligence (as relevant)

---

## File Naming Conventions

| Entity | Convention | Example |
|--------|------------|---------|
| Brand folders | Lowercase, hyphenated | `acme-kitchenware` |
| Log files | `YYYY-MM-DD-{author}-{type}.md` | `2024-01-15-john-weekly.md` |
| Product files | ASIN as filename | `B00ACME001.md` |
| Knowledge docs | `{topic}-{date}-{source}.md` | `amazon-ppc-iteration-29jan-training.md` |

---

## How It Works: Example Flow

**User says:** "This week I adjusted bids on the auto campaign by 20%, ACOS is down to 32%"

1. **Router detects RECORD intent**
   - Past tense: "adjusted"
   - Temporal reference: "this week"
   - Metrics mentioned: "20%", "32%"

2. **Routes to Logging Agent** (95% confidence)

3. **Logging Agent loads context**
   - Reads MEMORY.md for current focus
   - Reads last 2 logs for recent activity

4. **Asks clarifying questions**
   - "Which campaign specifically?"
   - "What was ACOS before?"
   - "Any other changes this week?"

5. **Structures into log entry**
   - Creates `2024-01-15-john-weekly.md`
   - Adds proper metadata and sections

6. **Shows draft for confirmation**

7. **Saves file and updates memory if needed**

8. **Offers git commit**

---

## Knowledge Base

The `/knowledge/` folder contains expert frameworks extracted from team training and calls:

**Topics Covered:**
- Amazon Platform Fundamentals (SKU, ASIN, seller types)
- Advertising & PPC (ACOS, campaigns, match types, iteration)
- Account Analysis (CVR, reviews, competitive pricing)
- Data Interpretation (SQP, reports, ICAP funnel)
- Listings & Content (SEO, indexing, A+ content)
- Inventory & Operations (FBA, fees, cost structure)
- Strategy & Planning (Portfolio strategy, budgets)

The Knowledge Agent surfaces relevant frameworks when uncertainty is detected.

---

## Global Registry

`accounts.md` maintains a central index:

- **Account Managers** - Name, slug, email, active brands
- **Brands** - Name, slug, assigned AM, status, start date
- **Quick lookups** - By AM name, by status

Used for name validation, auto-completion, cross-brand queries, and onboarding.

---

## Design Principles

1. **Intent-based, not keyword-based** - Detect what you need, not specific phrases
2. **Minimal friction** - Natural language in, structured docs out
3. **Rolling context** - MEMORY.md keeps accounts in sync
4. **Parallel agents** - Knowledge runs alongside without blocking
5. **Confidence scoring** - Route directly if confident, ask if not
6. **Expert context** - Knowledge base informs interpretation
7. **Version control** - Git integration is automatic and transparent

---

## Getting Started

### For New Users

1. Read `CLAUDE.md` for interaction patterns
2. Review relevant SOPs in `/sops/`
3. Check `accounts.md` for existing brands
4. Start talking naturally - the system will route appropriately

### For New Brands

Simply say "New client called [Brand Name]" - the onboarding flow will guide you through setup.

### For Daily Logging

Describe what you did: "This week I did X, Y, and Z" - the logging flow handles structuring.

---

## Technical Details

For deeper technical documentation:
- `/agents/ARCHITECTURE.md` - Full routing logic and agent specifications
- `/agents/TRIGGERS.md` - Complete activation criteria
- Individual agent files in `/agents/` - Per-agent implementation details
