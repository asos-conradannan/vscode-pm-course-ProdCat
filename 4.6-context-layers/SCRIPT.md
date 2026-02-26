# Module 4.6: Context Layer Setup
## Product Catalogue Edition — The Game Changer

**Teaching Script for GitHub Copilot**
**Duration:** ~35 minutes
**Do this module last** — it synthesises everything from Modules 4.2-4.5

---

## Teaching Flow

Welcome to Module 4.6 — the module that makes everything else permanent.

Every time you start a new Copilot session and ask it to help you write a GPSR story, it doesn't know what GPSR is. You explain. It helps. Next session — same thing. You explain again.

**Context layers fix this permanently.**

By the end of this lesson, you'll have built a `.context/` folder that loads automatically with every session. Copilot will know:
- What PIM, PLP, ATS, GPSR, TGR mean without you explaining
- Who your internal API consumers are and what they care about
- What your publication pipeline looks like
- What compliance constraints apply to your domain
- The specifics of your current initiatives (GPSR, FlatLays, IndexNow)

And we'll use Confluence MCP — the skill you built in Module 4.2 — to bootstrap most of this automatically from your existing pp space docs.

STOP: What's the context you have to re-explain most often at the start of a Copilot session?

USER: Responds (likely: what team they're on, what PIM is, who the stakeholders are, what they're working on)

---

## Why Context Layers?

Think of it like onboarding a new member of your team. The first week, you explain everything: how the pipeline works, who owns what, what GPSR is, who the PDP team lead is.

By week two, they just know.

Context layers are the equivalent for Copilot. You build the documentation once. It loads every session. Copilot arrives at week two on day one.

**The architecture (4 layers):**

```
Layer 1 — Global Context (.context/global/)
  What: Your role, ASOS context, universal terminology
  Persists: Always loaded — every session, every project

Layer 2 — Domain Context (.context/domains/product-catalogue/)
  What: Product Catalogue business rules, publication pipeline, compliance
  Persists: Loaded whenever working on PC domain work

Layer 3 — Project Context (.context/projects/[initiative]/)
  What: Specific initiative details (GPSR, FlatLays, IndexNow)
  Persists: Loaded when working on that initiative

Layer 4 — File Context (YAML frontmatter in files)
  What: What this specific file is, why it exists, how it connects
  Persists: Embedded in the file itself
```

Context flows downward: Global → Domain → Project → File. Lower layers inherit and extend higher layers.

STOP: Does this architecture make sense? Questions before we start building?

USER: Responds

---

## The Before/After Demo

Before we build anything, let's see the problem clearly.

**Exercise: Copilot without context**

STOP: Ask me: "What is GPSR and how does it affect the Product Catalogue API?"

USER: Types command

ACTION: Copilot gives a generic answer — explains GPSR as a regulation in general terms, with no specifics about:
- Which endpoints are affected
- What fields need to be added
- Who the consuming teams are (PDP, BackOffice)
- What the compliance timeline is

Tell the user: "This is what you get today — useful, but generic. By the end of this module, you'll get a specific, domain-aware answer without typing a single word of context."

---

## Phase 2: Build Global Context

Global context loads in every session. Keep it lean — under 2,000 tokens.

**Exercise 1: Create the Terminology File**

STOP: Ask me: "Create a file at `.context/global/terminology.md` for the Product Catalogue team at ASOS. Include definitions for these terms: PIM, PLP, SKU, colourway, ATS, on-query, fat events, lean events, GPSR, hazmat restrictions, brand restrictions, Store Product, TGR, TTL/jitter, Speed-to-Market, Redis cache, Fredhopper, Algolia, IndexNow, BazaarVoice, Smartling"

USER: Types command

ACTION: Create the terminology file using the template structure. Each term should have:
- Definition (2-3 sentences max)
- Context: where this term is used
- Related terms

---

**Exercise 2: Create the Personas File**

For a backend platform PM, "personas" are your internal API consumers — not customer personas.

STOP: Ask me: "Create `.context/global/personas.md` listing the internal consumers of the Product Catalogue API. Include: PDP team (web + app), Search (Fredhopper and Algolia), Recommendations, BackOffice, Marketing email feeds, third-party brand feeds. For each: what they consume from the API, what their SLA requirements are, and what they care about most."

USER: Types command

ACTION: Create the personas file. Frame each "persona" as:
- **Consumer name:** PDP team (web + app)
- **What they consume:** FullProduct endpoint — images, pricing, stock, size info, reviews
- **SLA:** Sub-100ms p99 response time
- **Care most about:** Data freshness, image availability, accurate stock status

