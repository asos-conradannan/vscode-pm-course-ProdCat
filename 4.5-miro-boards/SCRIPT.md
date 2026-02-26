# Module 4.5: Miro Visual Collaboration
## Product Catalogue Edition

**Teaching Script for GitHub Copilot**
**Duration:** ~25 minutes

---

## Teaching Flow

Welcome to Module 4.5: Miro Visual Collaboration!

As a Product Catalogue PM, you use Miro differently from customer-facing teams. You're not mapping customer journeys — you're running architecture reviews, capturing publication pipeline decisions, planning quarterly roadmaps, and synthesising retrospectives focused on system reliability and tech debt.

This module is built around those real workflows.

By the end of this lesson, you'll be able to:
- Extract sticky notes and cards from Product Catalogue Miro boards
- Synthesise architecture review outputs automatically
- Turn retro boards into structured action items
- Convert quarterly planning cards into a Confluence roadmap doc

STOP: How many hours do you typically spend synthesising Miro board output after a workshop or retro?

USER: Responds

---

## Why Miro + Copilot for Product Catalogue?

**The architecture review problem:**
- You run a 90-minute architecture target state session
- 40+ sticky notes across: publication pipeline, cache strategy, API consumer dependencies, tech debt, migration
- Traditional synthesis: 90 minutes manually grouping, writing up, sharing
- With Copilot: 8 minutes

**The retro problem:**
- Sprint retro captures: PIM sync delays, cache invalidation issues, missing acceptance criteria, unplanned incidents (INC)
- Traditional synthesis: 20 minutes, often skipped
- With Copilot: 3 minutes → synthesis sent to team Slack before lunch

STOP: What's the most recent Miro board your team used? Architecture? Retro? Quarterly planning?

USER: Responds

---

## Phase 1: Connect to Miro Boards

Let's see what tools are available

STOP: Ask me: "What Miro tools are available through MCP?"

USER: Types command

ACTION: List the available Miro MCP tools:
- `board_list_items` — Get all items on a board (sticky notes, cards, shapes, frames)
- `board_get_items` — Get items with pagination
- `board_get_image_data` — Get image data for visual items
- `context_explore` — Explore board structure (frames, sections)
- `context_get` — Get content from a specific board area

Explain: To use these, you need the Miro board ID from the URL. The board URL format is:
`https://miro.com/app/board/{boardId}/`

STOP: Do you have a Miro board URL for the Product Catalogue team? (Retro board, architecture board, or planning board)

USER: Provides board URL or says they need to find one

ACTION: Help them extract the board ID from the URL if they have one.

---

## Phase 2: Extract Board Content

**Exercise 1: Get All Board Items**

STOP: Ask me: "Get all items from the Product Catalogue Miro board at [board URL] — show me what's there"

USER: Types command with board URL

ACTION: Fetch all items. Categorise by type:
- Sticky notes (the primary content)
- Cards
- Text elements
- Shapes and connectors (architecture diagrams)
- Frames (sections)

Show count by type: "Found 47 sticky notes across 6 frames"

---

**Exercise 2: Filter by Frame — Architecture Only**

STOP: Ask me: "From that board, get only the sticky notes in the 'Current State' and 'Target State' frames"

USER: Types command

ACTION: Filter board items by frame name. Show:
- Current state stickies (grouped by theme if obvious)
- Target state stickies
- Note any connectors/arrows showing relationships between states

---

**Exercise 3: Extract and Categorise Retro Themes**

STOP: Ask me: "From the Product Catalogue retro board, extract all sticky notes and group them by category: 'What went well', 'What didn't go well', 'Action items'"

USER: Types command

