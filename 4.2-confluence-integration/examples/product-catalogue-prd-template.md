---
context:
  domain: product-catalogue
  type: template
  purpose: PRD template for Product Catalogue API and platform features
  related:
    - .context/domains/product-catalogue/business-rules.md
    - .context/domains/product-catalogue/terminology.md
status: template
owner: Product Catalogue PM
---

# [Feature Name] — Product Requirements Document

**Status:** Draft | In Review | Approved | Delivered
**Owner:** [PM Name]
**Engineering Lead:** [Name]
**Sprint Target:** [Sprint Number]
**Created:** [Date]
**Last Updated:** [Date]

---

## 1. Problem Statement

### The Problem
[What problem are we solving? Be specific about the business or regulatory driver.]

### Why Now
[What's the urgency? GPSR enforcement date? OKR alignment? Consumer team blocker?]

### Impact of Not Solving
[What happens if we don't build this? Compliance risk? Consumer team impact? Revenue impact?]

**Evidence:**
- [Data point 1 — e.g., "GPSR enforcement from [date], applies to all EU/UK orders"]
- [Data point 2 — e.g., "PDP team blocked on surfacing manufacturer info until this ships"]
- [Data point 3]

---

## 2. Goals & Success Metrics

### Primary Goal
[One sentence. E.g., "Ensure 100% of products sold in EU/UK include GPSR-compliant manufacturer contact data by Q2 2026."]

### Success Metrics

| Metric | Baseline | Target | Measurement |
|--------|----------|--------|-------------|
| [e.g., Products with manufacturer data] | [0%] | [100%] | [Databricks query] |
| [e.g., API latency — FullProduct endpoint] | [Xms p99] | [Xms p99] | [Azure Monitor] |
| [e.g., Publication SLA] | [Xmin] | [Xmin] | [Celonis dashboard] |

### Non-Goals
- [What is explicitly out of scope]
- [What we'll defer to a future initiative]

---

## 3. Internal Users & API Consumers

This feature serves internal teams via the Product Catalogue API. Key consumers:

| Consumer | What They Need | Priority |
|----------|----------------|----------|
| PDP Team (web + app) | [e.g., Display manufacturer contact on product page] | P0 |
| BackOffice Admin | [e.g., Review/edit manufacturer data per product] | P0 |
| Search (Fredhopper/Algolia) | [e.g., Index manufacturer fields for compliance filtering] | P1 |
| Marketing email feeds | [e.g., N/A — not affected] | N/A |
| Third-party feeds | [e.g., Include manufacturer fields in brand feeds] | P2 |

**Stakeholder sign-off needed from:**
- [ ] PDP Engineering Lead
- [ ] BackOffice team
- [ ] Search team (if schema change affects indexing)
- [ ] Legal/Compliance (for regulatory requirements)

---

## 4. Proposed Solution

### Approach
[High-level description of what we're building. Avoid implementation detail — that's engineering's job.]

### User / System Flow
```
[PIM] ─── [new GPSR fields] ──► [Publisher] ──► [Product Catalogue] ──► [FullProduct API]
                                                                              │
                                                          ┌───────────────────┼────────────┐
                                                          ▼                   ▼            ▼
                                                      [PDP Web]          [BackOffice]  [Feeds API]
```

### What Changes
- **PIM → Publisher:** [What new fields arrive from PIM and how the publisher maps them]
- **Publisher → Catalogue:** [What the catalogue stores]
- **API layer:** [Which endpoints are updated and how]
- **BackOffice:** [Any admin interface changes]

### What Doesn't Change
- [Endpoints that are not affected]
- [Existing field behaviour that is preserved]

---

## 5. API Contract

**Affected Endpoints:**
- `GET /fullproduct/{productId}` — primary change
- `GET /variants/{productId}` — [if affected]
- `GET /summaries/{productId}` — [if affected]

**New / Updated Fields:**

| Field Name | Type | Location in Response | Required | Default | Notes |
|------------|------|---------------------|----------|---------|-------|
| `manufacturerName` | `string` | `product.compliance` | Yes | `null` | GPSR requirement |
| `manufacturerAddress` | `string` | `product.compliance` | Yes | `null` | GPSR requirement |
| `manufacturerContact` | `string` | `product.compliance` | No | `null` | Email or phone |
| `isGpsrCompliant` | `boolean` | `product.compliance` | Yes | `false` | Derived from data presence |

**Example Response Snippet:**
```json
{
  "productId": "12345678",
  "compliance": {
    "manufacturerName": "Example Manufacturer Ltd",
    "manufacturerAddress": "123 Factory Road, London, UK",
    "manufacturerContact": "compliance@example.com",
    "isGpsrCompliant": true
  }
}
```

**Breaking Change:** No / Yes — [explain if yes]
**Versioning:** [Will a new API version be needed?]

---

## 6. Requirements

### P0 — Must Have (Launch Blockers)
- [ ] [e.g., GPSR fields returned in FullProduct endpoint]
- [ ] [e.g., PIM provides fields; Publisher maps them correctly]
- [ ] [e.g., BackOffice shows manufacturer data in product detail view]
- [ ] [e.g., API contract documented and shared with PDP and BackOffice teams]

### P1 — Should Have (This Sprint)
- [ ] [e.g., Validation: warn if GPSR fields missing for EU/UK-scoped products]
- [ ] [e.g., Feeds API updated with new fields]
- [ ] [e.g., Speed-to-Market dashboard updated to track GPSR compliance rate]

### P2 — Nice to Have (Future Sprint)
- [ ] [e.g., Admin bulk-edit tool for updating manufacturer data across product ranges]
- [ ] [e.g., Automated compliance check in publication pipeline]

---

## 7. Event Architecture

**Event-Driven Changes:**

| Event | Source | Trigger | Impact |
|-------|--------|---------|--------|
| `ProductUpdated` | PIM | Manufacturer fields updated in PIM | Re-publish to catalogue |
| `GpsrStatusChanged` | Catalogue | isGpsrCompliant changes value | Notify downstream (PDP cache invalidation) |

**Event Type:** Fat event (full product payload) / Lean event (ID only)
**Idempotency:** [How duplicate events are handled]
**Cache Invalidation:** [Which caches are invalidated on update — Redis PLP cache, FullProduct cache]

---

## 8. Compliance & Restrictions

**Regulatory Requirement:** [e.g., GPSR — General Product Safety Regulation (EU/UK)]
**Enforcement Date:** [Date]
**Scope:** [e.g., All products sold in EU and UK stores]
**Products Excluded:** [e.g., Digital products, gift cards]

**Regional Considerations:**

| Region | Required | Notes |
|--------|----------|-------|
| UK | Yes | Post-Brexit GPSR equivalent |
| EU (245 countries) | Yes | Original GPSR regulation |
| US / Rest of World | No | Not required, fields optional |

**Hazmat / Restrictions Interaction:** [Does this feature interact with existing hazmat or brand restriction logic?]

---

## 9. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| PIM doesn't provide all GPSR fields | Medium | High | Confirm with PIM team by [date] |
| Cache invalidation delays after manufacturer update | Low | Medium | TTL + jitter strategy; monitor with Azure Monitor |
| Event ordering issue (product published before manufacturer data) | Low | High | Idempotent event processing; republish on GPSR update |
| PDP team not ready to consume new fields | Medium | Medium | API contract shared by Sprint X; PDP team aligned |
| Translation impact (manufacturer names need localisation?) | Low | Medium | Confirm with Smartling/Azure AI translation pipeline |

---

## 10. Timeline & Milestones

| Milestone | Target Date | Owner | Status |
|-----------|-------------|-------|--------|
| PIM field confirmation | [Date] | [Name] | Not started |
| API contract signed off | [Date] | PM + Engineering Lead | Not started |
| Story cascade created in ADO | [Date] | PM | Not started |
| Publisher mapping complete | [Date] | Engineering | Not started |
| FullProduct endpoint updated | [Date] | Engineering | Not started |
| PDP integration tested | [Date] | PDP team | Not started |
| BackOffice updated | [Date] | Engineering | Not started |
| Compliance sign-off | [Date] | Legal | Not started |
| Launch | [Date] | PM | Not started |

---

## Open Questions

- [ ] [Question 1 — e.g., "Does PIM currently store manufacturer contact? If not, what's the data source?"]
- [ ] [Question 2 — e.g., "Is manufacturerContact email or phone or both?"]
- [ ] [Question 3 — e.g., "What's the PDP team timeline for consuming new fields?"]
- [ ] [Question 4]

---

## Approvals

| Role | Name | Status | Date |
|------|------|--------|------|
| Engineering Lead | | Pending | |
| PDP Team Lead | | Pending | |
| Legal/Compliance | | Pending | |
| Product Manager | | Pending | |

---

*This document uses the Product Catalogue PRD template. Template maintained by the Product Catalogue PM.*
