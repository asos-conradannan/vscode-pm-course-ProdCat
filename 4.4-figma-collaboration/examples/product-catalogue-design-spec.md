---
context:
  domain: product-catalogue
  type: example
  purpose: Design spec example for BackOffice internal tool — shows extraction output format
  related:
    - .context/domains/product-catalogue/terminology.md
status: example
---

# BackOffice — GPSR Manufacturer Data Screen
## Design Specification (Extracted via Figma MCP)

**File:** ASOS BackOffice Admin — Product Detail
**Frame:** Product Detail > Compliance Tab > Manufacturer Information
**Extracted:** [Date]
**Designer:** [Designer Name]
**Figma Link:** [URL]
**Status:** Approved for development

---

## Screen Overview

**Location:** BackOffice → Product Detail → Compliance tab
**Purpose:** Allow ASOS operations team to view and edit GPSR manufacturer contact information for individual products
**Users:** ASOS Operations, Merchandisers (edit), Legal/Compliance (view)
**Current State:** Does not exist — new screen for GPSR compliance initiative

---

## Layout

```
┌─────────────────────────────────────────────┐
│  Product Detail — [Product ID: 12345678]    │
│  [Summary] [Images] [Pricing] [Compliance]  │
│                                             │
│  MANUFACTURER INFORMATION                   │
│  ────────────────────────                   │
│                                             │
│  Manufacturer Name *               [_____]  │
│  Manufacturer Address *            [_____]  │
│                                    [_____]  │
│  Manufacturer Contact (email/phone)[_____]  │
│                                             │
│  GPSR Status:  ● Compliant / ○ Incomplete   │
│                                             │
│           [Cancel]  [Save Changes]          │
└─────────────────────────────────────────────┘
```

**Layout:** Single-column form within a tab panel
**Tab label:** "Compliance"
**Section heading:** "Manufacturer Information" (body bold, #1A1A1A)

---

## Typography

| Element | Font | Size | Weight | Colour |
|---------|------|------|--------|--------|
| Tab heading | ASOS sans | 14px | 600 | #1A1A1A |
| Section heading | ASOS sans | 16px | 600 | #1A1A1A |
| Field label | ASOS sans | 14px | 400 | #4A4A4A |
| Input text | ASOS sans | 14px | 400 | #1A1A1A |
| Placeholder text | ASOS sans | 14px | 400 | #9B9B9B |
| Error message | ASOS sans | 12px | 400 | #CC0000 |
| Status badge | ASOS sans | 12px | 600 | #117A3D (compliant) / #8C4700 (incomplete) |

---

## Field Specifications

### Manufacturer Name

| Property | Value |
|----------|-------|
| Label | "Manufacturer Name" |
| Required | Yes (*) |
| Input type | Single-line text |
| Max length | 200 characters |
| Placeholder | "Enter manufacturer or brand name" |
| Validation | Cannot be empty if product sold in EU/UK |
| Error state | "Manufacturer name is required for EU/UK products" |

### Manufacturer Address

| Property | Value |
|----------|-------|
| Label | "Manufacturer Address" |
| Required | Yes (*) |
| Input type | Multiline textarea |
| Rows | 3 |
| Max length | 500 characters |
| Placeholder | "Street address, city, country" |
| Validation | Cannot be empty if product sold in EU/UK |
| Error state | "Manufacturer address is required for EU/UK products" |

### Manufacturer Contact

| Property | Value |
|----------|-------|
| Label | "Manufacturer Contact (email or phone)" |
| Required | No |
| Input type | Single-line text |
| Max length | 150 characters |
| Placeholder | "e.g. compliance@manufacturer.com or +44 20 1234 5678" |
| Validation | Valid email or phone format if populated |
| Error state | "Enter a valid email address or phone number" |

---

## Component States

### GPSR Status Indicator

| State | Icon | Badge colour | Label |
|-------|------|-------------|-------|
| All required fields populated | ● | #117A3D (green) | "Compliant" |
| Required fields missing | ○ | #8C4700 (amber) | "Incomplete" |
| Not applicable (non-EU/UK product) | — | #9B9B9B (grey) | "Not required" |

**Note:** Status is read-only. It derives from whether required fields are populated.

### Form Actions

| Button | Style | Behaviour |
|--------|-------|-----------|
| Save Changes | Primary CTA | Validates → Saves → Shows success toast → Updates GPSR status |
| Cancel | Secondary | Discards unsaved changes → Confirmation dialog if changes detected |

### Success Toast

Text: "Manufacturer information saved. GPSR status updated to Compliant."
Duration: 4 seconds
Position: Bottom right
Colour: #117A3D background, white text

### Unsaved Changes Warning

Trigger: User navigates away or clicks Cancel with unsaved changes
Text: "You have unsaved changes. Are you sure you want to leave?"
Actions: "Continue editing" (dismiss) | "Leave without saving" (discard)

---

## Accessibility

- All required fields marked with asterisk (*) and aria-required="true"
- Error messages linked to fields via aria-describedby
- Status indicator has text label, not colour-only
- Tab order: Name → Address → Contact → Cancel → Save
- Minimum contrast ratio met for all text (WCAG AA)

---

## Interactions

1. **Auto-save:** Not implemented — explicit "Save Changes" required
2. **GPSR status update:** Recalculated after Save — not real-time while typing
3. **Read-only mode:** If user lacks edit permissions, all fields display as text (not inputs), Save button hidden
4. **Multiple products:** This screen is per-product only — no bulk edit in this phase (P2)

---

## Engineering Notes

- Field names as they should appear in BackOffice API payload: `manufacturerName`, `manufacturerAddress`, `manufacturerContact`
- These must match fields returned by the Product Catalogue API (`/fullproduct/{productId}` compliance object)
- GPSR status is derived server-side from field presence — frontend does not calculate this
- Validation for EU/UK scope requires knowing the product's store restrictions (consult Backend team)

---

*Extracted using Figma MCP — Module 4.4 of the Product Catalogue VSCode PM Course*
