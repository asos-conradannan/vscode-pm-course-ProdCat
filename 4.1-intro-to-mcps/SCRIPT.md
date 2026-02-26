# Module 4.1: Introduction to MCPs
## Product Catalogue Edition

**Teaching Script for GitHub Copilot**
**Duration:** ~20 minutes

---

## Teaching Flow

Welcome to Module 4: MCP Integration for Product Managers!

This is where you unlock **supercharged PM workflows** by connecting Copilot to your actual tools

**What are MCPs?** Model Context Protocol servers — they let Copilot directly interact with tools like Confluence, Azure DevOps, Figma, and Miro

Instead of copying and pasting between tools, Copilot can read from and write to them directly

As a Product Catalogue PM, you live across the `pp` Confluence space, the E-Commerce Foundations Azure DevOps board, Miro architecture diagrams, and Figma internal tool designs. MCPs connect all of these into a single workflow — right here in VSCode.

STOP: Have you used any AI integrations with your PM tools before? Or is this concept new?

USER: Responds

---

## Why MCPs Matter for Product Catalogue PMs

Let me show you the difference with examples from your actual work:

**Without MCPs:**
- You: Find GPSR requirements in Confluence → Copy into Copilot → Get analysis → Copy acceptance criteria back to Azure DevOps
- Time: 10-15 minutes per story
- Context: Degraded each time you paste

**With MCPs:**
- You: "Search pp space for GPSR manufacturer data requirements and create a user story in the Product Catalogue board"
- Copilot: Reads Confluence, understands the requirement, creates the story with technical AC
- Time: 90 seconds
- Context: Perfect fidelity

MCPs eliminate the "swivel chair" problem — the constant switching between Confluence tabs, Azure DevOps, Miro boards, and Figma links that defines a platform PM's day.

STOP: What tool do you spend the most time switching to and from each day?

USER: Responds

---

## What You'll Learn in Module 4

Over the next 6 lessons, you'll master MCP integration with:

**4.2 Confluence** — Search pp space, write feature docs, create API contract pages, sync feature logs
**4.3 Azure DevOps** — Create [FeatureTag] stories, cascade epics, query PC team sprint analytics
**4.4 Figma** — Reference internal tool designs (Product Preview, BackOffice, Speed-to-Market dashboard)
**4.5 Miro** — Extract architecture review outputs, retro themes, quarterly roadmap cards
**4.6 Context Layers** — Build `.context/` files so Copilot permanently knows your domain (the game-changer)

Each lesson follows the same pattern:
1. **Connect** — Set up the MCP
2. **Explore** — See what it can do
3. **Practice** — Real Product Catalogue workflows
4. **Integrate** — Combine with existing work

STOP: Which tool are you most excited to connect?

USER: Responds

---

## How MCPs Work (Simple Version)

You don't need to understand the technical details, but here's the concept:

```
┌─────────────┐
│   VSCode    │
│  + Copilot  │
└──────┬──────┘
       │
       │ MCP Protocol
       │
┌──────┴──────────────────────────────┐
│                                     │
│  ┌──────────┐  ┌───────────────┐   │
│  │Confluence│  │  Azure DevOps │   │
│  │   MCP    │  │     MCP       │   │
│  └────┬─────┘  └──────┬────────┘   │
│       │               │            │
│  ┌────▼─────┐  ┌──────▼─────────┐ │
│  │pp space  │  │E-Commerce Fnd  │ │
│  │Confluence│  │Product Catalogue│ │
│  └──────────┘  └────────────────┘ │
└────────────────────────────────────┘
```

MCPs are like translators — they speak "Copilot language" and "Tool API language"

You just talk to Copilot naturally. The MCP handles the rest.

STOP: Ready to see this in action?

USER: Confirms

---

## Setting Up Your First MCP

