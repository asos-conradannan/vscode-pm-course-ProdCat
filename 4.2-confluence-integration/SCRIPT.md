# Module 4.2: Confluence Integration
## Product Catalogue Edition

**Teaching Script for GitHub Copilot**
**Duration:** ~30 minutes

---

## Teaching Flow

Welcome to Module 4.2: Confluence Integration!

You're about to connect Copilot to the pp space — the Product Catalogue team's knowledge base — and eliminate hours of documentation busywork

By the end of this lesson, you'll be able to:
- Search the pp Confluence space without opening a browser
- Read feature logs, API contracts, and PRDs from VSCode
- Create and update documentation: feature docs, API contracts, status updates
- Extract integration dependencies and business rules from team docs

STOP: How much time do you spend in Confluence each week? (Just pp space — not including other spaces)

USER: Responds

---

## Why Confluence + Copilot for Product Catalogue?

Let me show you the workflow transformation for your specific work:

**Current workflow (without MCP):**
1. Switch to browser → Open pp space
2. Navigate to Feature Log or search for the right doc
3. Find the GPSR requirements page (scan 15+ search results)
4. Copy the relevant API requirements
5. Switch to Azure DevOps → Paste into story description
6. Format the acceptance criteria manually
7. **Time: 10-15 minutes per story**

**New workflow (with MCP):**
1. "Search pp space for GPSR manufacturer data requirements"
2. Copilot shows results with summaries
3. "Using those requirements, write acceptance criteria for a story to add manufacturer contact fields"
4. **Time: 90 seconds**

**Time saved:** ~90% reduction per story that references Confluence docs

STOP: What do you search the pp space for most often? (Feature log? API contracts? Architecture docs?)

USER: Responds

---

## Phase 1: Explore Confluence MCP

First, let's see what tools are available

STOP: Ask me: "What Confluence tools are available through MCP?"

USER: Types command

ACTION: List the available Confluence MCP tools:
- `confluence_search` — Search Confluence content across spaces
- `confluence_get_page` — Get page by title
- `confluence_get_page_by_id` — Get page by ID (faster and more precise)
- `confluence_get_page_children` — List child pages under a parent
- `confluence_create_page` — Create new page in a space
- `confluence_update_page` — Update existing page content
- `confluence_add_comment` — Add comment to a page
- `confluence_search_in_space` — Search within a specific space (use `pp` for Product Catalogue)

Explain that these tools connect directly to ASOS's Confluence instance using your existing permissions.

---

Great! The key tool you'll use most is `confluence_search_in_space` with space key `pp` — this keeps searches focused on Product Catalogue documentation.

**Authentication:** The MCP uses your Atlassian session, so you'll need to be logged into Confluence in your browser

STOP: Are you currently logged into ASOS Confluence?

USER: Confirms or needs to log in

---

## Phase 2: Search the pp Space

Let's start with the most common task: finding information in your team's space

**Exercise 1: Basic pp Space Search**

STOP: Ask me: "Search the pp Confluence space for pages about the product publication pipeline"

USER: Types command

ACTION: Use `confluence_search_in_space` with spaceKey `pp`. Show:
- Page titles with brief excerpts
- Space name (should all be `pp`)
- Last updated dates
- Page IDs for deeper access

---

Excellent! You just searched the pp space without opening a browser

Notice how you got:
- Results focused on your team's space (not all of ASOS Confluence)
- Enough context to decide which pages matter
- Page IDs for precise access

**Exercise 2: Targeted Search — Current Initiatives**

Now let's be more specific

STOP: Ask me: "Search the pp Confluence space for FlatLays or IndexNow pages created or updated in the last 3 months"

USER: Types command

ACTION: Perform targeted search in pp space with date filter. Explain how the query works and how date filtering helps find current work vs. archived docs.

---

**Pro Tip: Search Operators for Confluence**

