---
context:
  domain: product-catalogue
  type: template
  purpose: User story template for Product Catalogue API and platform feature work
  related:
    - .context/domains/product-catalogue/business-rules.md
  status: template
---

# Product Catalogue — User Story Template

## Title Format

```
[FeatureTag] - Component - Specific Technical Change
```

**Examples:**
- `[FlatLays] - Product Catalogue FullProduct EndPoint - Expose "supplementaryAssets" in API response`
- `[GPSR] - Product Packages Models - Add ManufacturerCompliance class with required fields`
- `[GPSR] - Product Publisher - Map manufacturerContact from PIM ComplianceInfo payload`
- `[INC0548985] - Expiring SPN App Registration - Review and rotate credentials`
- `[SEC] - Product Catalogue API - Remove WIZ security vulnerabilities from acceptance tests`
- `[IndexNow] - Product Catalogue API - Trigger IndexNow ping on product publication`

**Rules:**
- Feature tag in `[brackets]` — always at start
- Component name: the service/repo affected (Product Catalogue API, Product Publisher, Feeds API, Product Packages Models, etc.)
- Change verb: Expose, Add, Update, Remove, Map, Trigger, Fix
- Never vague: not "Improvements", "Review", "Investigate"

---

## Description Format

```
As a developer I want to [add/update/expose/map/remove] [specific field/behaviour/endpoint]
so that [consuming team or system] can [specific outcome].
```

**Examples:**
- `As a developer I want to add showShippedByMessage field in the stock price endpoint response so that the front end can use it to hide relevant messaging for the US store`
- `As a developer I want to expose supplementaryAssets in the FullProduct endpoint so that the PDP team can display flat lay images on the product detail page`
- `As a developer I want to add manufacturer contact fields to the FullProduct response so that PDP can surface GPSR-compliant manufacturer information on the product page`

**Never use:**
- "As a customer/user/shopper..." (this is internal/API work)
- "As the business..." (too vague)
- "As ASOS..." (meaningless)

---

## Technical Specification

```
Technical Specification:
- Field: [fieldName]: [type]                           ← Required if new field
- Location: [where in response — e.g. at variant level, sibling to source]
- Affected endpoints: [list every endpoint explicitly]  ← Never leave blank
- Default value: [if applicable]
- Condition/logic: [business rules, conditions, edge cases]

Updated logic (if replacing existing):
  - Old: [what the current behaviour is]
  - New: [what the new behaviour should be]
```

**Example — StockPrice endpoint change:**
```
Technical Specification:
- Field: showShippedByMessage: boolean
- Location: Sibling to source at variant level in /stockprice and /variants responses
- Affected endpoints: /stockprice, /variants
- Default: true (show source messaging by default)
- Condition: Set to false only when store = US AND sourceType = "marketplace"

Updated logic:
  - Old: Always return source messaging regardless of store
  - New: Return showShippedByMessage: false for US store marketplace variants
```

**Example — New model class:**
```
Technical Specification:
- Class: SupplementaryAssets (new)
- Fields: HasFlatlay: bool, FlatLayImages: List<string>
- Location: New class in product-packages-models repo
- Repo: https://github.com/asosteam/asos-product-packages-models
- Required test fixtures: Add SupplementaryAssets to existing product test fixtures
```

---

## Acceptance Criteria (Definition of Done)

Format as explicit checkboxes. Engineers expect this level of detail.

**For API endpoint changes:**
```
Acceptance Criteria:
- [ ] Field added to [ModelClass] in product-packages-models repo
- [ ] Publisher maps [field] from PIM [source payload/field name]
- [ ] [Endpoint] returns [field] in response
- [ ] Test fixtures updated to include [fieldName] with valid test data
- [ ] Contract test passes for [consuming team] consumer (PDP / Search / Feeds)
- [ ] API documentation updated in Confluence pp space
- [ ] No regression in existing endpoint response times (p99 target: [Xms])
```

**For compliance/restriction changes:**
```
Acceptance Criteria:
- [ ] Rule applied correctly for [region] store products
- [ ] Rule does NOT affect [other region] products
- [ ] Edge case handled: [describe edge case]
- [ ] Compliance validated with Legal sign-off
- [ ] Integration test covers compliant and non-compliant product scenarios
```

