# Sub-Agent Architecture Design

This document defines the sub-agent architecture for the Amazon Brand Management System.

---

## Overview

The current system uses a **single-agent design** where one Claude instance handles all workflows. This architecture introduces **specialized sub-agents** that handle specific domains, coordinated by a **Router Agent**.

### Why Sub-Agents?

| Benefit | Description |
|---------|-------------|
| **Focused context** | Each agent loads only what it needs |
| **Parallel execution** | Independent tasks can run simultaneously |
| **Specialized expertise** | Agents can be tuned for specific domains |
| **Clearer responsibility** | Easier to debug and improve individual workflows |
| **Scalability** | Add new agents without modifying existing ones |

---

## Architecture Diagram

```
                    ┌─────────────────────┐
                    │    User Request     │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    ROUTER AGENT     │
                    │  (Coordinator)      │
                    └──────────┬──────────┘
                               │
         ┌─────────┬─────────┬─┴───────┬─────────┬─────────┐
         │         │         │         │         │         │
         ▼         ▼         ▼         ▼         ▼         ▼
    ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐
    │ LOGGING │ │ONBOARD- │ │ANALYSIS │ │ MEMORY  │ │  QUERY  │ │   GIT   │
    │  AGENT  │ │ING AGENT│ │  AGENT  │ │  AGENT  │ │  AGENT  │ │  AGENT  │
    └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘
         │         │         │         │         │         │
         └─────────┴─────────┴────┬────┴─────────┴─────────┘
                                  │
                                  ▼
                    ┌─────────────────────┐
                    │   /brands/{slug}/   │
                    │   File System       │
                    └─────────────────────┘
```

---

## Global Registry

### Accounts Registry (`/accounts.md`)

A centralized index of all brands and account managers for quick lookups.

```markdown
## Account Managers
| Name | Slug | Email | Active Brands |

## Brands
| Brand | Slug | Account Manager | Status | Start Date |
```

**Purpose:**
- **Name validation** - Confirm brand/AM names are correct
- **Auto-completion** - Suggest brand slugs from partial input
- **Cross-brand queries** - "Show all brands for John"
- **Routing** - Know which AM owns which brand
- **Onboarding** - Pre-populate AM info for new brands

**Sync Rules:**
- Source of truth: Individual `brands/{slug}/README.md` files
- Registry is updated when:
  - New brand onboarded → add to registry
  - Brand status changes → update registry
  - AM reassigned → update both tables

**Agent Access:**

| Agent | Reads Registry | Writes Registry |
|-------|----------------|-----------------|
| Router | Yes (validation) | No |
| Onboarding | Yes (AM lookup) | Yes (new brand) |
| Query | Yes (cross-brand) | No |
| Others | No | No |

---

### Knowledge Base (`/knowledge/`)

Expert knowledge from team calls, training, and experience. The "how to think" library.

```
/knowledge/
├── README.md                    # Index by topic + situation
├── {topic}-{date}-{source}.md   # Individual knowledge docs
└── ...
```

**Structure of Knowledge Docs:**
```markdown
# {Title}
**Source:** {Call/Training}  |  **Date:** {YYYY-MM-DD}  |  **Tags:** {tags}

## Key Insights
### Insight 1: {Title}
{Explanation + when to apply + example}

## Frameworks
{Step-by-step thinking processes}

## Questions to Ask
## Red Flags to Watch For
## Raw Notes (optional)
```

**Index Structure (`/knowledge/README.md`):**
- **By Topic:** Advertising, Analysis, Listings, Inventory, Strategy
- **By Situation:** "I'm looking at ad data...", "I need to form a hypothesis..."

**Agent Access:**

| Agent | When to Reference | How |
|-------|-------------------|-----|
| **Logging** | AM mentions metrics/analysis | Surface relevant framework |
| **Analysis** | Generating insights | Apply documented patterns |
| **Onboarding** | Running reports | Reference analysis frameworks |
| **Query** | "How should I think about X?" | Search knowledge index |

**Retrieval Logic:**
```python
def get_relevant_knowledge(context):
    # 1. Extract topics from context
    topics = extract_topics(context)  # e.g., ["ads", "ACOS", "hypothesis"]

    # 2. Read knowledge index
    index = read_file("/knowledge/README.md")

    # 3. Match topics to tagged documents
    matches = match_by_tags(index, topics)

    # 4. Return ranked list of relevant docs
    return rank_by_relevance(matches, context)
```

**When to Surface Knowledge:**
- AM mentions they're "looking at" or "analyzing" something
- During report generation (Analysis Agent)
- When forming recommendations
- Explicit "how do I think about X?" questions

---

## Agent Definitions

### 1. Router Agent (Coordinator)