Use these patterns when searching:
- `space:pp` — Always scope to Product Catalogue space
- `"exact phrase"` — Find exact matches (e.g., "manufacturer contact")
- `type:page` — Filter to pages only (exclude blog posts, comments)
- `created > now("-3M")` — Last 3 months
- `ancestor:4506256613` — Under the Product Catalogue root page

STOP: What's a specific search you do regularly in the pp space? Let's try it now.

USER: Describes their use case

ACTION: Help them construct and execute the targeted search query.

---

## Phase 3: Read pp Space Content

Searching is useful, but let's go deeper

**Exercise 3: Get the Product Catalogue Feature Log**

Let's read the team's feature log — the living document of active initiatives

STOP: Ask me: "Get the child pages under the Product Catalogue main page (ID: 4506256613) in the pp space"

USER: Types command

ACTION: Use `confluence_get_page_children` with pageId `4506256613`. Display:
- List of child pages (Feature Log, Architecture docs, API contracts, etc.)
- Note the page IDs for the Feature Log and any PRD pages

---

Now let's read a specific page in full

STOP: Ask me: "Get the full content of the Product Catalogue Feature Log page"

USER: Types command (use the page ID from the previous result)

ACTION: Fetch the page using `confluence_get_page_by_id`. Display:
- Page title
- Content in readable format (list of active initiatives with status)
- Author and last modified date

---

**Exercise 4: Extract Integration Dependencies**

This is where it gets powerful for a platform PM

STOP: Ask me: "From the feature log content, extract: which initiatives have API consumer dependencies (PDP, Search, Marketing), which are blocked, and which are on track"

USER: Types command

ACTION: Analyse the feature log content and produce a structured summary:
- Active initiatives with consumer impact
- Blocked items and blockers
- Items close to launch

This is the synthesis a platform PM needs — from a single Copilot command.

---

## Phase 4: Create Confluence Pages

Now the powerful part — **writing** to the pp space from VSCode

STOP: Ask me: "Activate Confluence page management tools"

USER: Types command

ACTION: Confirm page creation/update capabilities are active for the pp space.

---

**Exercise 5: Create a GPSR PRD Draft**

Let's create a new PRD page — using Product Catalogue terminology from the start

STOP: Ask me: "Create a new Confluence page titled 'GPSR Manufacturer Data - PRD Draft' in the pp space under the Product Catalogue main page"

USER: Types command

ACTION: Create the page using `confluence_create_page`. The page should include:
- PRD structure (using the product-catalogue-prd-template.md from this module)
- Correct parent page (Product Catalogue root)
- Status header: Draft | Owner: [their name] | Sprint: current

Return the page URL and page ID.

---

Fantastic! You just published to the pp space without leaving VSCode

**Exercise 6: Create an API Contract Page**

Now a workflow unique to platform PMs — creating API contract documentation

STOP: Ask me: "Create a new Confluence page titled 'Product Catalogue API - GPSR Fields Contract v1' in the pp space with: endpoint affected (FullProduct), new fields (manufacturerName: string, manufacturerAddress: string, manufacturerContact: string), required for GPSR compliance"

USER: Types command

ACTION:
1. Create the page with OpenAPI-style field documentation
2. Include: field name, type, location in response (at product level), required/optional, validation rules
3. Add: "Consuming teams: PDP (web + app), BackOffice admin"
4. Provide the URL and page ID

This is a real workflow — API contract doc before implementation begins.

---

## Phase 5: Update Existing Pages

Pages evolve with sprints. Let's keep them current.

**Exercise 7: Update the Feature Log**

STOP: Ask me: "Update the Feature Log page — add a new entry under Active Initiatives: 'GPSR Compliance | Owner: [your name] | Target: Sprint 42 | Status: In Discovery | API Impact: FullProduct endpoint, BackOffice Admin, PDP display'"

USER: Types command

ACTION: Use `confluence_update_page` to:
1. Fetch current content
2. Append the new feature log entry in the same format as existing entries
3. Update the page
4. Confirm with new version number

---

**Pro Tip: Iterative Documentation**

Work iteratively in conversation — the way a platform PM actually refines PRDs