---

## Phase 3: Build Domain Context — Product Catalogue

Domain context loads when you're working on Product Catalogue work. This is the most important layer for you.

**The Power Move: Bootstrap from Confluence**

We're going to use the Confluence MCP you mastered in Module 4.2 to auto-populate these files from your existing pp space documentation.

STOP: Ask me: "Search the pp Confluence space for the Product Catalogue team overview page and any architecture documentation pages, then extract the key information about: what the team owns, how the publication pipeline works, and what the main compliance constraints are"

USER: Types command

ACTION:
1. Use `confluence_search_in_space` with spaceKey `pp` for "product catalogue team overview" and "architecture"
2. Read the relevant pages
3. Extract: team mission, pipeline flow (PIM → Publisher → Catalogue → API), compliance constraints
4. Present as structured text ready to save into context files

---

**Exercise 3: Create Domain README**

STOP: Ask me: "Using the Confluence content you just extracted, create `.context/domains/product-catalogue/README.md` with: team mission, what the team owns (APIs, pipeline, compliance), key internal consumers, and how the domain connects to the broader ASOS platform"

USER: Types command

ACTION: Create the domain README. It should answer:
- What does this domain exist to do?
- What are its boundaries?
- Who are its main consumers?
- Where does it fit in the ASOS data architecture?

---

**Exercise 4: Create Business Rules**

STOP: Ask me: "Create `.context/domains/product-catalogue/business-rules.md` covering:
1. Publication pipeline rules — what must happen in order (PIM → language products → store products → live)
2. Cache rules — TTL strategy, jitter, Redis invalidation triggers
3. Compliance constraints — GPSR scope, hazmat restrictions, brand restrictions
4. Event rules — fat vs lean events, idempotency, ordering guarantees
5. Stock rules — ATS, on-query, real-time update triggers"

USER: Types command

ACTION: Create business rules file. This is the most valuable file — Copilot will use it every time it makes a recommendation about API design, event architecture, or compliance.

---

**Exercise 5: Create Domain Workflows**

STOP: Ask me: "Create `.context/domains/product-catalogue/workflows.md` documenting:
1. Product publication workflow (trigger → PIM event → Publisher → Catalogue → live)
2. Incident response workflow (detection → INC creation → mitigation → post-mortem)
3. Sprint planning cadence (refinement timing, story types, cascade creation)
4. Compliance validation workflow (for GPSR and hazmat)"

USER: Types command

ACTION: Create workflows file. Frame each workflow as: trigger → steps → output → responsible party.

---

**Exercise 6: Create Domain Terminology**

Domain-specific terms that extend the global terminology — Product Catalogue jargon that other ASOS teams wouldn't know.

STOP: Ask me: "Create `.context/domains/product-catalogue/terminology.md` with Product Catalogue-specific terms: FullProduct endpoint, PLP Manager, supplementaryAssets, HasFlatlay, colourway variant, PPAV2, DMCCA, WIZ scan, product-packages-models repo, BackOffice product locate, StockPrice endpoint, Feeds API"

USER: Types command

ACTION: Create domain terminology. These are the terms that appear in stories and ADO work items that even other ASOS PMs wouldn't know.

---

## Phase 4: Build Project Context — Current Initiative

Project context is per-initiative. You build one when a new feature starts.

**Exercise 7: Create GPSR Project Context**

STOP: Ask me: "Create a project context folder `.context/projects/gpsr-compliance/` with these files:

README.md — project overview:
- One-liner: Ensure all EU/UK products include GPSR-compliant manufacturer data
- Status: In Discovery
- Target sprint: Sprint 44
- Stakeholders: Legal/Compliance, PDP team, BackOffice, Data Engineering (PIM)

product-context.md — the product problem:
- Problem: EU/UK General Product Safety Regulation requires manufacturer name, address, and contact on all products. Currently ~40% coverage.
- Target: 100% coverage for EU/UK products
- Success metric: GPSR compliance dashboard shows 100% before enforcement date
- Non-goal: Not building the BackOffice admin in this phase (P2)

technical-context.md — what engineering needs to know:
- New fields on FullProduct endpoint: manufacturerName, manufacturerAddress, manufacturerContact
- Data source: PIM ComplianceInfo payload
- Publisher mapping required
- Cache invalidation: trigger on manufacturer data change
- BackOffice: read-only view in Phase 1, edit in Phase 2"

USER: Types command

ACTION: Create all 3 files in the project context folder. Show the structure.

---

## Phase 5: The Magic Demo

Now let's prove it works.

**Exercise 8: Test the Context**

