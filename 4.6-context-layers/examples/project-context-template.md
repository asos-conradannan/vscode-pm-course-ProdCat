---
version: 1.0
project_id: gpsr-compliance
domain: product-catalogue
status: in_discovery
last_updated: 2026-02-26
owner: Product Catalogue PM
sprint_target: Sprint 44
---

# Project: GPSR Compliance — Product Catalogue

*This is a template showing the structure for a Product Catalogue initiative. Replace all [bracketed] values with actual initiative details. Copy this folder to `.context/projects/[initiative-name]/` for each new initiative.*

---

## Overview

### One-Liner
Ensure 100% of products sold in EU/UK markets include GPSR-compliant manufacturer contact data in the Product Catalogue API.

### Status
**Current Phase:** Discovery → MVP
**Health:** On Track
**Sprint Target:** Sprint 44 (Phase 1 — API fields)
**Azure DevOps Epic:** [Epic ID — link here]
**Confluence PRD:** [pp space page URL]

### Quick Links
- **ADO Epic:** [URL]
- **Confluence PRD:** [pp space URL]
- **Figma (BackOffice):** [URL if applicable]
- **Miro (Architecture):** [URL if applicable]

---

## Product Context

### Problem Statement
The EU/UK General Product Safety Regulation (GPSR) requires products sold in these markets to display the manufacturer's name, address, and contact information on product listings. Currently, ASOS's Product Catalogue API does not return this data — it is not sourced from PIM and not stored in the Catalogue.

Approximately 40% of products have partial manufacturer data in PIM. The remainder have no GPSR data.

**GPSR enforcement date:** [Date]
**Penalty for non-compliance:** [Regulatory consequence]

### Target Users (Internal API Consumers)
| Consumer | What they need | Priority |
|----------|----------------|----------|
| PDP team (web + app) | Display manufacturer info on product page | P0 |
| BackOffice admin | View/edit manufacturer data per product | P0 |
| Legal/Compliance | Confirm 100% coverage by enforcement date | P0 |
| Search (Fredhopper) | Index compliance fields for potential filtering | P2 |

### Goals

**Primary:**
- 100% of EU/UK products have GPSR-compliant manufacturer data in the Product Catalogue API
- Manufacturer data is available in FullProduct endpoint before enforcement date

**Success Metrics:**
| Metric | Baseline | Target | When |
|--------|----------|--------|------|
| Products with manufacturerName | ~40% | 100% (EU/UK) | Before enforcement date |
| API latency — FullProduct p99 | [current] | No regression | Sprint 44 |
| BackOffice GPSR edit workflow | Does not exist | Functional | Sprint 45 (Phase 2) |

**Non-Goals (this phase):**
- Bulk edit tool for manufacturer data (Phase 2 / P2)
- Automated compliance validation gate in Publisher (future initiative)
- Extending to non-EU/UK stores (not required by GPSR)

---

## Technical Context

### Architecture Overview
```
PIM ComplianceInfo payload
        │
        ▼ (fat event — GPSR fields included from Sprint X)
  Product Publisher
        │ (new mapping: ComplianceInfo → ManufacturerCompliance model)
        ▼
  Product Catalogue DB
        │ (new compliance sub-document per product)
        ▼
  FullProduct API endpoint
        │ (new compliance object in response)
        ├──► PDP team (web + app)
        └──► BackOffice (read in Phase 1, edit in Phase 2)
```

### New Data Fields

| Field | Type | Location in API | Source | Required for GPSR? |
|-------|------|----------------|--------|-------------------|
| `manufacturerName` | string | `product.compliance` | PIM ComplianceInfo | Yes |
| `manufacturerAddress` | string | `product.compliance` | PIM ComplianceInfo | Yes |
| `manufacturerContact` | string | `product.compliance` | PIM ComplianceInfo | No |
| `isGpsrCompliant` | boolean | `product.compliance` | Derived (fields populated?) | Yes |

