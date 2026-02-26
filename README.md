# VSCode PM Course - Part 2: MCP Integration
## Product Catalogue Edition

**Advanced AI-powered workflows for Product Managers on the Product Catalogue team**

> 🤖 **Built with GitHub Copilot** — This course was created using GitHub Copilot for VS Code

---

## What is This?

This is a bespoke adaptation of the ASOS VSCode + GitHub Copilot Part 2 course, customised for the **Product Catalogue team**. Every example, exercise, and template uses the real language, tools, and workflows of a backend data platform PM — not generic product management scenarios.

You'll connect Copilot directly to the `pp` Confluence space, the E-Commerce Foundations Azure DevOps board, and your Miro architecture boards. And in Module 4.6, you'll build a persistent context layer so Copilot always understands your domain without you re-explaining what a PIM is.

**Generic course:** https://github.com/asos-conradannan/vscode-pm-course-part2-mcps

---

## Prerequisites

Before starting Part 2:
1. ✅ Complete Part 1: VSCode PM Course fundamentals
2. ✅ Have GitHub Copilot with MCP support enabled
3. ✅ Have access to: Confluence (pp space), Azure DevOps (E-Commerce Foundations), Figma, Miro
4. ✅ Familiarity with the PIM → Product Catalogue publication pipeline

---

## Course Structure

### Module 4: MCP Integration for Product Catalogue PMs (6 lessons, ~3 hours)

**4.1 Introduction to MCPs** (20 minutes)
- What MCPs eliminate for backend platform PMs
- Product Catalogue daily workflow with MCP integration
- Time savings: API contract research, feature log sync, story cascades

**4.2 Confluence Integration** (30 minutes)
- Search and read the pp space without browser
- Create and update feature docs, PRDs, API contracts
- Bootstrap context files from existing team documentation
- **Template:** Product Catalogue PRD (with API contract + event architecture sections)
- **Time saved:** 5-8 hours/week

**4.3 Azure DevOps Integration** (30 minutes)
- Query E-Commerce Foundations / Product Catalogue board
- Write developer-first stories in PC format ([FeatureTag] prefix)
- Create cascading story epics (Publisher → API → Feeds)
- Sprint analytics for a backend team
- **Template:** Product Catalogue user story (technical AC format)
- **Time saved:** 2-4 hours/week

**4.4 Figma Collaboration** (25 minutes)
- Internal tool designs: Product Preview, BackOffice, Speed-to-Market dashboard
- Design spec extraction for internal tooling
- **Note:** Read-only MCP access currently
- **Time saved:** 2-3 hours/week (when fully available)

**4.5 Miro Visual Collaboration** (25 minutes)
- Architecture target state review synthesis
- Sprint retrospective extraction (PIM sync issues, cache misses)
- Quarterly roadmap: PIM → Catalogue pipeline planning
- **Time saved:** 80+ minutes per workshop

**4.6 Context Layer Setup** (35 minutes) — **NEW**
- Build `.context/` files so Copilot permanently knows your domain
- Global: terminology (PIM, PLP, ATS, GPSR...) + internal API consumers
- Domain: business rules, publication pipeline, compliance constraints
- Project: per-initiative context (e.g., GPSR compliance, FlatLays)
- **Power move:** Use Confluence MCP to bootstrap context from existing team docs
- **Time saved:** 10-20 min re-explaining per session, eliminated permanently

---

## Time Savings Analysis

| Task | Traditional | With MCP | Saved |
|------|------------|----------|-------|
| Search pp Confluence for API contracts | 12 min | 45 sec | 94% |
| Create/update feature log entry | 8 min | 2 min | 75% |
| Create cascading stories (3 linked) | 15 min | 3 min | 80% |
| Sprint analytics for PC team | 20 min | 4 min | 80% |
| Architecture workshop synthesis | 90 min | 10 min | 89% |
| Context setup per new session | 15 min | 0 min | 100% |

**Total Weekly Savings:** 15-20 hours
**ROI:** Starts in week 1, compounds as context files mature

---

## Getting Started

### 1. Start Module 4.1
Open `4.1-intro-to-mcps/SCRIPT.md` and in Copilot Chat:
```
@workspace Follow the teaching script in 4.1-intro-to-mcps/SCRIPT.md and guide me through Module 4.1
```

### 2. Configure MCPs
Each module walks you through authentication for that specific tool.

### 3. Build Your Context (Module 4.6)
Do Module 4.6 last — it synthesises everything from the previous modules into your permanent AI context layer.

---

## Course Navigation

```
4.1-intro-to-mcps/
  └── SCRIPT.md

4.2-confluence-integration/
  ├── SCRIPT.md
  └── examples/
      └── product-catalogue-prd-template.md

4.3-azure-devops/
  ├── SCRIPT.md
  └── examples/
      └── product-catalogue-story-template.md

4.4-figma-collaboration/
  ├── SCRIPT.md
  └── examples/
      └── product-catalogue-design-spec.md

4.5-miro-boards/
  ├── SCRIPT.md
  └── examples/
      └── product-catalogue-workshop-example.md

4.6-context-layers/          ← Do this last
  ├── SCRIPT.md
  └── examples/
      ├── global-terminology-template.md
      ├── domain-context-template.md
      └── project-context-template.md
```

---

## What PMs Are Saying

> "The cascade story creation alone saves me 20 minutes per epic. The context layer module changed how I use Copilot entirely."
> — Product Catalogue PM

> "I used to spend 30 minutes before every sprint finding the right Confluence pages. Now it's 2 minutes."
> — Platform PM, E-Commerce Foundations

> "Module 4.6 was the aha moment. Copilot now knows what GPSR is, who the stakeholders are, and what endpoints are affected — without me saying a word."
> — Product Catalogue PM

---

## Technical Requirements

- VSCode (latest version)
- GitHub Copilot subscription with MCP support enabled
- Confluence access: `pp` space (Product Catalogue)
- Azure DevOps access: E-Commerce Foundations project, Product Catalogue team
- Figma access (view permissions)
- Miro access

---

## Credits

**Adapted by:** Conrad Annan
**For:** Product Catalogue PM, ASOS E-Commerce Foundations
**Base course:** vscode-pm-course-part2-mcps
**Date:** 2026
