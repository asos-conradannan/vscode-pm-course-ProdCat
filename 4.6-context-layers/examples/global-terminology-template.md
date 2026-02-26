---
version: 1.0
scope: global
domain: product-catalogue
last_updated: 2026-02-26
owner: Product Catalogue PM
review_cycle: quarterly
---

# Global Terminology — Product Catalogue @ ASOS

**Purpose:** Universal glossary for the Product Catalogue domain at ASOS. Loaded in every Copilot session to ensure consistent language across all PM work.

**Scope:** Terms specific to the Product Catalogue team and its domain. Generic ASOS terms (e.g. OKR, sprint, retro) are not included here.

---

## Publication Pipeline Terms

### PIM
**Definition:** Product Information Management system. The upstream source of truth for all product data at ASOS. Product Catalogue receives events from PIM when products are created, updated, or enriched.
**Context:** Every API endpoint change must trace back to whether PIM provides the data. First question on any new feature: "Does PIM have this field?"
**Related:** Publisher, Product Catalogue API, fat events

### Publisher (Product Publisher)
**Definition:** The service that receives events from PIM and transforms/maps them into the format stored in the Product Catalogue database. It handles enrichment (translations, restriction lookups) before writing to Catalogue.
**Context:** Middle layer in the pipeline. API contract changes that require new fields typically need both a Publisher mapping change AND an API endpoint change.
**Related:** PIM, Product Catalogue, product-packages-models

### Store Product
**Definition:** A language- and store-specific view of a product. One base product in PIM becomes multiple Store Products in Catalogue — one per language/store combination. ASOS supports 245+ countries.
**Context:** When writing stories about "products", clarify whether you mean the base product or a specific Store Product.
**Related:** PIM, Colourway, Translation, Smartling

### Colourway
**Definition:** A colour + style combination for a product. One product page may show multiple colourways (e.g., a dress in red, blue, green). Each colourway has its own stock, images, and variant data.
**Context:** Colourway is a Product Catalogue concept — PIM calls them differently. Important for image management and FlatLays work.
**Related:** SKU, Variant, supplementaryAssets

---

## Stock & Availability Terms

### ATS (Available To Sell)
**Definition:** The count of units available for customer purchase. Calculated from physical stock minus reservations.
**Context:** ATS drives what ASOS shows as "in stock" to customers. Stock events from the fulfillment system update ATS in near-real-time.
**Related:** On-Query, Stock events, Redis cache

### On-Query
**Definition:** The real-time stock status check performed when a customer views a product. Contrasted with cached stock data — on-query is the freshest possible status.
**Context:** Performance-sensitive path. Any change to on-query processing must preserve sub-100ms response time.
**Related:** ATS, Redis cache, FullProduct endpoint

---

## API Terms

### FullProduct Endpoint
**Definition:** `GET /fullproduct/{productId}` — the most comprehensive Product Catalogue API response, including images, pricing, stock, size info, reviews, and restrictions. Primary endpoint consumed by PDP (web + app).
**Context:** Any new data field typically needs to appear in FullProduct first. Other endpoints (Summaries, Variants) may follow.
**Related:** Summaries endpoint, Variants endpoint, StockPrice endpoint, PDP team

### PLP (Product List Page ID)
**Definition:** A categorisation identifier that determines which Product List Pages a product appears on (e.g., "dresses", "sale", "new in"). Managed by the PLP Manager tool used by merchandisers.
**Context:** Redis cache is used for fast PLP lookups. Cache invalidation is a common source of incidents.
**Related:** Redis cache, PLP Manager, Merchandisers

### Feeds API
**Definition:** An API that provides product data to external consumers: marketing email systems, third-party brand feeds, affiliate partners. Changes to the core Product Catalogue data model often cascade to Feeds API.
**Context:** Story cascades: Models → Publisher → Catalogue API → Feeds API. Don't forget Feeds when planning a data model change.
**Related:** product-packages-models, third-party feeds, Marketing

### StockPrice Endpoint
**Definition:** `GET /stockprice/{productId}` — returns stock availability and pricing per variant. Used by checkout and basket pages.
**Context:** Separate from FullProduct — has its own performance requirements and response shape. Changes here affect checkout team, not just PDP.
**Related:** FullProduct endpoint, ATS, Pricing Service

---

## Event Architecture Terms

### Fat Event
**Definition:** An event that includes the full data payload (not just an ID). When PIM sends a fat event, the receiver has all the data it needs without making a follow-up API call.
**Context:** Product Catalogue prefers fat events from PIM to reduce coupling. Fat events are larger but eliminate round-trips.
**Synonyms:** Full payload event
**Related:** Lean event, Azure Service Bus, Publisher

### Lean Event
**Definition:** An event that includes only a reference ID. The receiver must make a follow-up API call to get the full data. Simpler to produce but creates coupling.
**Related:** Fat event, Azure Service Bus