**Purpose:** Interpret user requests and delegate to appropriate sub-agents.

**Responsibilities:**
- Parse user intent from trigger phrases
- Extract parameters (brand name, ASIN, date range, etc.)
- Validate brand/product exists before delegating
- Route to correct sub-agent(s)
- Aggregate results from multiple agents
- Handle errors and fallbacks

**Trigger Phrase Mapping:**

| Pattern | Route To |
|---------|----------|
| "Log weekly...", "Weekly log...", "I want to log..." | Logging Agent |
| "Onboard new brand...", "New client..." | Onboarding Agent (brand mode) |
| "Onboard product...", "Add product..." | Onboarding Agent (product mode) |
| "Analyze...", "Run report...", "What does the data show..." | Analysis Agent |
| "Update memory...", "What should we remember..." | Memory Agent |
| "Show logs...", "What happened...", "History of..." | Query Agent |
| "Commit...", "Push...", "Create PR..." | Git Agent |
| "Update checklist...", "Mark complete..." | Document Agent (or inline) |

**Context Required:** Minimal - just needs to understand request and validate paths exist.

---

### 2. Logging Agent

**Purpose:** Structure unstructured input into formatted log entries.

**File:** `agents/logging-agent.md`

**Responsibilities:**
- Accept voice transcription or typed notes
- Ask clarifying questions (2-4 max)
- Structure into log template
- Determine log type (weekly/poa/action/observation)
- Generate proper filename
- Request confirmation before save

**Context Contract:**

| Reads | Writes |
|-------|--------|
| `brands/{brand}/MEMORY.md` | `brands/{brand}/logs/YYYY-MM-DD-{author}-{type}.md` |
| Last 2-3 logs from `brands/{brand}/logs/` | |
| `templates/log-entry.md` | |

**Inputs:**
```yaml
brand: string          # Brand slug
author: string         # AM name
raw_input: string      # Unstructured user input
skip_questions: bool   # Optional - skip clarifying questions
```

**Outputs:**
```yaml
log_file: string       # Path to created log
log_content: string    # Full log content
needs_memory_update: bool  # True if 4+ logs since last MEMORY update
```

**Handoff Triggers:**
- After save → notify Memory Agent if `needs_memory_update`
- After save → notify Git Agent with file path

---

### 3. Onboarding Agent

**Purpose:** Guide brand and product onboarding workflows.

**File:** `agents/onboarding-agent.md`

**Responsibilities:**
- Create folder structures
- Gather information interactively
- Create README, MEMORY, checklist files
- Guide through checklist sections
- Track onboarding progress
- Coordinate with Analysis Agent for reports

**Context Contract:**

| Mode | Reads | Writes |
|------|-------|--------|
| Brand | `templates/brand-*.md`, `templates/onboarding-*.md` | `brands/{brand}/README.md`, `MEMORY.md`, `onboarding/*` |
| Product | `brands/{brand}/README.md`, `templates/product-onboarding.md` | `brands/{brand}/onboarding/products/{asin}.md` |

**Inputs (Brand Mode):**
```yaml
mode: "brand"
brand_name: string
account_manager: string
```

**Inputs (Product Mode):**
```yaml
mode: "product"
brand: string
asin: string
```

**Outputs:**
```yaml
files_created: list[string]  # Paths to all created files
checklist_status: object     # Current checklist state
next_steps: list[string]     # What to do next
```

**Handoff Triggers:**
- During report analysis phase → spawn Analysis Agent for each report type
- After initial POA → notify Logging Agent
- After any file creation → notify Git Agent

---

### 4. Analysis Agent

**Purpose:** Generate and interpret analysis reports.

**File:** `agents/analysis-agent.md`

**Responsibilities:**
- Create report documents from templates
- Guide data input for each report type
- Interpret data and generate insights
- Identify patterns and opportunities
- Suggest actions based on analysis

**Report Types:**
1. SQP Analysis (Search Query Performance)
2. Business Report Analysis
3. Ad Report Analysis
4. Competitor Analysis
5. Data Dive (third-party tools)
6. Inventory Analysis

**Context Contract:**

| Reads | Writes |
|-------|--------|
| `templates/reports/*.md` | `brands/{brand}/onboarding/reports/{report-type}.md` |
| `brands/{brand}/README.md` | |
| Existing reports (for cross-reference) | |

**Inputs:**
```yaml
brand: string
report_type: string    # sqp|business|ad|competitor|data-dive|inventory
raw_data: string       # Pasted data or description
```

**Outputs:**
```yaml
report_file: string    # Path to created report
key_insights: list[string]  # Top 3-5 insights
recommended_actions: list[string]  # Suggested next steps
```