STOP: Try: "Now update the GPSR PRD page — add an 'API Contract' section linking to the contract page we just created, and add a 'Compliance Constraint' section noting GPSR enforcement date and scope (all products sold in EU/UK)"

USER: Types command

ACTION: Update the PRD page with the new sections, preserving existing content.

---

## Phase 6: Real Product Catalogue Workflows

Let's combine everything into workflows you'll use every sprint

**Workflow 1: API Contract Documentation**

Scenario: A new endpoint change is approved — you need to document the contract before dev starts

STOP: Ask me: "Search the pp space for existing Product Catalogue API contract documentation pages, then help me create a new contract page for adding supplementaryAssets to the FullProduct endpoint"

USER: Types command

ACTION:
1. Search pp space for existing API contract docs
2. Read the format/structure of existing contracts
3. Create the new contract page matching that structure
4. Return URL to share with engineering and consuming teams (PDP, Search)

---

**Workflow 2: Feature Log Synthesis**

Scenario: Leadership asks for a cross-initiative status update — you have 24 active initiatives

STOP: Ask me: "Get the Product Catalogue feature log, extract all active initiatives with their status, and create a synthesised summary grouping by: at-risk, on-track, and blocked"

USER: Types command

ACTION:
1. Fetch the feature log page
2. Parse all active initiatives
3. Group by status
4. Create a summary in executive format (suitable for pasting into a Slack update or email)

---

**Workflow 3: Cross-Space Research — Compliance**

Scenario: GPSR requirements may exist across multiple spaces

STOP: Ask me: "Search across the pp space, Engineering space, and Legal/Compliance space for all GPSR-related documentation and create a consolidated requirements doc"

USER: Types command

ACTION:
1. Search pp space for GPSR
2. Search Engineering space for GPSR
3. Search any Legal/Compliance space
4. Synthesise findings into consolidated document
5. Offer to save as new pp space page

---

**Workflow 4: PRD Review Cycle**

Scenario: Your GPSR PRD needs engineering review

STOP: Ask me: "Add a comment to the GPSR PRD page tagging the engineering team: 'Ready for technical feasibility review — specifically need input on: (1) PIM provides manufacturerContact field? (2) Publisher mapping changes required? (3) FullProduct endpoint schema change timeline'"

USER: Types command

ACTION: Add a structured comment to the PRD page — the kind a platform PM leaves to drive technical alignment.

---

## Phase 7: Integration with Other Modules

Confluence MCP amplifies your entire Product Catalogue workflow:

**Module 4.3 (Azure DevOps):**
```
Search pp space for GPSR requirements
  ↓
Extract: field names, validation rules, compliance constraints
  ↓
Create Azure DevOps story with that content as AC
  ↓
Link story back to the Confluence PRD
```

**Module 4.6 (Context Layers):**
```
Search pp space for team architecture docs
  ↓
Bootstrap .context/domains/product-catalogue/business-rules.md
  ↓
Copilot now knows your domain permanently
  ↓
Every future session starts with full PC context
```

STOP: Which workflow would save you the most time this sprint?

USER: Responds

---

## Phase 8: Advanced Techniques

**Technique 1: Bulk Research — All GPSR Docs**

STOP: Try: "Search pp and Engineering spaces for all pages mentioning GPSR or 'General Product Safety' and create a comparison table showing: page title, space, last updated, key requirements mentioned"

USER: Types command

ACTION: Multi-space search, extract key data, create comparison table.

---

**Technique 2: Template-Based Page Creation**

STOP: Try: "Find the Product Catalogue PRD template page in the pp space and create a new page using that structure for a 'Retired Products - Discovery Phase' feature"

USER: Types command

ACTION: Fetch template structure, apply to new page with PC-specific content.

---

**Technique 3: Architecture Doc Synthesis**

STOP: Try: "Search pp space for the Product Catalogue architecture target state documents and create a summary of: current state, target state, key migrations in progress"

USER: Types command

ACTION: Fetch architecture docs, extract current vs. target state, synthesise gap analysis for a platform PM's needs.

