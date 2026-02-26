---
version: 1.0
domain_id: product-catalogue
last_updated: 2026-02-26
owner: Product Catalogue PM
status: active
team: E-Commerce Foundations / Product Catalogue (Azure DevOps)
confluence_space: pp
---

# Domain: Product Catalogue @ ASOS

## Overview

### Mission
Provide a reliable, scalable, and compliant product data platform that powers ASOS's ecommerce experience — ensuring every product is discoverable, accurately described, and legally compliant across 245+ countries.

### Scope
**In scope:**
- Product publishing pipeline (PIM → Publisher → Catalogue → APIs)
- Product Catalogue API (FullProduct, Summaries, Variants, StockPrice, PLP, Reviews)
- Data enrichment (translations, restrictions, colourway management)
- Compliance rules engine (GPSR, hazmat, brand restrictions)
- Stock and availability data
- SEO infrastructure (IndexNow, retired product detection)
- Internal tooling (Product Preview, BackOffice product data management)

**Out of scope:**
- PIM itself (owned by a separate team)
- Customer-facing PDP UI (owned by PDP team — we provide the API)
- Pricing logic (owned by Pricing Service — we surface it)
- Search ranking and personalisation (owned by Search/Recommendations — we provide data)
- Order management, checkout, payments

### Boundaries
Product Catalogue is the **data platform** — it owns the data contract and the API. Consuming teams (PDP, Search, Recommendations, BackOffice) own their own presentation and logic.

---

## Business Context

### Problem Space
1. Products must move from PIM to live on ASOS website in under 3 minutes (Speed-to-Market)
2. All EU/UK products must display GPSR-compliant manufacturer data (regulatory compliance)
3. Product data must be accurate across 245+ countries with regional restrictions
4. API consumers (PDP, Search, Recommendations) need reliable, low-latency data

### Internal Users (API Consumers)
| Consumer | Primary Endpoint | SLA | Key Concern |
|----------|-----------------|-----|-------------|
| PDP team (web + app) | FullProduct | <100ms p99 | Data freshness, image availability |
| Search (Fredhopper/Algolia) | Summaries, PLP | <200ms p99 | Index completeness, PLP categorisation |
| Recommendations | Summaries | <200ms p99 | Category accuracy, image availability |
| BackOffice | Product Preview API | <500ms p99 | Data accuracy, edit capability |
| Marketing email feeds | Feeds API | Batch | Product availability, pricing accuracy |
| Third-party brand feeds | Feeds API | Batch | Contract stability, field completeness |

### Value Proposition
Product Catalogue is the single source of truth for all product data consumed across ASOS. Without it, nothing is discoverable.

---

## Domain Model

### Core Entities

**Product**
- Definition: A unique product in the ASOS catalogue (identified by productId)
- Key attributes: title, description, brand, category, images, prices, variants
- Lifecycle: Draft → Published → Active → Retired

**Colourway**
- Definition: A colour + style variant of a product
- Key attributes: colourwayId, images, variants (sizes), stock by variant
- Relationships: Child of Product; parent of Variant

**Variant**
- Definition: A specific size + colour combination (the item a customer adds to basket)
- Key attributes: variantId (SKU), size, ATS (Available To Sell), price, restrictions
- Lifecycle: In stock → Low stock → Out of stock → Discontinued

**Store Product**
- Definition: Language- and store-specific view of a product
- Key attributes: language, store, translated content, store-specific restrictions
- Relationships: One Product → many Store Products

---

## Business Rules

*See `.context/domains/product-catalogue/business-rules.md` for full details.*

### Publication Pipeline Rules (Summary)
1. Product data flows: PIM → Publisher → Product Catalogue → API
2. Publisher processes fat events from PIM — full payload, no follow-up calls
3. Store Products are created per language/store combination after base product is published
4. Translations are enriched during publishing (Smartling for human translation, Azure AI for bulk)
5. Restrictions are applied at publish time, not at query time

### Cache Rules (Summary)
- Redis cache for PLP lookups: TTL + jitter (varies by entity type)
- Event-driven cache invalidation for high-churn entities (stock, price)
- Cache invalidation must not block publication pipeline