**Prerequisites** (check with your IT team):
- VSCode with Copilot installed ✓ (you have this)
- MCP extension installed (we'll do this)
- Access credentials for each tool (Confluence pp space, Azure DevOps, etc.)

**The Setup Process** (3 steps):
1. Install MCP extension in VSCode
2. Configure connection to each tool
3. Authenticate with your credentials

Don't worry — each lesson in Module 4 will walk you through setup for that specific tool

STOP: Do you have access to the pp Confluence space and the E-Commerce Foundations Azure DevOps project?

USER: Responds

ACTION: Note any access issues. If they don't have pp space access, guide them to request it from the PC team.

---

## MCP Commands You'll Use

Once connected, you'll use Copilot naturally:

**Confluence (pp space) Examples:**
- "Search pp space for GPSR manufacturer data requirements"
- "Create a new page in the pp space with my GPSR PRD"
- "Get the Product Catalogue Feature Log page and list active initiatives"
- "Search pp and Engineering spaces for all IndexNow integration docs"

**Azure DevOps Examples:**
- "Show me all work items in the Product Catalogue team board"
- "Create a user story: [GPSR] - Product Catalogue API - Add manufacturer contact fields to FullProduct endpoint"
- "What's the Product Catalogue team velocity over the last 3 sprints?"
- "Create 3 linked stories for the FlatLays epic: Models → Publisher → API"

**Figma Examples:**
- "Show me the latest designs for the Product Preview tool"
- "Extract layout specs from the BackOffice product locate screen"
- "What components are in the Speed-to-Market Celonis dashboard design?"

**Miro Examples:**
- "Get all sticky notes from the Product Catalogue architecture target state board"
- "Extract action items from the last PC team retro board"
- "Synthesise the quarterly roadmap cards from our planning board"

See the pattern? **Natural language, tool-specific actions — all in your domain language**

STOP: Try drafting a command you'd like to use. Format: "[Tool]: [Action in PC terms]"

USER: Writes example command

---

ACTION: Review their command and give feedback:
- Is it specific to PC team context?
- Does it reference the right Confluence space (pp) or the right ADO project?
- How could it be more precise with PC terminology?

---

## MCP Benefits for Product Catalogue PMs

Let me be specific about time savings for your actual work:

| Task | Without MCP | With MCP | Savings |
|------|-------------|----------|---------|
| Search pp space for API contract docs | 12 min | 45 sec | 94% |
| Create feature log entry in Confluence | 8 min | 2 min | 75% |
| Create 3 cascading stories (Models → Publisher → API) | 15 min | 3 min | 80% |
| Analyse Product Catalogue sprint velocity | 20 min | 4 min | 80% |
| Synthesise architecture workshop output | 90 min | 10 min | 89% |
| Build stakeholder update from ADO + Confluence | 45 min | 10 min | 78% |

**Total weekly time saved:** 15-20 hours (assuming these tasks happen across the week)

That's time back for actual product thinking — understanding publication pipeline constraints, working through compliance implications, or driving architecture decisions

STOP: Which task would save you the most time?

USER: Responds

---

## A Product Catalogue PM's Day with MCPs

Here's what a typical day looks like with MCP integration — in your actual world:

**9:00 AM — Daily Standup Prep (2 minutes)**
- "Get all Product Catalogue work items updated yesterday in E-Commerce Foundations"
- Review FlatLays stories in Active state, check for blockers

**10:00 AM — Story Refinement**
- "Search pp space for GPSR manufacturer data requirements"
- "Using those requirements, update the acceptance criteria on work item 1172555"
- "Search Confluence for the Product Catalogue API contract and extract the FullProduct endpoint schema"

**2:00 PM — Sprint Planning Prep**
- "Analyse last 3 sprints velocity for the Product Catalogue team"
- "Get the Feature Log page from pp space and list all active initiatives with status"
- "Create a sprint plan document for the upcoming sprint based on team capacity"

**4:00 PM — Stakeholder Update**
- Pull GPSR initiative status from Azure DevOps epic
- Pull related Confluence doc links
- Publish summary to pp space
- Share link with PDP team, Search team, and BackOffice stakeholders

**Total tool switches:** 0 — everything in VSCode/Copilot

STOP: How does your current workflow compare to this?

USER: Responds

---

## Important: MCP Limitations

MCPs are powerful, but they have boundaries:

**What MCPs CAN do:**
✓ Read data from tools
✓ Create/update items (stories, pages, comments)
✓ Search and filter across spaces and boards
✓ Extract and synthesise information
✓ Automate repetitive tasks (feature log updates, story creation)

**What MCPs CANNOT do:**
✗ Make product decisions for you
✗ Understand context you haven't provided (yet — Module 4.6 solves this)
✗ Replace engineering judgment on API design
✗ Work without proper permissions
✗ Guarantee 100% accuracy — always verify API contracts and compliance rules

**For a Product Catalogue PM specifically:**
MCPs are brilliant for the busywork — searching for specs, creating stories, updating docs. They are not a replacement for understanding the publication pipeline, making trade-off decisions on cache invalidation strategy, or knowing when a GPSR requirement is ambiguous.

STOP: Does this make sense? Any concerns about relying on MCPs?

USER: Responds

---

## Security & Permissions

Important security notes:

**Your data stays secure:**
- MCPs use your existing Atlassian and Azure DevOps credentials
- Same permissions as when you use the tools directly in browser
- No data stored outside ASOS systems
- All connections encrypted

**Best practices:**
1. Only connect to official MCPs approved by ASOS IT
2. Review permissions when authenticating
3. Never paste credentials or API tokens into prompts
4. Disconnect MCPs you're not actively using
5. Follow ASOS security policies — especially for product data containing PII

STOP: Any security questions before we proceed?

USER: Asks questions or confirms ready

---

## Module 4 Roadmap

Here's what's coming:

**Lesson 4.2: Confluence** (~30 min)
- Search pp space without browser
- Create GPSR PRD, API contract docs, feature log entries
- New: API contract documentation workflow

**Lesson 4.3: Azure DevOps** (~30 min)
- Create [FeatureTag] stories in PC format
- Multi-story epic cascades (Models → Publisher → API)
- Security/tech-debt story pattern
- Sprint analytics for Product Catalogue team

**Lesson 4.4: Figma** (~25 min)
- Internal tool designs
- Product Preview, BackOffice, Speed-to-Market
- Design handoff for internal tooling

**Lesson 4.5: Miro** (~25 min)
- Architecture review extraction
- Retro synthesis (PIM sync issues, cache issues)
- Quarterly pipeline roadmap

**Lesson 4.6: Context Layer Setup** (~35 min) ← DO THIS LAST
- Build `.context/` files permanently
- Use Confluence MCP to bootstrap your context from pp space docs
- The "zero to context" workflow — your permanent AI brain for PC domain

**Total time investment:** ~3 hours
**Time saved weekly from week 1:** 15-20 hours

That's a **750-1000% ROI** in your first month

STOP: Ready to start connecting tools?

USER: Confirms

---

## Next Steps

To prepare for the next lessons, do this:

**Before Lesson 4.2 (Confluence):**
1. Confirm you have pp space access in Confluence
2. Note 2-3 pages you use most often (Feature Log, API contracts, team overview)
3. Think of a doc you want to create or update this sprint

**Before Lesson 4.3 (Azure DevOps):**
1. Confirm access to E-Commerce Foundations project
2. Know the Product Catalogue team name on the board
3. Identify the most repetitive story-writing task you do

**Before Lesson 4.4 (Figma):**
1. Confirm Figma access (view permissions)
2. Find the internal tool design you reference most often

**Before Lesson 4.5 (Miro):**
1. Confirm Miro access
2. Find your most recent architecture or planning board

**All set?** Let's move to Lesson 4.2 and connect to Confluence!

STOP: Any final questions about MCPs?

USER: Asks questions or confirms ready

---

## Reflection

You now understand:
✓ What MCPs are and why they matter for a backend platform PM
✓ How they save Product Catalogue PMs 15-20 hours per week
✓ The pp space and E-Commerce Foundations project are your primary data sources
✓ Security and permission considerations
✓ Real Product Catalogue workflow examples

**Your new advantage:** Domain knowledge + AI speed = unstoppable platform PM

Next up: **Hands-on Confluence integration with the pp space**

See you in Module 4.2!