---

## Common Issues & Solutions

**Issue 1: "pp space not found"**
- Verify space key is exactly `pp` (lowercase)
- Check you have view permissions for the pp space
- Try searching without space filter first to confirm Confluence connection

**Issue 2: "Page not found"**
- Use page IDs rather than titles when possible (more reliable)
- Check exact page title (case-sensitive)
- Try searching for the page first to get the correct ID

**Issue 3: "Formatting issues"**
- Markdown → Confluence conversion has limitations
- Tables may need manual adjustment in Confluence editor
- Use Confluence editor for final polish of published PRDs

STOP: Encountered any issues? Let's troubleshoot.

USER: Reports issues or confirms working

---

## Practice Challenge

Put everything together in one complete workflow:

**Challenge: Complete Feature Documentation Cycle**

1. Search pp space for "Speed-to-Market" or "Celonis" pages
2. Extract the key metrics and data sources mentioned
3. Create a new page: "Speed-to-Market Dashboard - PM Briefing"
4. Add 3 key findings from the research
5. Add a "Next Steps" section: "Confirm Celonis data freshness SLA with Data Engineering"
6. Share the page URL

STOP: Complete this challenge now. Take 5 minutes.

USER: Works through the challenge

ACTION: Guide through each step, provide feedback, celebrate completion.

---

## Time Savings Analysis

Let's calculate your personal ROI:

**Traditional pp Confluence workflow:**
- Search + scan docs: 12 min
- Create new page (format, structure): 8 min
- Update feature log: 6 min per update
- API contract documentation: 20 min
- Cross-space research: 25 min
- **Total: 50-70 minutes per full workflow cycle**

**With Confluence MCP:**
- All operations combined: 5-8 min
- **Time saved: 45-60 minutes per cycle**

STOP: How many times per week do you create or update Confluence pages?

USER: Responds with number

ACTION: Calculate their specific savings:
- Weekly: [number] × 50 minutes saved
- Monthly: [weekly] × 4 weeks

Example: 3 page cycles/week = 150 min/week = 2.5 hours/week = 10 hours/month

---

## Best Practices for Confluence MCP

**DO:**
✓ Always scope searches to pp space (`space:pp`) for PC team docs
✓ Use page IDs over page titles — more reliable
✓ Work locally in VSCode, review before publishing to shared spaces
✓ Use version history when making significant changes to API contracts
✓ Link Confluence pages to Azure DevOps work items for traceability

**DON'T:**
✗ Publish incomplete API contracts — consuming teams (PDP, Search) act on these
✗ Update the Feature Log without checking current version first
✗ Forget to notify stakeholders (PDP team, Search team) of API contract changes
✗ Rely solely on MCP for complex OpenAPI spec formatting
✗ Skip manual review of compliance-related content (GPSR, hazmat)

STOP: Any questions about best practices?

USER: Asks questions or confirms ready

---

## Next Steps

You've mastered Confluence MCP for the pp space! Next: **Azure DevOps Integration**

**Before Module 4.3:**
1. Practice creating 2-3 pages in the pp space
2. Set up your standard PRD template page
3. Identify the most repetitive pp space task to automate

**In Module 4.3, you'll learn:**
- Query the Product Catalogue board without browser
- Create [FeatureTag] stories in PC format
- Build cascading story epics (Publisher → API → Feeds)
- Analyse sprint velocity for the PC team
- Write security/tech-debt stories in the correct pattern

STOP: Ready to connect your Azure DevOps board?

USER: Confirms

---

## Reflection

You can now:
✓ Search the pp space from VSCode in seconds
✓ Read feature logs and API contracts without browser
✓ Create PRDs and API contract pages with correct structure
✓ Update the feature log iteratively
✓ Run cross-space research for compliance requirements
✓ Integrate Confluence into your Azure DevOps story workflow

**Your new superpower:** Platform documentation at the speed of thought — no more losing 20 minutes searching for that API contract page

**Time saved:** 5-8 hours per week on documentation

See you in Module 4.3!
