# Module 4.4: Figma Collaboration
## Product Catalogue Edition

**Teaching Script for GitHub Copilot**
**Duration:** ~25 minutes

---

## Teaching Flow

Welcome to Module 4.4: Figma Collaboration!

**Important note for Product Catalogue PMs:** Unlike customer-facing teams, your Figma work is mostly internal tooling — the Product Preview tool, BackOffice admin interfaces, Speed-to-Market dashboards, and operational runbook companion screens. This module is framed around that reality.

By the end of this lesson, you'll be able to:
- Browse internal tool design files from VSCode
- Extract specifications from admin interfaces and dashboards
- Reference designs in PRDs and Azure DevOps stories
- Prepare for design handoffs with internal engineering teams

**Current limitation:** Figma MCP has **read-only access**. We can explore and extract, but not edit or comment. Full write access is coming.

STOP: What Figma files do you reference most often? (Product Preview? BackOffice? Speed-to-Market Celonis screens?)

USER: Responds

---

## Why Figma + Copilot for Product Catalogue?

Even with read-only access, the workflow improvement is significant:

**Without MCP:**
1. Ask designer for Figma link → Open in browser → Navigate to the right frame
2. Manually copy measurements, colours, states from inspect panel
3. Paste into PRD or story with manual formatting
4. **Time: 8-10 minutes to extract specs from one component**

**With MCP:**
1. "Get the specs for the manufacturer data section in the BackOffice product detail screen"
2. Copilot extracts: layout, fields, states, copy
3. **Time: 45 seconds**

**Time saved:** ~90% on spec extraction — especially valuable when writing AC for BackOffice stories

STOP: When did you last need to reference a Figma design for a spec or story you were writing?

USER: Responds

---

## Phase 1: Understanding Figma MCP

Let's see what tools are available

STOP: Ask me: "What Figma tools are available through MCP?"

USER: Types command

ACTION: List the available Figma MCP tools and what they can do:

**Current Capabilities (Read-Only):**
✓ Browse file structure and frames
✓ Get component information (name, description, variants)
✓ Extract design properties (measurements, colours, typography)
✓ Get component states (default, hover, disabled, error)
✓ Find where components are used across a file

**Not Yet Available:**
✗ Add or edit design elements
✗ Leave comments on designs
✗ Create new frames or components
✗ Export assets directly

**Key concept:** You'll need the Figma file URL or file key to get started. Your team's design files live in the ASOS Figma workspace.

STOP: Do you have a Figma file URL for a Product Catalogue internal tool design handy?

USER: Responds (may need to ask their designer or find it in team docs)

---

## Phase 2: Browse Internal Tool Design Files

**Exercise 1: List Files and Frames**

STOP: Ask me: "Browse the Figma file for the [Product Preview tool / BackOffice product detail / Speed-to-Market dashboard] — show me the main frames and sections"

USER: Types command with their file URL or file key

ACTION: Use the Figma MCP to:
- List top-level pages in the file
- Show frames within the relevant page
- Identify the frame(s) most relevant to their current work

---

Excellent! You now have a map of the design file without leaving VSCode

Notice how much faster this is than scrolling through Figma's canvas to find the right screen.

**Exercise 2: Get Component Details**

STOP: Ask me: "Get the details of the Manufacturer Information section in the BackOffice product detail screen — fields, layout, and states"

USER: Types command

ACTION: Extract from the relevant frame:
- Section heading and layout
- Input fields (label, placeholder, validation state)
- Field order and grouping
- Required vs optional field indicators

---

## Phase 3: Extract Design Specifications

This is where Figma MCP saves the most time for a platform PM writing PRDs

**Exercise 3: Extract Admin Interface Specs for a Story**

STOP: Ask me: "Extract the full specification for the Manufacturer Data form in BackOffice — I need field names, types, validation rules, and error states to write acceptance criteria"

USER: Types command

ACTION: Extract and format as a structured specification:
- Field name (as labelled in design)
- Input type (text, dropdown, textarea, toggle)
- Placeholder / hint text (exact copy)
- Validation states (empty, filled, error, success)
- Error message copy
- Character limits if visible
- Required/optional indicator

---

**Exercise 4: Extract Typography and Colour Tokens**

For consistency when writing content specs in PRDs

STOP: Ask me: "What typography styles and colours are used in the BackOffice admin interface? I need to document these for the engineering handoff"

USER: Types command

ACTION: List the design tokens:
- Typography: font family, size, weight, line height for each text style
- Colours: hex values and semantic names (error red, success green, primary text, etc.)
- Note: Reference to the ASOS design system if tokens are already documented there