**Specialization:** This agent should have deep Amazon marketing knowledge embedded in its instructions, including:
- How to interpret ACOS, ROAS, TACoS
- What good/bad metrics look like by category
- Common patterns in search term reports
- Competitive positioning frameworks

---

### 5. Memory Agent

**Purpose:** Maintain rolling context in MEMORY.md files.

**File:** `agents/memory-agent.md`

**Responsibilities:**
- Summarize recent logs into memory updates
- Identify key decisions worth preserving
- Track patterns and learnings
- Update goal progress
- Keep memory scannable (not bloated)

**Context Contract:**

| Reads | Writes |
|-------|--------|
| `brands/{brand}/MEMORY.md` | `brands/{brand}/MEMORY.md` |
| Recent 4-5 logs since last update | |
| `brands/{brand}/README.md` (for goal reference) | |

**Inputs:**
```yaml
brand: string
trigger: string        # "scheduled" | "significant_decision" | "manual"
recent_logs: list[string]  # Paths to logs to summarize
```

**Outputs:**
```yaml
memory_updated: bool
changes_made: list[string]  # Summary of what was added/changed
```

**Update Rules:**
- Automatically trigger after every 4-5 logs
- Immediately trigger after significant decisions
- Keep entries concise (1-2 lines each)
- Remove resolved items
- Update timestamps

---

### 6. Query Agent

**Purpose:** Search and summarize historical information.

**File:** `agents/query-agent.md`

**Responsibilities:**
- Interpret query intent
- Search across logs, memory, onboarding docs
- Summarize findings
- Provide source references (dates, authors)
- Offer drill-down options

**Context Contract:**

| Reads | Writes |
|-------|--------|
| `brands/{brand}/MEMORY.md` | None (read-only) |
| `brands/{brand}/logs/*` | |
| `brands/{brand}/README.md` | |
| `brands/{brand}/onboarding/*` | |

**Inputs:**
```yaml
brand: string          # Optional - can query across brands
query: string          # Natural language query
date_range: object     # Optional - {start, end}
topic_filter: string   # Optional - advertising|inventory|listings|etc
```

**Outputs:**
```yaml
answer: string         # Direct answer to query
sources: list[object]  # [{file, date, author, relevant_excerpt}]
related_queries: list[string]  # Suggested follow-up questions
```

**Query Types:**
- **Specific:** "What was the ACOS last week?"
- **Timeline:** "Show me all advertising changes this month"
- **Topic:** "What have we done about inventory?"
- **Cross-brand:** "Which brands have inventory issues?"

---

### 7. Git Agent

**Purpose:** Handle version control operations.

**File:** `agents/git-agent.md`

**Responsibilities:**
- Stage files
- Create commits with good messages
- Push to remote
- Create branches
- Handle merge conflicts (with guidance)
- Create PRs

**Context Contract:**

| Reads | Writes |
|-------|--------|
| Git status | Git operations |
| Recent commits (for message style) | |

**Inputs:**
```yaml
operation: string      # stage|commit|push|branch|pr
files: list[string]    # Files to operate on
message: string        # Optional commit/PR message
```

**Outputs:**
```yaml
success: bool
output: string         # Git command output
next_steps: list[string]  # Any follow-up needed
```

**Auto-Commit Rules:**
- After log creation: `"Add {type} log for {brand} - {date}"`
- After onboarding file: `"Create {file} for {brand} onboarding"`
- After memory update: `"Update {brand} memory with recent activity"`

---

## Routing Logic

### Router Decision Tree

```
User Request
    │
    ├─ Contains "log" or "weekly" or "update for"?
    │   └─ YES → Logging Agent
    │
    ├─ Contains "onboard" or "new brand" or "new client"?
    │   └─ YES → Onboarding Agent (brand mode)
    │
    ├─ Contains "onboard product" or "add product" or "ASIN"?
    │   └─ YES → Onboarding Agent (product mode)
    │
    ├─ Contains "analyze" or "report" or "data"?
    │   └─ YES → Analysis Agent
    │
    ├─ Contains "memory" or "remember"?
    │   └─ YES → Memory Agent
    │
    ├─ Contains "show" or "history" or "what happened" or "find"?
    │   └─ YES → Query Agent
    │
    ├─ Contains "commit" or "push" or "git" or "PR"?
    │   └─ YES → Git Agent
    │
    └─ Ambiguous?
        └─ Ask clarifying question
```

### Brand Validation

Before routing to any agent that operates on a brand:

```python
def validate_brand(brand_slug):
    path = f"brands/{brand_slug}/"
    if not exists(path):
        return {
            "error": "brand_not_found",
            "suggestion": "Would you like to onboard this as a new brand?"
        }
    return {"valid": True, "path": path}
```

---

## Context Passing

### Context Hierarchy

