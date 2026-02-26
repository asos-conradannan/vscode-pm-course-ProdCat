---
context:
  domain: product-catalogue
  type: example
  purpose: Shows the output format for a Product Catalogue architecture review synthesis
  related:
    - .context/domains/product-catalogue/business-rules.md
status: example
---

# Product Catalogue Architecture Target State Review
## Workshop Synthesis — Q2 2026

**Session date:** [Date]
**Participants:** [Engineering Lead, Senior Engineers x3, PM, Architect]
**Miro board:** [URL]
**Synthesised via:** Module 4.5 Miro MCP + Copilot
**Time to synthesise:** 8 minutes (vs. ~90 minutes manual)

---

## Executive Summary

The Product Catalogue team aligned on a target state centred around three strategic shifts: reducing publication latency from current ~15 minutes to under 3 minutes via event-driven architecture improvements; achieving full GPSR compliance across all EU/UK product lines by Q3 2026; and completing the MongoDB Cosmos DB migration to unlock horizontal scaling for 245+ country support.

The session surfaced four systemic gaps — primarily around cache invalidation reliability, PIM event ordering guarantees, the backlog of API consumers still relying on polling rather than event subscriptions, and the operational burden of the TGR legacy system running in parallel.

Prioritised actions (see below) reflect a phased approach: unblock compliance work first, then address scalability, then complete the legacy migration.

---

## Key Themes from Target State Stickies

### Theme 1: Publication Speed (11 mentions)
**Desired state:** Product published in PIM is live on ASOS within 3 minutes (vs. current ~15 minutes)
**Key ideas:**
- Reduce Publisher processing time with parallel enrichment
- Move PLP cache population to async (not blocking publication)
- IndexNow ping on publish (removes 24h indexing delay for SEO)
- Alert when publication latency exceeds 5 minutes (currently no SLA monitoring)

### Theme 2: Compliance Coverage (9 mentions)
**Desired state:** 100% of EU/UK products have GPSR-compliant manufacturer data; hazmat restrictions applied at publication not query time
**Key ideas:**
- Manufacturer data sourced from PIM (not BackOffice manual entry)
- Compliance validation gate in Publisher pipeline (reject non-compliant before publish)
- Audit log for compliance field changes
- RegTech dashboard showing compliance coverage by brand/category

### Theme 3: API Consumer Modernisation (7 mentions)
**Desired state:** All consuming teams (PDP, Search, Recommendations) using event subscriptions, not polling
**Key ideas:**
- Deprecate polling endpoints on clear timeline (communicate with PDP and Search teams)
- Event schema versioning to support backward compatibility during migration
- Dead letter queue handling for failed event processing
- Consumer SLA dashboards per team

### Theme 4: Cache Architecture (6 mentions)
**Desired state:** Cache invalidation reliable, observable, and with defined TTL strategy per entity type
**Key ideas:**
- TTL + jitter implemented consistently (not ad-hoc)
- Cache invalidation dashboard — when was each cache last refreshed?
- Event-driven cache invalidation (not time-based) for high-churn entities (stock, price)
- Redis cluster health monitoring integrated into team alerting

### Theme 5: Legacy System Exit (5 mentions)
**Desired state:** TGR system fully decommissioned, all data migrated to Product Catalogue
**Key ideas:**
- Hard cutover date agreed with business
- Data migration validation — 100% product parity check before cutover
- Rollback plan tested in non-prod
- Communication plan for teams still consuming TGR endpoints

---

## Current State vs. Target State Gaps

| Area | Current State | Target State | Gap Size |
|------|--------------|--------------|----------|
| Publication latency | ~15 min avg | <3 min | Large |
| GPSR coverage | ~40% products | 100% EU/UK | Large |
| Cache invalidation | Time-based TTL, inconsistent | Event-driven, observable | Medium |
| Consumer pattern | Mix of polling and events | 100% event-based | Medium |
| TGR migration | 40% migrated | 100% (decommission) | Large |
| Monitoring/observability | Basic Azure Monitor | Team dashboards, SLA alerting | Medium |

---

## Voting Results

| Idea | Votes | Effort | Impact | Priority |
|------|-------|--------|--------|----------|
| GPSR compliance validation gate in Publisher | 8 | Medium | High | P0 |
| IndexNow integration (publication ping) | 7 | Low | High | P0 |
| Event-driven cache invalidation for stock/price | 6 | High | High | P1 |
| Publication latency SLA monitoring | 5 | Low | Medium | P1 |
| Consumer modernisation — PDP migration to events | 4 | High | High | P1 |
| TGR decommission date + comms plan | 4 | Medium | High | P1 |
| Cache invalidation dashboard | 3 | Low | Medium | P2 |
| RegTech compliance coverage dashboard | 3 | Medium | Medium | P2 |
| Dead letter queue handling | 2 | Medium | Medium | P2 |
| API consumer SLA dashboards | 2 | Low | Low | P3 |

---

## Prioritised Action Items

| # | Action | Owner | Sprint Target | ADO Story | Status |
|---|--------|-------|---------------|-----------|--------|
| 1 | Create GPSR validation gate initiative in ADO — epic + cascade stories | PM | Sprint 42 | TBC | Pending |
| 2 | IndexNow integration spike — confirm effort estimate with engineering | Engineering Lead | Sprint 42 | TBC | Pending |
| 3 | Publication latency: add SLA monitoring to Azure Monitor | Senior Engineer | Sprint 43 | TBC | Pending |
| 4 | Cache invalidation: spike on event-driven approach for stock/price entities | Senior Engineer | Sprint 43 | TBC | Pending |
| 5 | TGR: schedule decommission decision meeting with business stakeholders | PM | This week | N/A | Pending |
| 6 | Consumer modernisation: communicate deprecation timeline to PDP and Search teams | PM | This week | N/A | Pending |
| 7 | GPSR compliance coverage: add dashboard requirement to backlog | PM | Sprint 44 | TBC | Pending |

---

## Parking Lot (Deferred)

| Idea | Reason Deferred | Revisit |
|------|----------------|---------|
| Full API versioning system | Significant engineering effort; not blocking current work | Q3 2026 |
| Real-time stock push to all consumers | Needs event infrastructure upgrade first | After cache initiative |
| Multi-region Product Catalogue replication | Too early — architecture not stable enough | H2 2026 |
| GraphQL API layer | Not raised by any consumer team as need | Never (no demand) |

---

## Key Discussion Insights

> "We keep saying event-driven but half our consumers are still polling — until we have a migration deadline with a named owner, this won't change."
> — Engineering Lead

**Recurring tension:** Speed-to-market (publish fast) vs. compliance (validate before publish). The team landed on: validate at Publisher (block bad data early), but don't block publication for optional GPSR fields that PIM hasn't provided yet.

**Risk flagged:** If PIM doesn't provide manufacturer data for products, the BackOffice manual entry path becomes the fallback — and that's not scalable. Action: confirm PIM roadmap for GPSR field population.

---

## Artefacts

- Miro board: [URL]
- Architecture diagram (current state): [pp Confluence link]
- Architecture diagram (target state): [pp Confluence link]
- This synthesis page: [pp Confluence URL]

---

*Generated via Miro MCP synthesis — Module 4.5 of the Product Catalogue VSCode PM Course*
*Manual synthesis time: ~90 min | MCP synthesis time: ~8 min*
