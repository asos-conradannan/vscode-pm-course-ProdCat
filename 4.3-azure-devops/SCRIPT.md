# Module 4.3: Azure DevOps Integration
## Product Catalogue Edition

**Teaching Script for GitHub Copilot**
**Duration:** ~30 minutes

---

## Teaching Flow

Welcome to Module 4.3: Azure DevOps Integration!

Time to connect Copilot to the E-Commerce Foundations project and automate the Product Catalogue team's story-writing busywork

By the end of this lesson, you'll be able to:
- Query the Product Catalogue board without opening a browser
- Write developer-first stories in the [FeatureTag] format
- Create cascading story epics across the Publisher → API → Feeds layers
- Analyse sprint metrics for the Product Catalogue team
- Generate team status reports instantly

STOP: How much time do you spend writing or updating Azure DevOps stories each day?

USER: Responds

---

## Why Azure DevOps + Copilot for Product Catalogue?

Let me show you the transformation for your specific work:

**Current workflow (without MCP):**
1. Open browser → Navigate to E-Commerce Foundations
2. Find the Product Catalogue board → Locate the right epic
3. Create a story → Fill in title, description, area path manually
4. Write developer-first AC — field name, type, location, endpoints, test fixtures
5. Create 3 more stories for the same change (Publisher, API, Feeds)
6. Link them all manually as children of the epic
7. **Time: 20-30 minutes for a 4-story cascade**

**New workflow (with MCP):**
1. "Create 3 linked stories for the FlatLays supplementary assets feature — Models class, Publisher mapping, and FullProduct endpoint"
2. Copilot creates all 3 with technical AC, links them to the epic
3. **Time: 3 minutes**

**Time saved:** 17-27 minutes per feature cascade = **2-3 hours per sprint**

STOP: What Azure DevOps task do you find most repetitive for the Product Catalogue team?

USER: Responds

---

## Phase 1: Explore Azure DevOps MCP

Let's see what tools are available

STOP: Ask me: "What Azure DevOps tools are available through MCP?"

USER: Types command

ACTION: List the available Azure DevOps MCP tools:

**Work Item Management:**
- Create, update, get, delete work items
- Query work items by filter, state, tag, or sprint
- Add comments, link work items (parent/child, related)
- Get work item history

**Backlog & Iteration:**
- List backlogs (Initiatives, Epics, Features, Stories, Tasks)
- Get work items for a specific iteration
- Get team capacity

**Repository & PR:**
- Get repositories, branches, commits
- List and comment on pull requests

**Build & Pipeline:**
- Get build status
- Query pipeline runs

Explain these connect to ASOS's Azure DevOps using your existing E-Commerce Foundations permissions.

---

Great! Two tools you'll use constantly:
- `wit_my_work_items` — Your assigned items in E-Commerce Foundations
- `wit_list_backlog_work_items` — The full Product Catalogue backlog

**Authentication:** The MCP uses your Azure DevOps session and permissions

STOP: Are you familiar with the E-Commerce Foundations project and Product Catalogue team on the Azure DevOps board?

USER: Responds

ACTION: Note the project name and team for use throughout. If they're unsure: project = "E-Commerce Foundations", team = "Product Catalogue"

---

## Phase 2: Query the Product Catalogue Board

Let's start with finding your work

**Exercise 1: Get Your Work Items**

STOP: Ask me: "Show me all work items assigned to me in the E-Commerce Foundations project"

USER: Types command

ACTION: Use `wit_my_work_items` with project `E-Commerce Foundations`. Display:
- Work item ID, title, type (User Story, Task, Bug), state, sprint

---

Excellent! You just pulled your board without opening Azure DevOps

**Exercise 2: Filter by Current Sprint**

STOP: Ask me: "Show me all work items for the Product Catalogue team in the current sprint in E-Commerce Foundations"

USER: Types command

ACTION: Query with iteration filter. Group results by state (Active/Ready/New) and show sprint progress overview.

---

**Exercise 3: Search by Feature Tag**

STOP: Ask me: "Find all user stories tagged or titled with 'FlatLays' in E-Commerce Foundations"

USER: Types command

ACTION: Search for FlatLays work items. Show the full cascade if visible (Models → Publisher → API → Feeds stories).

This is how you orient yourself on an existing feature mid-sprint.

---