### Affected Services / Repositories
- `asos-product-packages-models` — ManufacturerCompliance class (new)
- `asos-product-publisher` — ComplianceInfo → ManufacturerCompliance mapping (new)
- `asos-product-catalogue-api` — FullProduct response (compliance object added)
- BackOffice (Phase 2 only)

### Story Cascade
```
Phase 1:
[GPSR] Story 1: product-packages-models — Add ManufacturerCompliance class
[GPSR] Story 2: Product Publisher — Map ComplianceInfo from PIM
[GPSR] Story 3: Product Catalogue API — Expose compliance in FullProduct

Phase 2 (future sprint):
[GPSR] Story 4: BackOffice — Display manufacturer info in product detail (read-only)
[GPSR] Story 5: BackOffice — Edit manufacturer info in product detail
```

### Open Technical Questions
- [ ] Does PIM currently provide `ComplianceInfo` in the fat event payload? (Confirm with PIM team by Sprint 42)
- [ ] Is `manufacturerContact` email, phone, or either? (Confirm with Legal)
- [ ] Do we need to validate EU/UK store scope at Publisher level, or query time?
- [ ] What cache invalidation strategy when manufacturer data is updated in BackOffice (Phase 2)?

### Constraints
- Must not increase FullProduct p99 latency
- Must not affect products outside EU/UK scope
- ManufacturerCompliance class must be backward-compatible (optional fields, no breaking change)

---

## Key Decisions

### Decision 1: Source of Truth for Manufacturer Data
- **Date:** [Date]
- **Context:** Options were (a) PIM provides data, or (b) BackOffice manual entry is the primary source
- **Decision:** PIM is the primary source; BackOffice is a fallback edit tool for exceptions
- **Rationale:** PIM has manufacturer data for brands we work with; BackOffice manual entry is not scalable for 100M+ products
- **Consequences:** Dependency on PIM team to provide ComplianceInfo field in events

### Decision 2: Phase 1 = API only, Phase 2 = BackOffice edit
- **Date:** [Date]
- **Context:** Legal needs API data for GPSR compliance; BackOffice edit can follow
- **Decision:** Ship Phase 1 (API read) before enforcement date; BackOffice edit in following sprint
- **Rationale:** Reduces scope and risk for enforcement deadline
- **Consequences:** BackOffice initially read-only; manual Jira ticket to track Phase 2

---

## Stakeholders

| Role | Name | Engagement | Key Ask |
|------|------|------------|---------|
| Engineering Lead | [Name] | Daily | Technical design, effort estimate |
| PDP Lead | [Name] | Sprint-by-sprint | API consumer readiness, contract review |
| Legal/Compliance | [Name] | Milestone sign-off | Confirm fields meet GPSR requirements |
| PIM Team | [Name] | Discovery blocker | Confirm ComplianceInfo available in PIM events |
| BackOffice Team | [Name] | Phase 2 | BackOffice implementation design |

---

## Timeline

| Milestone | Target | Owner | Status |
|-----------|--------|-------|--------|
| PIM field confirmation | Sprint 42 | PM | Pending |
| Story cascade created in ADO | Sprint 42 | PM | Pending |
| Phase 1 development complete | Sprint 43 | Engineering | Not started |
| PDP integration tested | Sprint 44 | PDP team | Not started |
| Legal sign-off | Sprint 44 | Legal | Not started |
| Phase 1 launch | Sprint 44 | PM | Not started |
| Phase 2 (BackOffice edit) | Sprint 45 | Engineering | Not started |

---

## Related Resources

- [Domain context](../../domains/product-catalogue/README.md)
- [Business rules](../../domains/product-catalogue/business-rules.md)
- [Global terminology](../../global/terminology.md)
- Confluence PRD: [pp space URL]
- ADO Epic: [URL]
- GPSR regulation text: [Legal source URL]

---

*Project context created via Module 4.6 of the Product Catalogue VSCode PM Course.*
*Bootstrap additional content using: "Search pp Confluence space for GPSR pages and extract into this context file."*
