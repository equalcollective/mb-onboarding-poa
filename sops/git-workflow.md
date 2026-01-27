# Git Workflow SOP

## Purpose

Ensure multi-user collaboration works smoothly with proper version control and manager oversight through PR reviews.

## Overview

```
Main Branch (protected)
     ↑
     | PR (requires approval)
     |
Feature Branch (your-name/brand-date)
     ↑
     | commits
     |
Your Local Changes
```

---

## Daily Workflow

### 1. Start Your Day: Pull Latest

```bash
git checkout main
git pull origin main
```

### 2. Create Your Branch

Naming convention: `{your-name}/{brand-or-task}-{date}`

```bash
git checkout -b john/acme-weekly-2024-01-15
```

Examples:
- `john/acme-weekly-2024-01-15`
- `sarah/newbrand-onboarding`
- `mike/multiple-brands-logs`

### 3. Make Your Changes

Work with Claude to:
- Log weekly updates
- Create onboarding docs
- Update checklists
- Add reports

### 4. Commit Your Changes

```bash
# Stage specific files
git add brands/acme-widgets/logs/2024-01-15-john-weekly.md

# Or stage all changes in a brand folder
git add brands/acme-widgets/

# Commit with descriptive message
git commit -m "Add weekly log for Acme Widgets - Jan 15"
```

**Commit Message Guidelines:**
- Start with action verb: Add, Update, Complete, Fix
- Include brand name
- Include what was done

Examples:
- `Add weekly log for Acme Widgets - Jan 15`
- `Complete onboarding docs for NewBrand`
- `Update competitor analysis for Widget Co`
- `Add product onboarding for B00ABC123`

### 5. Push and Create PR

```bash
git push origin john/acme-weekly-2024-01-15
```

Then create PR via GitHub:
- Title: Same as commit message
- Description: Brief summary of changes
- Assign reviewer (manager)

### 6. PR Review and Merge

Manager reviews:
- Checks completeness
- Verifies formatting
- May request changes
- Approves and merges

---

## Common Scenarios

### Scenario: Multiple Brands in One Session

If logging for multiple brands:

```bash
git checkout -b john/multi-brand-logs-2024-01-15

# Make changes for Brand A
git add brands/brand-a/
git commit -m "Add weekly log for Brand A - Jan 15"

# Make changes for Brand B
git add brands/brand-b/
git commit -m "Add weekly log for Brand B - Jan 15"

git push origin john/multi-brand-logs-2024-01-15
# Create single PR with multiple commits
```

### Scenario: Continuing Work Over Multiple Days

```bash
# Day 1
git checkout -b sarah/newbrand-onboarding
# Do partial work
git add .
git commit -m "WIP: Start onboarding for NewBrand"
git push origin sarah/newbrand-onboarding

# Day 2
git checkout sarah/newbrand-onboarding
git pull origin sarah/newbrand-onboarding
# Continue work
git add .
git commit -m "Complete onboarding for NewBrand"
git push origin sarah/newbrand-onboarding
# Now create PR
```

### Scenario: Changes Requested on PR

```bash
git checkout john/acme-weekly-2024-01-15
# Make requested changes
git add .
git commit -m "Address PR feedback: add missing ASIN details"
git push origin john/acme-weekly-2024-01-15
# PR automatically updates
```

### Scenario: Merge Conflicts

If your branch has conflicts with main:

```bash
git checkout main
git pull origin main
git checkout your-branch
git merge main
# Resolve conflicts in editor
git add .
git commit -m "Resolve merge conflicts with main"
git push origin your-branch
```

---

## Best Practices

### Do:
- Pull main before creating new branch
- Use descriptive branch names
- Commit frequently with clear messages
- Keep PRs focused (one brand or one task)
- Respond to PR feedback promptly

### Don't:
- Push directly to main
- Create huge PRs with weeks of work
- Leave PRs open for too long
- Ignore merge conflicts

---

## Quick Reference

| Action | Command |
|--------|---------|
| Pull latest | `git pull origin main` |
| Create branch | `git checkout -b name/description` |
| Stage files | `git add path/to/files` |
| Commit | `git commit -m "Message"` |
| Push | `git push origin branch-name` |
| Switch branch | `git checkout branch-name` |
| See status | `git status` |
| See changes | `git diff` |

---

## For Managers: Reviewing PRs

When reviewing PRs, check:

1. **Completeness** - All required sections filled?
2. **Accuracy** - ASINs correct? Dates accurate?
3. **Clarity** - Will this be understandable later?
4. **Consistency** - Follows templates and conventions?
5. **Actionability** - Next steps are clear?

Approve and merge, or request changes with specific feedback.