## Phase 3: Get Work Item Details

Searching gives you the list — but let's go deeper on individual stories

**Exercise 4: Get Full Story Details**

Note a work item ID from your previous results. Let's use a real Product Catalogue example:

STOP: Ask me: "Get all details for work item 1172555" (the StockPrice endpoint story — a production example of a well-written PC story)

USER: Types command

ACTION: Fetch using `wit_get_work_item`. Display:
- Full description (developer-first format)
- Technical specification (field, type, location, endpoints affected)
- Decision logic and constraints
- Comments and history
- Links to related items (parent epic, Confluence PRD)

---

**Exercise 5: Analyse Story Quality**

STOP: Ask me: "Analyse this work item — does it have: a [FeatureTag] in the title, developer-first description, technical field specification, list of affected endpoints, and test fixture updates in the AC?"

USER: Types command

ACTION: Analyse the story against the Product Catalogue story quality checklist:
- ✓ [FeatureTag] in brackets in title
- ✓ "As a developer I want to..." format
- ✓ Specific field name, type, location
- ✓ Affected endpoints listed
- ✓ Test fixtures mentioned in AC
- ✓ GitHub repo or Confluence link attached

Show what's present and flag any gaps.

---

## Phase 4: Create Work Items — PC Format

Now the powerful part — creating stories in the Product Catalogue way

**Exercise 6: Create a GPSR Story — Basic**

STOP: Ask me: "Create a new user story in E-Commerce Foundations for the Product Catalogue team titled '[GPSR] - Product Catalogue API - Add manufacturer contact fields to FullProduct endpoint'"

USER: Types command

ACTION: Use `wit_create_work_item` to create:
- Type: User Story
- Title with [GPSR] feature tag
- Area path: E-Commerce Foundations/Product Catalogue
- Return work item ID and URL

---

**Exercise 7: Create with Full Technical Detail**

Let's get the AC right — in the Product Catalogue developer-first style

STOP: Ask me: "Update that story with a full developer-first description:
As a developer I want to add manufacturer contact fields to the FullProduct endpoint response so that the PDP team can display GPSR-compliant manufacturer information on the product page.

Technical Specification:
- Fields: manufacturerName (string), manufacturerAddress (string), manufacturerContact (string, optional)
- Location: product.compliance object in response
- Affected endpoints: /fullproduct/{productId}, /variants/{productId}
- Default: null (fields optional until PIM provides data)
- Compliance rule: If selling in EU/UK, fields required for GPSR

Acceptance Criteria:
- Fields added to ProductComplianceModel in product-packages-models repo
- Publisher maps fields from PIM ComplianceInfo payload
- FullProduct endpoint returns compliance object
- Test fixtures updated with manufacturerName, manufacturerAddress
- Contract test passes for PDP consumer"

USER: Types command

ACTION: Update the work item with the full description and AC. Show the formatted result. Note how this maps to the real work item pattern seen on the board (like ID 1172555).

---

## Phase 5: The Cascade — Multi-Story Epic Creation (PC-Specific)

This is the workflow unique to backend platform PMs. One data model change = 3-4 stories across layers.

**Exercise 8: Create a Full Feature Cascade**

STOP: Ask me: "Create a set of 3 linked user stories in E-Commerce Foundations for the Product Catalogue team, for adding supplementaryAssets to the FlatLays feature:

Story 1: [FlatLays] - Product Packages Models - Add SupplementaryAssets class with HasFlatlay boolean flag
Story 2: [FlatLays] - Product Publisher - Map supplementaryAssets from PIM FlatLayAssets payload
Story 3: [FlatLays] - Product Catalogue FullProduct EndPoint - Expose supplementaryAssets in API response

Link stories 2 and 3 as children/related to story 1, and link all to the FlatLays epic"

USER: Types command

ACTION:
1. Create all 3 work items with appropriate descriptions
2. Link them appropriately (parent/child or related)
3. Show the cascade structure
4. Return all 3 IDs and URLs

This mirrors real work items 1174632 (Models), 1163490 (FullProduct API), etc. from the actual board.

---

## Phase 6: Update Work Items

Work items evolve through refinement. Let's keep them current.

**Exercise 9: Move a Story to Ready for Refinement**

STOP: Ask me: "Update work item [ID from Exercise 6] status to 'Ready for Refinement' and add the tag 'gpsr'"

