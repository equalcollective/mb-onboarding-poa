# Amazon Brand Management System

A file-based system for managing Amazon brand accounts, powered by Claude Code.

## What This Is

This repository serves as the central knowledge base and logging system for account managers working on Amazon seller accounts. It provides:

- **Structured logging** of weekly actions and observations
- **Brand onboarding** documentation and checklists
- **Product onboarding** with detailed analysis
- **Plan of Action (POA)** tracking
- **Historical context** for querying past activity

## How It Works

```
Account Manager + Claude Code
         ↓
    Voice/Text Input
         ↓
  Claude structures and asks clarifying questions
         ↓
    Markdown files created/updated
         ↓
    Commit → Push → PR
         ↓
    Manager reviews and approves
```

## Quick Start

### For Account Managers

**1. Log a weekly update:**
> "I want to log my weekly update for [Brand Name]"

**2. Onboard a new brand:**
> "Let's onboard a new brand called [Brand Name]"

**3. Review recent activity:**
> "Show me recent logs for [Brand Name]"

**4. Create a plan of action:**
> "I want to log a plan of action for [Brand Name]"

See [SOPs](./sops/) for detailed instructions.

### Git Workflow

```bash
# Pull latest
git pull origin main

# Create your branch
git checkout -b yourname/brand-task-date

# Make changes with Claude
# ...

# Commit and push
git add .
git commit -m "Add weekly log for Brand - Jan 15"
git push origin yourname/brand-task-date

# Create PR for review
```

## Repository Structure

```
/
├── README.md                    # This file
├── brands/                      # All brand folders
│   └── {brand-name}/
│       ├── README.md            # Brand overview
│       ├── onboarding/          # Onboarding docs
│       │   ├── account.md
│       │   ├── checklist.md
│       │   ├── reports/
│       │   └── products/
│       └── logs/                # All log entries
│           └── YYYY-MM-DD-author-type.md
├── templates/                   # Document templates
│   ├── brand-readme.md
│   ├── account-onboarding.md
│   ├── onboarding-checklist.md
│   ├── product-onboarding.md
│   ├── log-entry.md
│   └── reports/
└── sops/                        # Standard Operating Procedures
    ├── system-overview.md
    ├── weekly-logging.md
    ├── brand-onboarding.md
    ├── product-onboarding.md
    ├── querying-history.md
    └── git-workflow.md
```

## Log Entry Types

| Type | Tag | Use For |
|------|-----|---------|
| Weekly | `weekly` | Regular weekly updates |
| POA | `poa` | Strategic plans of action |
| Action | `action` | Specific significant actions |
| Observation | `observation` | Important findings or insights |

## Brands

| Brand | Account Manager | Status |
|-------|-----------------|--------|
| [Acme Kitchenware](./brands/acme-kitchenware/) | John Smith | Active |

---

## Documentation

- [System Overview](./sops/system-overview.md) - How everything works
- [Weekly Logging SOP](./sops/weekly-logging.md) - How to log
- [Brand Onboarding SOP](./sops/brand-onboarding.md) - New brand setup
- [Product Onboarding SOP](./sops/product-onboarding.md) - New product setup
- [Querying History SOP](./sops/querying-history.md) - Finding past info
- [Git Workflow SOP](./sops/git-workflow.md) - Collaboration process

---

## For Claude (System Context)

When interacting with account managers in this repository:

1. **You are an Amazon marketing expert** - Ask clarifying questions that help create better documentation
2. **Structure inputs** - Take raw voice/text input and organize into proper markdown format
3. **Gather context** - Read brand README and recent logs before responding to queries
4. **Use templates** - When creating new documents, use templates from `/templates/`
5. **Be thorough but concise** - Ensure all required metadata is captured
6. **Prompt for specifics** - ASINs, campaign names, numbers, dates make logs more useful

### Common Commands

| User Says | Claude Does |
|-----------|-------------|
| "Log weekly update for X" | Create log entry, ask clarifying questions |
| "Onboard brand X" | Create folder structure, guide through setup |
| "Show recent logs for X" | Read and summarize recent log files |
| "What's the POA for X" | Find and present current plan of action |
| "Update checklist for X" | Read and update checklist.md |
