# Sub-Agent System

This folder contains the design and specifications for the sub-agent architecture.

## Quick Start

Read these in order:
1. **[ARCHITECTURE.md](ARCHITECTURE.md)** - Overall design, rationale, and implementation plan
2. **[router-agent.md](router-agent.md)** - How requests get routed to sub-agents
3. Individual agent specs (as needed)

## Agents Overview

| Agent | File | Purpose |
|-------|------|---------|
| **Router** | [router-agent.md](router-agent.md) | Coordinates all other agents, routes requests |
| **Logging** | [logging-agent.md](logging-agent.md) | Structures unstructured input into log entries |
| **Memory** | [memory-agent.md](memory-agent.md) | Maintains rolling context in MEMORY.md files |
| **Query** | [query-agent.md](query-agent.md) | Searches and summarizes historical information |
| **Analysis** | [analysis-agent.md](analysis-agent.md) | Generates and interprets analysis reports |
| **Onboarding** | [onboarding-agent.md](onboarding-agent.md) | Guides brand and product onboarding |
| **Knowledge** | [knowledge-agent.md](knowledge-agent.md) | Surfaces expert frameworks from `/knowledge/` |
| **Git** | [git-agent.md](git-agent.md) | Handles version control operations |

## Architecture Summary

```
User Request → Router Agent → Specialized Sub-Agent → File System
                                      │
                                      ├──→ Knowledge Agent (parallel)
                                      │         │
                                      │         └──→ /knowledge/
                                      ↓
                              Handoffs between agents as needed
```

## Implementation Phases

| Phase | Focus | Agents |
|-------|-------|--------|
| 1 | Core workflow | Router + Logging |
| 2 | Context management | Memory + Query |
| 3 | Onboarding | Onboarding + Analysis |
| 4 | Automation | Git + Polish |

## Key Concepts

- **Context contracts**: Each agent knows exactly what files to read/write
- **Handoff protocol**: Structured way for agents to pass work to each other
- **Parallel execution**: Independent tasks can run simultaneously
- **Fallback behavior**: Graceful degradation to single-agent if needed

## Status

**Current:** Design complete, ready for Phase 1 implementation.