Agents should load context in this order (most important first):

1. **MEMORY.md** - Current state, recent decisions (always read first)
2. **Recent logs** - Last 2-5 entries for detailed context
3. **README.md** - Static brand info (only if needed)
4. **Onboarding docs** - Deep context (only for specific queries)

### Context Minimization

Each agent should load **only what it needs**:

| Agent | Must Read | May Read | Never Reads |
|-------|-----------|----------|-------------|
| Logging | MEMORY, last 2-3 logs | README | All reports |
| Onboarding | Templates | README (if exists) | Logs |
| Analysis | README, relevant reports | MEMORY | All logs |
| Memory | MEMORY, recent logs | README | Reports |
| Query | MEMORY first, then targeted | Everything | - |
| Git | Nothing | Recent commits | - |

---

## Inter-Agent Communication

### Handoff Protocol

When one agent completes work that another agent needs to know about:

```yaml
handoff:
  from: logging-agent
  to: memory-agent
  trigger: log_count >= 4
  payload:
    brand: "acme-kitchenware"
    recent_logs:
      - "logs/2024-01-15-john-weekly.md"
      - "logs/2024-01-08-john-weekly.md"
      - "logs/2024-01-02-sarah-action.md"
      - "logs/2023-12-26-john-weekly.md"
    message: "4 logs since last memory update"
```

### Parallel Execution

Some operations can run in parallel:

```yaml
parallel_operations:
  - onboarding:
      - Analysis Agent → SQP Report
      - Analysis Agent → Business Report
      - Analysis Agent → Ad Report
      # All three can run simultaneously

  - post_logging:
      - Git Agent → stage and commit
      - Memory Agent → check if update needed
      # Independent operations
```

### Sequential Dependencies

Some operations must be sequential:

```yaml
sequential_operations:
  - brand_onboarding:
      1. Create folder structure
      2. Create README.md  # Needs folder
      3. Create MEMORY.md  # Needs README for reference
      4. Run analyses      # Needs brand context
      5. Create initial POA # Needs analysis results
```

---

## Error Handling

### Agent-Level Errors

Each agent should handle its own errors gracefully:

```yaml
error_responses:
  brand_not_found:
    message: "I don't see a folder for {brand}."
    suggestion: "Would you like to onboard this as a new brand?"
    fallback: route_to_onboarding_agent

  missing_context:
    message: "I don't have enough context for {brand}."
    action: load_more_context
    fallback: ask_user_for_input

  template_not_found:
    message: "Template {template} is missing."
    action: use_default_structure
    fallback: alert_user

  git_conflict:
    message: "There's a merge conflict in {file}."
    action: show_conflict_details
    fallback: provide_manual_resolution_steps
```

### Fallback to Single Agent

If sub-agent routing fails, fall back to the original single-agent behavior:

```yaml
fallback:
  condition: routing_confidence < 0.7 OR agent_error
  action: use_original_claude_md_instructions
  notify: "I'm handling this directly instead of routing to a specialized agent."
```

---

## Implementation Approach

### Phase 1: Router + Logging Agent
Start with the most common workflow:
1. Implement Router Agent with basic phrase matching
2. Implement Logging Agent with full workflow
3. Test on weekly logging scenarios

### Phase 2: Add Memory + Query Agents
Build context management:
1. Implement Memory Agent with auto-trigger
2. Implement Query Agent for history search
3. Connect Logging → Memory handoff

### Phase 3: Onboarding + Analysis Agents
Complete the onboarding workflow:
1. Implement Onboarding Agent (brand + product modes)
2. Implement Analysis Agent with all 6 report types
3. Connect Onboarding → Analysis parallelization

### Phase 4: Git Agent + Polish
Automate version control:
1. Implement Git Agent for common operations
2. Connect all agents to Git Agent for auto-commits
3. Add PR creation workflow

---

## File Structure

```
/agents/
    ARCHITECTURE.md          # This document
    router-agent.md          # Router/coordinator instructions
    logging-agent.md         # Logging specialist
    onboarding-agent.md      # Brand/product onboarding
    analysis-agent.md        # Report generation & interpretation
    memory-agent.md          # Context maintenance
    query-agent.md           # Historical search
    git-agent.md             # Version control operations
```

---

## Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Routing accuracy | >95% | Correct agent selected for request |
| Context efficiency | -50% tokens | Compared to single-agent loading everything |
| Parallel speedup | 2-3x | For onboarding report generation |
| Error recovery | 100% | All errors gracefully handled |
| User experience | No degradation | Same or better than single-agent |

---

## Next Steps

1. Review and approve this architecture
2. Create individual agent instruction files
3. Implement Phase 1 (Router + Logging)
4. Test and iterate
5. Proceed through remaining phases
