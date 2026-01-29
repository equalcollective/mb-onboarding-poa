# Agent Activation Triggers

This document defines when each agent activates based on **intent + context signals** rather than specific trigger phrases.

---

## Core Principle: Need-Based Routing

The system detects user needs through a combination of:

1. **Intent Classification** - What the user is trying to accomplish
2. **Context Signals** - Linguistic and situational cues
3. **Confidence Scoring** - How certain we are about the intent

```
User Input → Intent Classifier → Context Analyzer → Signal Detector → Agent Selector
```

---

## Intent Categories

| Intent | Description | Primary Agent |
|--------|-------------|---------------|
| **RECORD** | Capture activity, log work done | Logging Agent |
| **CREATE** | Set up new entity (brand/product) | Onboarding Agent |
| **UNDERSTAND** | Analyze data, interpret metrics | Analysis Agent |
| **RECALL** | Find historical information | Query Agent |
| **RESEARCH** | Investigate competitors, market | Research Agent |
| **MAINTAIN** | Update rolling context | Memory Agent |
| **PUBLISH** | Version control operations | Git Agent |
| **LEARN** | Apply frameworks, get guidance | Knowledge Agent |
| **MEASURE** | Get real-time metrics/data | Data Agent |

---

## Signal Detection by Intent

### RECORD Intent (→ Logging Agent)

**Context Signals:**
- Past tense verbs: "did", "changed", "adjusted", "updated", "added", "removed"
- Temporal references: "this week", "yesterday", "last few days", "today I"
- Activity descriptions: "worked on", "looked at", "made changes to"
- Work summaries: "here's what happened", "update for", "to report"

**Entity Signals:**
- Brand name mentioned
- Products/ASINs referenced
- Campaign names included

**Confidence Boosters:**
- Multiple actions described
- Specific metrics included
- "Weekly" or "log" mentioned (but not required)

**Example Activations:**
```
"This week I adjusted bids on the auto campaign"          → RECORD
"ACOS is down to 30% after the changes"                  → RECORD
"Here's what happened with KetoGoods"                    → RECORD
"Reduced spend by 20%, added negative keywords"          → RECORD
```

---

### CREATE Intent (→ Onboarding Agent)

**Context Signals:**
- Creation language: "new", "set up", "create", "add", "onboard"
- Unknown entity: brand/product name not in system
- Introduction language: "starting with", "just signed", "new client"

**Entity Signals:**
- New brand name (not in accounts.md)
- New ASIN (not in existing product docs)

**Confidence Boosters:**
- Client/brand context provided
- "Starting" or "beginning" language

**Example Activations:**
```
"New client called Sunrise Foods"                         → CREATE (brand)
"We just signed a brand called XYZ"                       → CREATE (brand)
"Need to add product B00ABC1234 for Acme"                → CREATE (product)
"Let's get started with a new brand"                      → CREATE (brand)
```

---

### UNDERSTAND Intent (→ Analysis Agent)

**Context Signals:**
- Questions about data: "what does this mean", "how should I interpret"
- Analysis language: "analyze", "run report", "look at data"
- Insight seeking: "what's working", "what needs attention", "opportunities"

**Metric Signals:**
- ACOS, ROAS, TACoS mentioned
- Performance metrics: sessions, conversion, BSR
- Trend questions: "why is it up/down"

**Parallel Invocation:**
- Knowledge Agent (when uncertainty detected)
- Data Agent (when real-time data needed)

**Example Activations:**
```
"What does the SQP data show?"                           → UNDERSTAND
"Help me interpret these ad metrics"                     → UNDERSTAND
"Run the business report analysis"                       → UNDERSTAND
"ACOS at 45%, not sure why"                              → UNDERSTAND + LEARN
```

---

### RECALL Intent (→ Query Agent)

**Context Signals:**
- Retrieval language: "show me", "find", "what happened", "when did"
- Historical focus: "last time", "previous", "history of"
- Search orientation: "search for", "look up", "check on"

**Scope Signals:**
- Specific dates/periods mentioned
- Topic focus (advertising, inventory, etc.)
- Cross-brand indicators ("all brands", "my accounts")

**Example Activations:**
```
"Show me the last few logs for Acme"                     → RECALL
"What happened with inventory last month?"               → RECALL
"When did we change the bid strategy?"                   → RECALL
"Find any mentions of competitor X"                      → RECALL
```

---

### RESEARCH Intent (→ Research Agent)

**Context Signals:**
- Competitor focus: "competitors", "competition", "market"
- Investigation language: "research", "analyze", "investigate"
- D2C/external: "their website", "off Amazon", "direct site"

**Entity Signals:**
- Competitor names/ASINs
- Product URLs provided
- Category/market mentioned

**Example Activations:**
```
"Who are the main competitors for this product?"         → RESEARCH
"Analyze the competitive landscape"                      → RESEARCH
"How are competitors priced?"                            → RESEARCH
"Research B00XYZ123 for competitive analysis"            → RESEARCH
```

---

### MAINTAIN Intent (→ Memory Agent)

**Context Signals:**
- Memory language: "remember", "note that", "important for later"
- Summary requests: "summarize recent", "update memory", "consolidate"
- Decision importance: "key decision", "we decided", "going forward"

**Automatic Triggers:**
- 4+ logs since last memory update
- Significant decision logged
- Pattern identified in recent logs