STOP: Ask me: "What is GPSR and how does it affect the Product Catalogue API?"

USER: Types command

ACTION: Copilot now answers with full domain-specific context:
- References the FullProduct endpoint by name
- Mentions manufacturerName, manufacturerAddress, manufacturerContact fields
- Notes the EU/UK scope (not global)
- References PIM as the data source
- Mentions the BackOffice admin requirement

Compare to the answer from Phase 1 — same question, completely different quality.

---

**Exercise 9: Prove Context Inheritance**

STOP: Ask me: "Draft an acceptance criteria checklist for a story to add GPSR manufacturer fields to the Product Catalogue API. I haven't told you anything about what we're building."

USER: Types command

ACTION: Copilot generates:
- Field names in the Product Catalogue format (manufacturerName, etc.)
- Mentions product-packages-models repo
- References Publisher mapping from PIM
- Includes test fixture requirement (because it knows the PC story pattern)
- Flags cache invalidation as a concern (because it knows the business rules)

This is the moment. Without typing a word of context.

---

## Phase 6: File Context (Quick Win)

File context is optional but powerful. It tells Copilot what a specific file is and how it connects to the rest of your context.

STOP: Ask me: "Add YAML frontmatter context to my GPSR PRD Confluence draft (or the local PRD file I'm working on) — link it to the gpsr-compliance project context and the product-catalogue domain"

USER: Types command

ACTION: Add context frontmatter:
```yaml
---
context:
  domain: product-catalogue
  project: gpsr-compliance
  type: prd
  purpose: Requirements for GPSR manufacturer data fields in Product Catalogue API
  business_value: EU/UK regulatory compliance — GPSR enforcement
  related:
    - .context/domains/product-catalogue/business-rules.md
    - .context/projects/gpsr-compliance/product-context.md
    - .context/projects/gpsr-compliance/technical-context.md
  stakeholders:
    - Legal/Compliance (sign-off)
    - PDP team (consumer)
    - BackOffice team (consumer + admin)
  status: draft
---
```

---

## Phase 7: The Bootstrap Power Move

This is the fastest way to build context for an existing team — use Confluence to do it.

STOP: Ask me: "Search the pp Confluence space for pages about: GPSR, FlatLays, IndexNow, Speed-to-Market, and TGR. For each topic, extract: what it is, what work has been done, who owns it, and what the target state is. Then update my .context files with this information."

USER: Types command

ACTION:
1. Use `confluence_search_in_space` for each topic
2. Read the relevant pages
3. Extract structured information
4. Update:
   - `.context/domains/product-catalogue/business-rules.md` — add any new rules discovered
   - `.context/projects/` — create or update project context files for each initiative
   - `.context/global/terminology.md` — add any new terms discovered

This is the "zero to context" workflow. In 5 minutes, you've populated context from months of accumulated team documentation.

STOP: How different would your next Copilot session feel after running this?

USER: Responds

---

## Phase 8: Maintaining Your Context

Context files are living documents — they need maintenance.

**When to update:**
- New initiative starts → Create project context folder
- Team terminology changes → Update terminology.md
- Architecture decision made → Add to business-rules.md
- Sprint completes → Archive project context, create next one
- Stakeholder changes → Update personas.md

**How long does maintenance take?**
- New initiative: 5 minutes (or use Bootstrap Power Move)
- Term added: 30 seconds
- Business rule updated: 1 minute

**When does it pay off?**
- From your very first session after setup
- Compounds as context files mature

STOP: How often do you start new initiatives? That's how often you'll add a new project context folder.

USER: Responds

---

## Integration with Modules 4.2-4.5

Your context files amplify every other module:

**Module 4.2 (Confluence):**
```
Copilot knows pp space is your space
Copilot knows which pages are relevant before you ask
Searches return domain-relevant results first
```

**Module 4.3 (Azure DevOps):**
```
Copilot knows E-Commerce Foundations / Product Catalogue board
Copilot knows [FeatureTag] format without being told
Copilot generates developer-first AC because it knows the pattern
```

**Module 4.5 (Miro):**
```
Copilot knows your retro recurring themes before you ask
Architecture synthesis uses correct domain terminology
Action routing uses your team's actual workflow
```

STOP: What's the most impactful context file you built today?

USER: Responds

---

## Common Issues & Solutions

**Issue 1: "Context not loading"**
- Ensure `.context/` folder is in the root of your VSCode workspace
- Reference context files explicitly: "@workspace check .context/domains/product-catalogue/business-rules.md"
- Some setups require explicit context file referencing in first message