### TTL (Time To Live)
**Definition:** The duration before a cached item expires and is refreshed from the source. Used in Redis for PLP lookups and other cached data.
**Context:** TTL strategy is a trade-off between data freshness and performance. Shorter TTL = fresher data, more database load.
**Related:** Jitter, Redis cache

### Jitter
**Definition:** A random variation added to TTL values to prevent "cache stampede" — where many cache entries expire simultaneously and cause a spike in database load.
**Context:** Always applied alongside TTL in Product Catalogue Redis caching. "TTL + jitter" is a standard pattern.
**Related:** TTL, Redis cache

---

## Compliance Terms

### GPSR (General Product Safety Regulation)
**Definition:** EU and UK regulation requiring products to display manufacturer name, address, and contact information. Enforcement creates a compliance requirement for ASOS's Product Catalogue API to expose this data.
**Context:** An active Q2 2026 initiative. Affects FullProduct endpoint, BackOffice admin, and PDP display. Data sourced from PIM's ComplianceInfo payload.
**Related:** Hazmat restrictions, Brand restrictions, BackOffice, manufacturerName field

### Hazmat Restrictions
**Definition:** Restrictions on products containing hazardous materials (e.g., aerosols, certain chemicals). These products can only be shipped to specific regions (currently UK-only). Enforced via the Product Catalogue restrictions rules engine.
**Context:** Hazmat restrictions are pre-existing compliance logic. GPSR is the new compliance initiative. They coexist in the restrictions layer.
**Related:** GPSR, Brand restrictions, Regional restrictions

### Brand Restrictions
**Definition:** Rules that limit which countries specific brands can be sold and shipped to. Managed by ASOS commercial/legal team and enforced in Product Catalogue's restrictions engine.
**Context:** Separate from hazmat and GPSR — managed by different stakeholders (commercial vs. legal).
**Related:** GPSR, Hazmat restrictions, Regional restrictions

---

## Systems & Tools

### TGR
**Definition:** A legacy system currently being migrated to the Product Catalogue. Products and data that exist in TGR are being progressively migrated.
**Context:** Until migration is complete, some features must work across both TGR and Product Catalogue. Decommission date is a significant milestone.
**Related:** Migration, Product Catalogue, Publisher

### Smartling
**Definition:** Translation management platform used to translate product content (descriptions, care instructions, size guidance) into multiple languages. Part of the publication pipeline.
**Context:** Translations via Smartling are part of what makes publication slow. Azure AI is being piloted as a faster alternative for some content types.
**Related:** Azure AI translation, Store Product, Publication pipeline

### IndexNow
**Definition:** A protocol for notifying search engines (Google, Bing) of new or updated URLs instantly — bypassing the typical crawl delay. Product Catalogue sends IndexNow pings when products are published.
**Context:** Active initiative. Enabling IndexNow for product publications reduces time-to-index from 24h to near-instant, with measurable SEO impact.
**Related:** SEO, Publication pipeline, Speed-to-Market

### BazaarVoice
**Definition:** Third-party platform for product reviews and ratings. Product Catalogue provides a Reviews API that proxies BazaarVoice data for consumption by PDP and other surfaces.
**Context:** Reviews data is not stored in Product Catalogue — it's fetched from BazaarVoice on query. API latency constraints apply.
**Related:** Reviews API, PDP team, FullProduct endpoint

### Speed-to-Market
**Definition:** The time from a product being ready in PIM to that product being live and discoverable on ASOS. Tracked via the Celonis dashboard.
**Context:** A key business metric for the Product Catalogue team. Target: <3 minutes from PIM event to live product. Current: ~15 minutes.
**Related:** Publication pipeline, IndexNow, Celonis

---

## Repositories

### product-packages-models
**Definition:** Shared model library containing the data contracts (C# classes) that define API response shapes. Changes to API response fields require changes to this repo first.
**Context:** The starting point for any cascade of stories. Story 1 of any data model change is always a product-packages-models change.
**GitHub:** `https://github.com/asosteam/asos-product-packages-models`
**Related:** Publisher, Product Catalogue API, Feeds API, user story cascade pattern

### PPAV2
**Definition:** Product Preview API Version 2. An internal tool that allows ASOS business teams to preview product page content before it's live.
**Context:** PPAV2 is a consumer of Product Catalogue data. Security vulnerability remediations in PPAV2 appear as tech-debt stories in the E-Commerce Foundations board.
**Related:** BackOffice, Product Catalogue API

---

## Controlled Vocabulary

### Story Title Format
`[FeatureTag] - Component - Specific Technical Change`

### Valid Feature Tags
`[FlatLays]`, `[GPSR]`, `[IndexNow]`, `[TGR]`, `[SEC]`, `[INC####]`, `[SpeedToMarket]`

### Work Item States
`New` → `Ready for Refinement` → `Active` → `Done`

### Story Priority
`P0` = launch blocker | `P1` = this sprint | `P2` = near-term | `P3` = backlog

---

*Maintained by the Product Catalogue PM. Update when new domain terms emerge.*
*Version history: see pp Confluence space for change log.*