USER: Types command

ACTION: Update state and tags. Show new state and confirm tag applied.

---

**Exercise 10: Add an Engineering Comment**

STOP: Ask me: "Add a comment to the GPSR story: 'PIM team confirmed manufacturer fields are available in ComplianceInfo payload from Sprint 41. Publisher mapping story unblocked. Assigning to Shreya for refinement.'"

USER: Types command

ACTION: Add comment with timestamp. This creates the audit trail that makes the work item useful.

---

**Exercise 11: Create a Security / Tech Debt Story**

The other common PC story type — security vulnerability remediation

STOP: Ask me: "Create a security story in E-Commerce Foundations: '[SEC] - Product Catalogue API - Remove WIZ security vulnerabilities from acceptance tests (PPAV2 dependencies)' — tag with 'security' and 'tech-debt', set value area to Business"

USER: Types command

ACTION: Create security story with:
- [SEC] feature tag
- Tags: security, tech-debt
- Value Area: Business (all PC stories are Business value area)
- State: New (not yet refined)

---

## Phase 7: Sprint Analytics

Let's unlock insights from the Product Catalogue board data

**Exercise 12: Product Catalogue Velocity**

STOP: Ask me: "Calculate the velocity for the Product Catalogue team over the last 3 sprints in E-Commerce Foundations"

USER: Types command

ACTION:
1. Get last 3 completed iterations for Product Catalogue team
2. Query completed work items in each sprint
3. Sum story points where available
4. Show velocity trend (increasing / decreasing / stable)
5. Flag: Product Catalogue work items tend to be 3-5 story points — API changes are complex

---

**Exercise 13: Burndown Check**

STOP: Ask me: "Show the current sprint burndown status for the Product Catalogue team — how many stories are Active vs Done, and are we on track?"

USER: Types command

ACTION:
1. Get current iteration for Product Catalogue team
2. Query Active and Done items
3. Compare to sprint midpoint expected progress
4. Flag any items stuck in Active for 3+ days without update

---

**Exercise 14: Unplanned Work Analysis**

STOP: Ask me: "Find all Product Catalogue work items tagged 'unplanned' this sprint and calculate what percentage of capacity is reactive work"

USER: Types command

ACTION: This is a real platform PM metric — show what % of sprint is incident-driven vs. planned feature work.

---

## Phase 8: Real Product Catalogue Workflows

Combine everything into daily workflows

**Workflow 1: Daily Standup Prep (2 minutes)**

STOP: Ask me: "Prepare my standup update: Show Product Catalogue work items updated yesterday, their current state, and any items stuck in Active without recent activity"

USER: Types command

ACTION:
1. Query work items in E-Commerce Foundations / Product Catalogue updated yesterday
2. Flag any Active items with no update in 24h (potential blocker)
3. Generate standup format: what was completed, what's in progress, any blockers

---

**Workflow 2: Sprint Planning — PC Team**

STOP: Ask me: "Prepare sprint planning for Product Catalogue: show last 3 sprint velocity, top 10 backlog items by priority, and current team capacity"

USER: Types command

ACTION:
1. Calculate velocity trend
2. Query top backlog items (ordered by business value/priority)
3. Get team capacity for next sprint
4. Generate planning summary: how many points realistically achievable, which items to pull in first

---

**Workflow 3: Feature Epic Health Check**

Scenario: You're the PM for the GPSR epic — need to know the state of all child stories

STOP: Ask me: "Show all work items linked to the GPSR epic and create a status summary: how many are Active, Ready for Refinement, New (unrefined), and Done"

USER: Types command

ACTION:
1. Query work items by parent epic link
2. Group by state
3. Flag any child stories still in New (unrefined) with sprint approaching

---

## Phase 9: Advanced Automation

**Technique 1: Bulk Create from PRD**

STOP: Ask me: "Read the GPSR PRD page from Confluence pp space and create user stories for each P0 requirement section"

USER: Types command

ACTION:
1. Search Confluence pp space for the GPSR PRD
2. Extract P0 requirements
3. Create work items for each — in [GPSR] PC format
4. Link them all to the GPSR epic
5. Return list of created IDs

---

**Technique 2: Tech Debt Triage**

