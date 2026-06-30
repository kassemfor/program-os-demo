# Notion Recreation Guide

**For buyers who use Notion.** This document maps the AI Program OS HTML to a Notion workspace that you can build in ~2 hours. It covers structure, the database schemas, the views, and the templates.

If you prefer the standalone HTML version, just open `index.html` in any browser. Everything saves to localStorage and works offline. The HTML version is the recommended deliverable — the Notion recreation is a bonus.

---

## Notion Workspace Structure

Create a new top-level page: **AI Program OS — [Program Name]**

Inside it, build these sub-pages and databases:

```
📁 AI Program OS — [Program Name]
│
├── 📄 Program Charter              (single page, properties = charter fields)
├── 📊 Stakeholders                  (database)
├── 📊 Vendors                       (database)
├── 📊 RAID Log                      (database)
├── 📊 Decision Log                  (database)
├── 📊 Scorecard Tracker             (database)
├── 📅 Weekly Status Archive         (database of weekly statuses)
├── 📄 This Week                     (page — auto-pre-filled from RAID)
└── 📄 Next SteerCo Pack             (page — auto-generated one-pager)
```

---

## Database Schemas

### 📊 Stakeholders

| Property | Type | Notes |
|----------|------|-------|
| Name | Title | Person's name |
| Role | Text | Title / function |
| Influence | Select | Low / Medium / High |
| Interest | Select | Low / Medium / High |
| Last contact | Date | |
| Sentiment | Select | Champion / Supporter / Neutral / Skeptic / Blocker |
| Next action | Text | Single concrete next action |

**Views to create:**
- Table (default)
- Board by Sentiment — for at-a-glance health
- Filtered: Sentiment = Blocker OR Skeptic (for risk review)

### 📊 Vendors

| Property | Type | Notes |
|----------|------|-------|
| Vendor | Title | Name |
| What they provide | Text | |
| Annual $ | Number | Or total contract value |
| Renewal | Date | |
| Risk | Select | Low / Medium / High |
| Exit clause | Text | Documented exit terms |

**Views:**
- Table (default)
- Calendar by Renewal date (catch expiring contracts 90 days out)
- Filtered: Risk = High

### 📊 RAID Log

| Property | Type | Notes |
|----------|------|-------|
| Description | Title | The actual item |
| Type | Select | Risk / Assumption / Issue / Dependency |
| Owner | Person | Or text if not using a People field |
| Severity | Select | High / Medium / Low |
| Status | Select | Open / In Progress / Closed |
| Action / Mitigation | Text | |
| Date logged | Created time | Auto |

**Views (critical):**
- Table (default)
- Board by Status (Kanban — review weekly)
- Board by Owner (accountability check)
- Filtered: Status != Closed AND Type = Risk AND Severity = High (top of weekly status)
- Filtered: Status != Closed (the "open" view — review every Monday)

### 📊 Decision Log

| Property | Type | Notes |
|----------|------|-------|
| Decision | Title | |
| Date | Date | |
| Made by | Text | |
| Evidence | Text | |
| Status | Select | Pending / Approved / Rejected |

**Views:**
- Table by Date desc (default)
- Filtered: Status = Pending (the "awaiting" view)

### 📊 Scorecard Tracker

| Property | Type | Notes |
|----------|------|-------|
| Quarter | Title | e.g. 2026-Q3 |
| Strategy | Number | 0-50 |
| Data | Number | 0-50 |
| Tech | Number | 0-50 |
| GRC | Number | 0-50 |
| Talent | Number | 0-50 |
| Delivery | Number | 0-50 |
| Adoption | Number | 0-50 |
| Vendor | Number | 0-50 |
| Agentic | Number | 0-50 |
| Comms | Number | 0-50 |
| Total | Formula | Sum of all section scores |
| Band | Formula | IF Total < 100 then "Nascent" else IF Total < 200 then "Forming" else IF Total < 350 then "Operational" else IF Total < 450 then "Scaling" else "Leading" |

**Formulas (Notion syntax):**

```
Total:
  prop("Strategy") + prop("Data") + prop("Tech") + prop("GRC") + prop("Talent") + prop("Delivery") + prop("Adoption") + prop("Vendor") + prop("Agentic") + prop("Comms")

Band:
  if(prop("Total") < 100, "Nascent",
    if(prop("Total") < 200, "Forming",
      if(prop("Total") < 350, "Operational",
        if(prop("Total") < 450, "Scaling", "Leading"))))
```