**Example Activations:**
```
"Update memory with recent decisions"                    → MAINTAIN
"We made an important decision about pricing"            → MAINTAIN
"Consolidate the last few weeks"                         → MAINTAIN
[Automatic after 4 logs]                                 → MAINTAIN
```

---

### PUBLISH Intent (→ Git Agent)

**Context Signals:**
- Git language: "commit", "push", "branch", "PR", "pull request"
- Ready language: "done", "finished", "ready to save"
- Version control: "save changes", "share with team"

**Example Activations:**
```
"Commit these changes"                                   → PUBLISH
"Create a PR for review"                                 → PUBLISH
"Push the updates"                                       → PUBLISH
"I'm done, save everything"                              → PUBLISH
```

---

### LEARN Intent (→ Knowledge Agent)

**Context Signals:**
- Uncertainty: "not sure", "wondering", "how should I"
- Guidance seeking: "best practice", "framework", "approach"
- Theory formation: "hypothesis", "theory", "suspect", "might be"

**Parallel Invocation:**
- Always runs alongside primary agent when uncertainty detected
- Provides frameworks without interrupting main workflow

**Example Activations:**
```
"How should I think about this ACOS spike?"              → LEARN (+ UNDERSTAND)
"Not sure why performance dropped"                       → LEARN (+ RECORD if logging)
"What's the framework for analyzing this?"               → LEARN
"I think it might be a competition issue"                → LEARN (+ relevant agent)
```

---

### MEASURE Intent (→ Data Agent)

**Context Signals:**
- Data requests: "how are sales", "what's the ACOS", "show metrics"
- Real-time focus: "current", "right now", "latest"
- Validation needs: "check if", "verify", "confirm"

**Entity Signals:**
- Seller name mentioned
- Product name (needs resolution)
- Metric types specified

**Example Activations:**
```
"How are sales this week?"                               → MEASURE
"What's the current ACOS for the connector kit?"         → MEASURE
"Check if that ACOS improvement held"                    → MEASURE
"Show me Utah Pneumatics metrics"                        → MEASURE
```

---

## Multi-Intent Detection

Many inputs trigger multiple intents. Handle with parallel or sequential routing:

### Parallel Combinations

| Combined Intent | Example | Agents |
|-----------------|---------|--------|
| RECORD + LEARN | "ACOS jumped, not sure why" | Logging + Knowledge |
| UNDERSTAND + MEASURE | "Analyze current performance" | Analysis + Data |
| RECALL + UNDERSTAND | "What happened and what should we do?" | Query + Analysis |
| RESEARCH + CREATE | "Research and onboard this product" | Research → Onboarding |

### Sequential Combinations

| Sequence | Example | Flow |
|----------|---------|------|
| CREATE → UNDERSTAND | "Onboard brand and run analysis" | Onboarding, then Analysis |
| RECORD → PUBLISH | "Log this and commit it" | Logging, then Git |
| UNDERSTAND → MAINTAIN | "Analyze and update memory" | Analysis, then Memory |

---

## Confidence Thresholds

| Confidence | Action |
|------------|--------|
| **>90%** | Route directly to agent |
| **70-90%** | Route with clarification note |
| **<70%** | Ask clarifying question |

### Low Confidence Handling

When intent is ambiguous:

```
"Update Acme"

Confidence: Low (~50%)
Possible intents: RECORD, MAINTAIN, CREATE (product)

Response:
"I can help update Acme. Are you looking to:
1. Log recent activity? (RECORD)
2. Update the brand memory? (MAINTAIN)
3. Something else?"
```

---

## Signal Priority

When multiple signals conflict, prioritize:

1. **Explicit requests** - "Log this", "Create new brand"
2. **Entity context** - Brand exists vs new
3. **Verb tense** - Past (RECORD) vs Present (MEASURE) vs Future (CREATE)
4. **Question structure** - Questions → RECALL/UNDERSTAND, Statements → RECORD

---

## Fallback Behavior

If routing fails or confidence is very low:

1. Ask a single clarifying question
2. If still unclear, default to Query Agent for exploration
3. Never block the user - always provide a helpful path forward

---

## Quick Reference Table

| User Says Pattern | Primary Signal | Agent | Parallel Agent |
|-------------------|----------------|-------|----------------|
| Past tense + metrics | RECORD | Logging | Knowledge (if uncertain) |
| "New" + unknown entity | CREATE | Onboarding | - |
| Questions about data | UNDERSTAND | Analysis | Knowledge, Data |
| "Show me" + historical | RECALL | Query | - |
| Competitor mentions | RESEARCH | Research | Knowledge |
| 4+ logs since update | MAINTAIN | Memory | - |
| "Commit", file changes | PUBLISH | Git | - |
| Uncertainty language | LEARN | Knowledge | (runs parallel) |
| Real-time metric requests | MEASURE | Data | Knowledge |

---

## Testing Your Understanding

| Test Input | Expected Agent(s) | Why |
|------------|-------------------|-----|
| "This week I adjusted bids, ACOS is down to 30%" | Logging + Data | Past tense + metric |
| "What's happening with KetoGoods?" | Query + Analysis | Question about state |
| "New client called Sunrise Foods" | Onboarding | Unknown entity |
| "ACOS at 45%, not sure why" | Analysis + Knowledge | Metric + uncertainty |
| "Show me last few logs for Acme" | Query | Recall request |
| "How are competitors priced?" | Research | Competitor mention |
| "Commit the changes" | Git | Explicit git action |