### Compliance Rules (Summary)
- GPSR: Required for all products sold in EU/UK — manufacturer name, address, and contact
- Hazmat: UK-only shipping restriction — enforced via restrictions rules engine
- Brand restrictions: Commercial/legal agreements — enforced at product level

---

## Workflows

*See `.context/domains/product-catalogue/workflows.md` for full details.*

### Key Workflows (Summary)
1. **Product publication:** PIM event → Publisher → Catalogue → API → IndexNow ping
2. **Stock update:** Fulfillment event → Stock service → Catalogue → Redis cache invalidation
3. **Compliance update:** BackOffice/PIM update → Publisher → Catalogue → Consumer cache invalidation
4. **Sprint cadence:** 2-week sprints; refinement on week 1; sprint planning on week 2 start

---

## Technical Context

### Architecture
```
PIM ──[fat events]──► Publisher ──► Product Catalogue DB (Cosmos DB/Mongo)
                           │                     │
                           │              [Redis Cache]
                           │                     │
                           │              ┌──────┴──────┐
                           │              ▼             ▼
                     [Stock events]   REST APIs    [Feeds API]
                           │              │
                     Fulfillment      ┌───┴──────────────────┐
                        System        │   PDP    │  Search   │
                                      │   Team   │  Fredh.   │
                                      │  BackOff │  Recs.    │
                                      └──────────┴───────────┘
```

### Tech Stack
- **Database:** Azure Cosmos DB (MongoDB API)
- **Cache:** Redis (Azure Cache for Redis)
- **Events:** Azure Service Bus
- **Languages:** C# (.NET)
- **Shared models:** `asos-product-packages-models` (GitHub)
- **API style:** REST (JSON)
- **Translation:** Smartling (human), Azure AI (bulk/automated)
- **Monitoring:** Azure Monitor, Celonis (Speed-to-Market)

### Azure DevOps
- **Project:** E-Commerce Foundations
- **Team:** Product Catalogue
- **Board URL:** https://dev.azure.com/asos/E-Commerce%20Foundations/_boards/board/t/Product%20Catalogue/Stories
- **Story format:** `[FeatureTag] - Component - Specific Technical Change`
- **Description format:** "As a developer I want to..."
- **Story types:** User Story, Bug, Spike, Task

### Confluence
- **Space key:** `pp`
- **Root page ID:** 4506256613
- **Key pages:** Feature Log, Architecture diagrams, API contracts, Decision records

---

## Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Speed-to-Market | <3 min PIM → live | Celonis dashboard |
| API latency (FullProduct p99) | <100ms | Azure Monitor |
| Publication SLA | 99.9% within target | Azure Monitor |
| GPSR compliance coverage | 100% EU/UK products | Databricks query |
| Unplanned work % | <20% of sprint | ADO tags analysis |

---

## Stakeholders

| Name | Role | Team | Engagement |
|------|------|------|-----------|
| [Engineering Lead] | Engineering Lead | Product Catalogue | Daily collaboration |
| [PDP Lead] | API Consumer | PDP team | Sprint-by-sprint alignment on API changes |
| [Search Lead] | API Consumer | Search/Fredhopper | Quarterly API contract review |
| [BackOffice Lead] | API Consumer | BackOffice | GPSR initiative; BackOffice design review |
| [Legal/Compliance] | Compliance sign-off | Legal | GPSR enforcement timeline |
| [Data Engineering] | PIM roadmap owner | Data Engineering | PIM field availability for GPSR |

---

## Related Resources

### Documentation
- [Domain business rules](./business-rules.md)
- [Domain workflows](./workflows.md)
- [Domain terminology](./terminology.md)
- [Global terminology](../../global/terminology.md)
- [Internal API consumers (personas)](../../global/personas.md)

### External
- pp Confluence space: https://asoscom.atlassian.net/wiki/spaces/pp
- Azure DevOps board: https://dev.azure.com/asos/E-Commerce%20Foundations/_boards/board/t/Product%20Catalogue/Stories
- Product Catalogue root page (ID: 4506256613)

---

*Maintained by Product Catalogue PM. Bootstrap from pp Confluence space using Module 4.6 power move.*