**Views:**
- Table by Quarter (default)
- Line chart of Total over time (manual setup via Notion's chart view in newer versions)
- The "single point of failure" view: filter by each section to see trends

### 📅 Weekly Status Archive

| Property | Type | Notes |
|----------|------|-------|
| Week ending | Title | Date |
| RAG | Select | Green / Amber / Red |
| Progress | Text | |
| Decisions needed | Text | |
| Risks escalating | Text | |
| Next week | Text | |
| Sent to | Text | Email list |
| Sent at | Date | When sent |

---

## Templates

### Program Charter Template

```
# [Program Name]

**Sponsor:** [Name]
**Delivery lead:** [Name]
**Business owner:** [Name]
**Risk lead:** [Name]

## The decision being automated
[One sentence]

## Counterfactual — what we do today, what it costs
[2-3 paragraphs]

## Quantified outcome (delta against counterfactual)
[Specific, measurable]

## What the AI is NOT allowed to do
[Boundary list]

## Trust floor
[Minimum acceptable performance]

## Funding horizon
[Months committed]

## Steering committee
- Cadence: [Weekly / Bi-weekly / Monthly]
- Next: [Date]

## Kill criteria
1. [Condition 1]
2. [Condition 2]
3. [Condition 3]
```

### Weekly Status Template

```
# Weekly Status — Week ending [DATE]

**RAG:** [Green / Amber / Red]

## Progress (what actually moved)
- [Bullet 1]
- [Bullet 2]
- [Bullet 3]

## Decisions needed (by when)
1. [Decision] — by [Date]
2. [Decision] — by [Date]

## Risks escalating
- [Risk] (Owner: [Name])
- [Risk] (Owner: [Name])

## Next week — top 3
1. ...
2. ...
3. ...

---
Sent to: [email list]
Sent at: [timestamp]
```

### SteerCo Pack Template

```
# [Program Name] — Steering Committee
[Date] · [Cadence]

## RAG / Health
- RAG: [Green / Amber / Red]
- Open risks: [N] ([M] high)
- Pending decisions: [N]

## The decision being automated
[One sentence from charter]

## Outcome being delivered
[Quantified outcome from charter]

## Decisions required from SteerCo
[From weekly status]

## Top risks (top 5)
| Severity | Description | Owner |
|----------|-------------|-------|
| High | ... | ... |
| High | ... | ... |
| Medium | ... | ... |

## Next week priorities
[From weekly status]

---
Generated [date]
```

---

## Building This in Notion (Step by Step)

**Time required: 2 hours.**

1. **Create the top page** (5 min) — "AI Program OS — [Program Name]"
2. **Build the Charter page** (15 min) — Use the template above as the body
3. **Create the 5 databases** (45 min) — Stakeholders, Vendors, RAID, Decisions, Scorecard
   - For each: create the database inline, add properties from the schemas
   - Create the views listed above
4. **Create the Weekly Status Archive database** (15 min)
5. **Create the templates** (15 min) — Save each template as a Notion template in its respective database
6. **Wire up the linked databases** (15 min) — On the Charter page, add linked database views showing:
   - "Open RAID items" (filtered view)
   - "Pending decisions" (filtered view)
   - "Steerco readiness" (everything due before next steerco date)

---

## Why The HTML Version Is Better

The standalone HTML version (in `index.html`) wins for Kris's buyer because:

| | HTML version | Notion recreation |
|---|---|---|
| Offline | ✅ Works fully offline | ❌ Requires Notion login |
| Privacy | ✅ Data never leaves the browser | ⚠️ Notion sees everything |
| Speed | ✅ Loads in 1 second | ❌ Notion is slow with many databases |
| One-click delivery | ✅ Email the .html file | ❌ Requires Notion account setup |
| Print/PDF | ✅ Print-ready CSS built in | ❌ Manual setup |
| Backup | ✅ One JSON export | ⚠️ Notion export is awkward |

**Recommended positioning:** Lead with the HTML version. Offer the Notion recreation guide as a bonus included with the bundle. Position as "use the HTML, recreate in Notion if your team insists on it."

---

## Pricing The Operating System

Recommended SKU structure (when Kris is ready to add it to the lineup):

| Tier | Includes | Price |
|------|----------|-------|
| **AI Program OS (HTML)** | `index.html` only | $97 |
| **AI Program OS (Notion)** | Notion recreation guide + 2hr implementation walkthrough | $67 |
| **AI Delivery OS Bundle** | HTML OS + Notion recreation + all 3 PDFs | $197 |

The HTML version is the premium product because it's a working tool, not a template. Buyers get immediate utility. Position it as "open and use in 30 seconds."