---

## Phase 4: Real Product Catalogue Workflows

**Workflow 1: PRD with BackOffice Design Reference**

Scenario: You're writing the GPSR PRD and need to include the BackOffice design specs

STOP: Ask me: "Get the design specs for the Manufacturer Data section of the BackOffice product detail screen, then add a 'BackOffice Design Reference' section to my GPSR PRD in Confluence"

USER: Types command

ACTION:
1. Extract Figma specs (layout, fields, states)
2. Format as a clear spec table
3. Open Confluence pp space
4. Add the spec section to the GPSR PRD page
5. Add Figma link for full design reference

---

**Workflow 2: Design Review Prep**

Scenario: Weekly design review — you need to check what's changed in the BackOffice designs since last sprint

STOP: Ask me: "Get all frames in the BackOffice design file and summarise what components exist for product management — focus on anything that could be related to GPSR or compliance fields"

USER: Types command

ACTION: Scan the file structure, identify relevant frames, summarise what exists and flag anything new or changed compared to current implementation.

---

**Workflow 3: Engineering Handoff Documentation**

Scenario: BackOffice GPSR screen is approved by design — you need to document it for engineering

STOP: Ask me: "Create a design handoff document for the GPSR Manufacturer Data screen in BackOffice — include all fields, states, validation rules, and any accessibility requirements visible in the design"

USER: Types command

ACTION: Create a structured handoff document:
- Screen overview (purpose, where it appears)
- Field-by-field specification
- Interaction states (empty, filled, error, success)
- Error message copy
- Accessibility requirements visible in design
- Link to full Figma file for engineering reference

---

## Phase 5: Integration with Other Modules

Figma MCP amplifies your existing workflows:

**Module 4.2 (Confluence) + Module 4.4:**
```
Extract BackOffice design specs from Figma
  ↓
Add 'BackOffice Design Reference' section to GPSR PRD in pp space
  ↓
Engineering has specs without asking designer
```

**Module 4.3 (Azure DevOps) + Module 4.4:**
```
Extract Figma component spec for BackOffice screen
  ↓
Add exact field names to User Story AC
  ↓
"AC: Form fields match design: manufacturerName (max 200 chars), manufacturerAddress (multiline, max 500 chars)"
```

STOP: Which integration would save you the most time?

USER: Responds

---

## Common Issues & Solutions

**Issue 1: "File not found"**
- Confirm you have the correct Figma file URL or key
- Verify you have at least View access to the file in Figma
- Ask your designer for the share link

**Issue 2: "Component not found"**
- Use the exact frame/component name as it appears in Figma
- Browse the file structure first to find the correct name
- Designers may have renamed screens — check with them

**Issue 3: "Read-only — can't add comments"**
- This is a current Figma MCP limitation, not an error
- Add design feedback via Confluence comments or Slack to designer
- Full Figma write access is coming in a future MCP update

STOP: Encountered any issues? Let's troubleshoot.

USER: Reports issues or confirms working

---

## Practice Challenge

**Challenge: BackOffice Spec Extraction**

1. Find a Figma file for any internal Product Catalogue tool (BackOffice, Product Preview, or Speed-to-Market)
2. Browse the file structure and find the most relevant admin screen
3. Extract the full field specification for one section
4. Format it as an acceptance criteria table
5. Optionally: add it to a relevant Confluence PRD or Azure DevOps story

STOP: Complete this challenge now. Take 5 minutes.

USER: Works through challenge

ACTION: Guide through each step, provide feedback.

---

## What's Coming: Full Figma MCP Access

When write access is enabled, you'll be able to:
- Add comments directly in Figma (replaces Slack back-and-forth with designers)
- Create new annotation frames
- Mark screens as "PM reviewed"
- Link Figma frames directly to Azure DevOps stories

For a Product Catalogue PM, the biggest unlock will be commenting on BackOffice designs during design review — without switching applications.

---

## Reflection

You can now:
✓ Browse internal tool design files (BackOffice, Product Preview, Speed-to-Market)
✓ Extract field specifications for admin interfaces
✓ Document design specs for engineering handoffs
✓ Reference Figma designs in Confluence PRDs and Azure DevOps stories
✓ Prepare for design review without opening Figma in browser

**Your new superpower:** Design spec extraction in 45 seconds — no more manual copying from Figma inspect panel

**Time saved:** 2-3 hours per week on spec extraction and design documentation

See you in Module 4.5!