**For security/tech debt:**
```
Acceptance Criteria:
- [ ] WIZ scan passes with 0 critical vulnerabilities
- [ ] Dependency [package] upgraded to version [X.X.X]
- [ ] All acceptance tests still pass after upgrade
- [ ] No breaking changes to API contract
- [ ] Key Vault / secrets management unaffected
```

---

## Metadata

```
Priority:    P0 (launch blocker) | P1 (this sprint) | P2 (near-term) | P3 (backlog)
Story Points: [1 | 2 | 3 | 5 | 8] — Fibonacci. PC stories typically 3-5 for API changes.
Tags:        [feature-tag] (e.g. gpsr, flatlays, indexnow)
             [work-type] (e.g. unplanned, cost-savings, security, tech-debt)
Value Area:  Business (all PC stories — even infrastructure — are Business value)
Area Path:   E-Commerce Foundations\Product Catalogue
Iteration:   E-Commerce Foundations\Product Catalogue\[Sprint name]
```

---

## Links (Add All That Apply)

```
Parent: Epic — [Epic title and ID]
PRD:    Confluence pp space — [Page URL]
Repo:   GitHub — [repo URL, e.g. asos-product-catalogue-api, product-packages-models]
Design: Figma — [link if BackOffice or admin UI is affected]
ADO:    Related work items — [IDs for dependent stories]
```

---

## State Machine

```
New → Ready for Refinement → Active → Done

New:                  Created but not yet refined. No engineer assigned.
Ready for Refinement: PM has written full description and AC. Ready for sprint.
Active:               Assigned to engineer, in-sprint.
Done:                 Engineering complete, tested, deployed to appropriate environment.
```

**Assignment rule:** Only assign to an engineer when in Active state. Ready for Refinement = PM work, not engineer work yet.

---

## Cascade Pattern — Multi-Story Epics

When a data model change cascades across services, create linked stories in this order:

```
1. [Feature] - Product Packages Models - [Change to shared model/contract]
       ↓
2. [Feature] - Product Publisher - [Mapping change from PIM]
       ↓
3. [Feature] - Product Catalogue API - [Endpoint exposure change]
       ↓
4. [Feature] - Feeds API - [Downstream feed inclusion] (if feeds affected)
```

Link stories 2-4 as children or related items of story 1.
All stories link to the same parent Epic in Azure DevOps and the same PRD in Confluence.

---

## Complete Example Story

**Title:** `[FlatLays] - Product Catalogue FullProduct EndPoint - Expose "supplementaryAssets" in API response`

**Description:**
```
As a developer I want to expose the supplementaryAssets object in the
FullProduct endpoint response so that the PDP team can display flat lay
images alongside standard product images on the product detail page.

Technical Specification:
- Object: supplementaryAssets: SupplementaryAssets (new nested object)
- Fields within: HasFlatlay: bool, FlatLayImages: List<string> (image URLs)
- Location: At product level in FullProduct response (sibling to images array)
- Affected endpoints: /fullproduct/{productId}, /summaries/{productId}
- Default: supplementaryAssets: null (if no flat lay images exist)
- Condition: Populated only when HasFlatlay = true in Publisher payload

Acceptance Criteria:
- [ ] SupplementaryAssets class added to product-packages-models repo
      (see Story [ID] for models change)
- [ ] Publisher maps FlatLayImages from PIM SupplementaryAssets payload
      (see Story [ID] for publisher change)
- [ ] FullProduct endpoint returns supplementaryAssets object when HasFlatlay=true
- [ ] supplementaryAssets returns null (not empty object) when HasFlatlay=false
- [ ] Test fixtures updated with flat lay product and non-flat-lay product scenarios
- [ ] Contract test passes for PDP web and app consumers
- [ ] Summaries endpoint also updated (separate story or included here — confirm scope)
```

**Tags:** `flatlays`
**Story Points:** 5
**Priority:** P1
**Area Path:** E-Commerce Foundations\Product Catalogue
**Links:** Parent epic: FlatLays [Epic ID] | PRD: [Confluence URL] | Models story: [ID] | Publisher story: [ID]

---

*Template maintained by the Product Catalogue PM. Based on production story patterns from the E-Commerce Foundations board.*
