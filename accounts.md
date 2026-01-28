# Accounts Registry

Central reference for all brands and account managers.

---

## Account Managers

| Name | Slug | Email | Active Brands |
|------|------|-------|---------------|
| John Smith | john | john@agency.com | acme-kitchenware |
| Sarah Chen | sarah | sarah@agency.com | maurices-piggie-park |

---

## Brands

| Brand | Slug | Account Manager | Status | Start Date |
|-------|------|-----------------|--------|------------|
| Acme Kitchenware | acme-kitchenware | john | Active | 2024-01-01 |
| Maurice's Piggie Park | maurices-piggie-park | sarah | Onboarding | 2024-01-20 |

---

## Quick Lookups

### By Account Manager
```
john:
  - acme-kitchenware

sarah:
  - maurices-piggie-park
```

### By Status
```
active:
  - acme-kitchenware

onboarding:
  - maurices-piggie-park
```

---

## Usage

Agents should reference this file for:
- Validating brand/AM names
- Auto-completing brand slugs
- Routing queries across multiple brands
- Finding which AM owns which brand

**Source of truth:** Individual `brands/{slug}/README.md` files.
This registry is a convenience index that should stay in sync.

---

*Last Updated: 2024-01-28*