**Issue 2: "Context too large — response slowing down"**
- Keep each context file under 1,000 tokens
- Use links between files rather than repeating content
- Archive stale project contexts rather than growing the folder

**Issue 3: "Confluence bootstrap didn't find the right pages"**
- Narrow your search: add specific keywords ("GPSR manufacturer", "FlatLays supplementary")
- Get the exact page ID from pp space and use `confluence_get_page_by_id`
- Try reading the Feature Log page (ID: 4506256613 children) for all active initiatives

STOP: Any issues? Let's troubleshoot.

USER: Reports issues or confirms working

---

## Practice Challenge

**Challenge: Complete Context Setup Sprint**

1. Create `.context/global/terminology.md` with at least 10 Product Catalogue terms
2. Create `.context/global/personas.md` with at least 3 internal API consumers
3. Use Confluence MCP Bootstrap: search pp space for 2 initiative pages and extract them into `.context/projects/`
4. Create `.context/domains/product-catalogue/business-rules.md` with at least 3 publication pipeline rules
5. Test: ask Copilot "What are the key business rules for publishing a product to the Product Catalogue?" without providing any context in the prompt

STOP: Complete this challenge now. Take 10 minutes.

USER: Works through challenge

ACTION: Guide through each step. The final test question is the proof point — celebrate when Copilot answers correctly.

---

## Time Savings Analysis

**Without context layers:**
- Per-session context re-explanation: 10-15 minutes
- Generic answers requiring correction and clarification: 5-10 minutes per session
- **Daily overhead: 15-25 minutes on context**

**With context layers:**
- Session startup: 0 minutes (context loads automatically)
- Answers are domain-specific from first prompt
- **Daily overhead: 0 minutes**

**Weekly savings: 75-125 minutes** (at 5 sessions/day)
**This compounds** — as context files mature, answer quality improves without additional effort

STOP: How many Copilot sessions do you run per day? Let's calculate your specific savings.

USER: Responds with number

ACTION: Calculate weekly savings. Even at 3 sessions/day × 15 min = 45 min/day = 3.75 hours/week saved.

---

## Best Practices

**DO:**
✓ Keep global context lean (terminology + personas only — not project-specific info)
✓ Create a new project context folder for every new initiative at discovery stage
✓ Use Bootstrap Power Move at start of each new initiative to populate from Confluence
✓ Archive completed project context folders — don't delete (institutional memory)
✓ Link context files to each other rather than repeating content

**DON'T:**
✗ Put everything in one giant context file — keep layers separated
✗ Include credentials, API keys, or personal data in context files
✗ Over-engineer context for small tasks — save it for recurring work
✗ Forget to update context when the architecture or team changes
✗ Keep stale project context (completed initiatives) loading — archive them

STOP: Any questions about best practices?

USER: Asks questions or confirms

---

## Reflection

You have now built:
✓ `.context/global/terminology.md` — Product Catalogue language, permanently loaded
✓ `.context/global/personas.md` — Internal API consumers, permanently loaded
✓ `.context/domains/product-catalogue/README.md` — Domain mission and scope
✓ `.context/domains/product-catalogue/business-rules.md` — Publication pipeline, cache, compliance
✓ `.context/domains/product-catalogue/workflows.md` — How work flows in your team
✓ `.context/domains/product-catalogue/terminology.md` — Domain-specific jargon
✓ `.context/projects/gpsr-compliance/` — Initiative-specific context
✓ File context frontmatter pattern

**Your new reality:** Copilot arrives at "week two" on day one of every session

**Time saved:** 75+ minutes per week on context re-explanation — permanently

**What changes:** Every module in this course now works better. Your Confluence searches are more targeted. Your Azure DevOps stories are more accurate. Your Miro synthesis uses the right terminology. Your PRDs reference the right stakeholders.

**This is the module that makes the other modules 10x more powerful.**

---

## Congratulations — You've Completed the Product Catalogue PM Course!

You've connected all 4 MCPs to your real tools and built a permanent AI brain for your domain.

**What you can do now:**
- Search pp space from VSCode in seconds
- Create [FeatureTag] stories with cascade epics in 3 minutes
- Synthesise architecture reviews in 9 minutes
- Run a GPSR PRD draft without re-explaining your domain
- Bootstrap context for any new initiative in 5 minutes

**What to do next:**
1. Book 20 minutes to run the Bootstrap Power Move across your 5 most active Confluence initiatives
2. Share this course with another PM on the E-Commerce Foundations team
3. Add to your `.context/` files as your domain evolves

**Need help or found a bug?** Contact Conrad Annan (PM, ASOS) or open an issue on the course repo.

See you in the next level!