STOP: Ask me: "Find all Product Catalogue stories in New state with 'security' or 'tech-debt' tags and suggest priority order based on: title keywords (WIZ, vulnerability, upgrade), and how long they've been in New state"

USER: Types command

ACTION: Surface unrefined tech debt and suggest refinement order for the PM.

---

## Common Issues & Solutions

**Issue 1: "Project not found"**
- Use exact project name: "E-Commerce Foundations" (case-sensitive, with spaces)
- Try `wit_my_work_items` first to confirm connection

**Issue 2: "Team not found"**
- Team name: "Product Catalogue" (as it appears on the board)
- Try listing all teams in the project first

**Issue 3: "Work item type not available"**
- Confirmed types in E-Commerce Foundations: User Story, Task, Bug, Epic, Feature, Spike, Retro Bucket
- Use "User Story" (not "Story")

STOP: Hit any issues? Let's troubleshoot.

USER: Reports issues or confirms working

---

## Practice Challenge

Complete workflow challenge:

**Challenge: Full Story Lifecycle for a New Requirement**

1. Search the Product Catalogue board for any existing GPSR-related stories
2. If none exist matching your target, create a new story: "[GPSR] - Product Catalogue API - Expose isGpsrCompliant boolean on FullProduct endpoint"
3. Add developer-first description with technical AC (field name, type, location, endpoints, test fixtures)
4. Add the tag "gpsr"
5. Link it to the GPSR epic (if visible)
6. Add a comment: "Created from GPSR PRD requirements. Pending PIM field confirmation."
7. Set state to "Ready for Refinement"
8. Calculate: how long did that take vs. manual?

STOP: Complete this 8-step workflow now. Time yourself!

USER: Works through challenge

ACTION: Guide through each step, note their time, celebrate completion.

---

## Time Savings Calculation

**Traditional Product Catalogue story workflow:**
- Navigate to board and find the right epic: 5 min
- Create 1 story with proper [FeatureTag] title: 4 min
- Write developer-first description + technical AC: 10 min
- Create 3 linked cascade stories: 30 min total
- Tag, link to epic, update state: 5 min
- **Total for a 4-story cascade: 54 min**

**With Azure DevOps MCP:**
- Full cascade creation: 5-8 min
- **Time saved: 46+ minutes per cascade**

STOP: How many cascades do you create per sprint?

USER: Responds with number

ACTION: Calculate specific savings per sprint.

---

## Best Practices for PC Team Stories

**DO:**
✓ Always use [FeatureTag] in brackets at the start of titles
✓ Use "As a developer I want to..." format (not "As a user...")
✓ Specify field name, type, and location in the response schema
✓ List ALL affected endpoints explicitly
✓ Include test fixtures in AC — engineers expect this
✓ Link Confluence PRD to the parent epic

**DON'T:**
✗ Create stories without checking the epic exists first
✗ Write vague AC ("API should return manufacturer data") — be specific
✗ Forget to link cascade stories to the parent epic
✗ Create work items for features that already exist (search first)
✗ Change state without notifying the assigned engineer

STOP: Any questions about best practices?

USER: Asks questions or confirms

---

## Next Steps

You've mastered Azure DevOps MCP for the Product Catalogue team! Next: **Figma Collaboration**

**Before Module 4.4:**
1. Practice creating 3 stories in [FeatureTag] PC format
2. Try creating a 2-story cascade (Model + API)
3. Run the sprint velocity query for your team

**In Module 4.4, you'll learn:**
- Browse internal tool designs (Product Preview, BackOffice, Speed-to-Market)
- Extract specs from Product Catalogue admin interfaces
- Reference internal designs in PRDs

STOP: Ready to connect your design tools?

USER: Confirms

---

## Reflection

You can now:
✓ Query the E-Commerce Foundations / Product Catalogue board without browser
✓ Create [FeatureTag] stories in developer-first format
✓ Write technical AC with field specs, endpoint lists, and test fixture requirements
✓ Build multi-story cascades (Publisher → API → Feeds)
✓ Create security/tech-debt stories in the correct PC pattern
✓ Analyse sprint velocity and burndown for the PC team
✓ Automate your daily standup prep

**Your new superpower:** Product Catalogue story-writing at machine speed — consistent, technical, properly formatted

**Time saved:** 2-4 hours per week on story creation and sprint management

See you in Module 4.4!