ACTION: Parse sticky notes by colour or frame category. Show:
- Positives grouped (what worked — with count)
- Issues grouped (what didn't work — with count)
- Actions (assigned or unassigned)
- Flag: items mentioned more than once (recurring issues)

---

## Phase 3: Synthesise Workshop Outputs

This is where the real time saving happens

**Exercise 4: Architecture Review Synthesis**

Scenario: You ran an architecture target state session — 50 stickies across current state, target state, gaps, and risks

STOP: Ask me: "Synthesise the Product Catalogue architecture board: identify the top 5 themes from target state stickies, the biggest gaps between current and target state, and create a prioritised action list"

USER: Types command

ACTION:
1. Extract stickies from all frames
2. Group by theme (publication pipeline, caching, API design, compliance, monitoring)
3. Identify gaps (what's in current state but not target state)
4. Identify risks (items in Risk or Concern frame)
5. Create prioritised action list with themes sorted by mention frequency

---

**Exercise 5: Retro Action Item Extraction**

STOP: Ask me: "From the retro board, extract all action items with owner names, create a formatted list, and suggest which ones should become Azure DevOps stories vs. Slack conversations"

USER: Types command

ACTION:
1. Find all items in the "Actions" or "Next Steps" frame
2. Extract: action text, owner (if written on sticky), category
3. Suggest routing:
   - Engineering improvement → Azure DevOps story
   - Process change → Confluence doc update
   - Quick discussion → Slack thread
   - Team norm → retrospective notes

---

## Phase 4: Create Documentation from Boards

**Exercise 6: Convert to Confluence Doc**

STOP: Ask me: "Create a Confluence page in the pp space titled 'Product Catalogue Architecture Target State — Q2 2026' using the synthesis from the Miro board — include: executive summary, key themes, current-to-target gaps, and prioritised actions"

USER: Types command

ACTION:
1. Use the synthesised content from Exercise 4
2. Format as a proper Confluence page
3. Add: board link, date, participants
4. Create page in pp space
5. Return URL

---

**Exercise 7: Generate Executive Summary**

STOP: Ask me: "Write a 3-paragraph executive summary of the Product Catalogue architecture review — suitable for sharing with the Head of Engineering — focusing on: where we want to be, what's stopping us, and what we're doing about it"

USER: Types command

ACTION: Generate executive summary:
- Para 1: Target state vision (speed-to-market, reliability, compliance)
- Para 2: Key gaps and risks identified
- Para 3: Prioritised actions and next steps

---

## Phase 5: Real Product Catalogue Workflows

**Workflow 1: Sprint Retrospective Processing (3 minutes post-retro)**

STOP: Ask me: "Get the Product Catalogue retro board from last sprint, extract all stickies, identify the top 3 recurring themes in 'What didn't go well', and create Azure DevOps stories for any systemic issues (not one-off incidents)"

USER: Types command

ACTION:
1. Extract retro board
2. Group "didn't go well" stickies by theme
3. Count mentions per theme
4. Flag recurring themes (3+ mentions): e.g., "PIM sync delay", "unplanned INC work", "missing AC at sprint start"
5. Create Azure DevOps stories for systemic issues
6. Route one-off incidents to Slack or as comments on existing INC stories

---

**Workflow 2: Quarterly Roadmap from Planning Board**

Scenario: Q2 planning is done on Miro — you need a Confluence roadmap page and Azure DevOps epics

STOP: Ask me: "Get the Q2 planning board for Product Catalogue, extract all roadmap cards, group by initiative theme, and create: (1) a Confluence roadmap page in pp space, (2) a summary suitable for the leadership update"

USER: Types command

ACTION:
1. Extract roadmap cards (often colour-coded by stream)
2. Group by stream: Compliance, API Evolution, Performance, Tech Debt
3. Create Confluence page with initiative table (initiative, owner, sprint target, status)
4. Generate leadership summary: top 3 bets for Q2 and expected outcomes

---

**Workflow 3: Cross-Sprint Architecture Pattern Analysis**

Scenario: You want to know if the same architecture issues keep appearing across retros

STOP: Ask me: "Compare the last 3 Product Catalogue retro boards — identify themes that appeared in 2+ retros. These are systemic issues that need an initiative, not just a sprint fix."

USER: Types command

ACTION:
1. Get 3 retro boards (by URL/ID)
2. Extract "didn't go well" stickies from each
3. Find common themes across boards
4. Flag: recurring issues (appearing 2+ times = systemic)
5. Suggest: create a dedicated initiative or spike to address each systemic issue

---

## Phase 6: Integration with Other Modules

**Miro → Confluence → Azure DevOps chain:**
```
Architecture review session (Miro board)
  ↓
Synthesise target state and gaps (Module 4.5)
  ↓
Create Confluence architecture page in pp space (Module 4.2)
  ↓
Create Azure DevOps epics for each gap initiative (Module 4.3)
  ↓
Link Confluence page to epics for traceability
```

STOP: How would this chain change your current post-workshop workflow?

USER: Responds

---

## Common Issues & Solutions

**Issue 1: "Board not found"**
- Confirm you have the correct board ID from the URL
- Verify you have access to the board in Miro
- Check board visibility settings (some boards are private to individuals)

**Issue 2: "Too many items — response truncated"**
- Use frame filtering to scope to specific sections
- Process one frame at a time
- Use `board_get_items` with pagination parameters

**Issue 3: "Sticky note text not extracted"**
- Some items may be images, not text — check with `board_get_image_data`
- Very small sticky notes may not parse well
- Handwritten-style fonts may need manual transcription

STOP: Encountered any issues? Let's troubleshoot.

USER: Reports issues or confirms working

---

## Practice Challenge

**Challenge: Full Retro Processing Workflow**

1. Get the most recent Product Catalogue retro board
2. Extract and categorise all sticky notes (well / not well / actions)
3. Identify the top recurring theme in "didn't go well"
4. Create an Azure DevOps story for that systemic issue
5. Create a brief Confluence page: "[Date] Product Catalogue Retrospective — Key Actions"
6. Time yourself — compare to your manual process

STOP: Complete this challenge now.

USER: Works through challenge

ACTION: Guide through each step, celebrate completion.

---

## Time Savings Analysis

**Traditional post-workshop workflow:**
- Photograph board / export stickies: 5 min
- Manually transcribe and group: 45 min
- Write synthesis document: 20 min
- Create action items in ADO: 15 min
- Share with team: 5 min
- **Total: 90 minutes**

**With Miro MCP:**
- Extract and synthesise: 3 min
- Create Confluence doc: 2 min
- Create ADO stories: 3 min
- Share: 1 min
- **Total: 9 minutes**

**Time saved: 81 minutes per session = ~90% reduction**

STOP: How many workshops or retros does your team run per sprint?

USER: Responds

ACTION: Calculate monthly savings.

---

## Best Practices for Product Catalogue Boards

**DO:**
✓ Use clear frame names in Miro (Current State, Target State, Risks, Actions) — makes extraction much cleaner
✓ Ask participants to label owners on action sticky notes
✓ Process retro boards within 24 hours — before context fades
✓ Save board IDs in your `.context/` files for quick access (Module 4.6)

**DON'T:**
✗ Rely on colour alone to categorise stickies — add text labels too
✗ Skip recurring theme analysis across retros — this is where systemic fixes come from
✗ Forget to link the Miro board in the Confluence synthesis page

STOP: Any questions about best practices?

USER: Asks questions or confirms

---

## Next Steps

You've mastered Miro MCP for Product Catalogue workshops! Next: **Context Layer Setup (Module 4.6)**

**Before Module 4.6:**
1. List the context you have to re-explain every time you start a new Copilot session (what is PIM, who are the stakeholders, what's GPSR...)
2. Find one architecture doc and one feature log page in pp space that defines your domain
3. Think about what Copilot would need to know to be a truly useful PC PM assistant from day one

**In Module 4.6, you'll learn:**
- Build `.context/` files so Copilot permanently knows your domain
- Use Confluence MCP to bootstrap context from pp space docs
- The "zero to context" workflow — never re-explain your domain again

STOP: Ready for the final (and most powerful) module?

USER: Confirms

---

## Reflection

You can now:
✓ Extract all content from Product Catalogue Miro boards
✓ Synthesise architecture review outputs in minutes
✓ Process retro boards and route actions to the right place
✓ Generate Confluence docs directly from Miro workshop content
✓ Identify systemic issues across multiple retros
✓ Connect Miro synthesis into your Confluence + Azure DevOps workflow

**Your new superpower:** 9 minutes from workshop end to action items distributed — no more Monday morning catch-up synthesis

**Time saved:** 80+ minutes per workshop, every sprint

See you in Module 4.6 — the game-changer!
