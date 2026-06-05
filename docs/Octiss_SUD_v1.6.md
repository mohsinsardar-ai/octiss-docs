# Octiss — Solution Understanding Document (SUD)
**Version:** v1.6.7 | **Date:** 05 June 2026 | **Status:** LIVE — BETA UNBLOCKED
**Owner:** Mohsin Sardar — EM Intelligence Labs  
**Classification:** Confidential — Internal Use Only  
**Domains:** octiss.com | emintelligencelab.com

> **PURPOSE:** This is the single source of truth for what Octiss should be and why.  
> It is NOT a build log or task tracker — those live in the Continuation Prompt.  
> **Every Claude session and every Codex prompt must read this document first.**  
> Update immediately whenever an architectural decision is made.

---

## Changelog

| Version | Date | Changes |
|---|---|---|
| v1.0 | 01 Jun 2026 | Initial release — vision, architecture, 11 agents, UX principles, decisions log |
| v1.1 | 01 Jun 2026 | Section 3 expanded — 5-level task hierarchy, module expansion patterns, scheduling CTAs, module library, phase duration benchmarks (M10b-3). Section 9 updated with 4 new decisions. Agent count corrected to 12. |
| v1.2 | 01 Jun 2026 | SF (Bluefield) deferred from v1.0 scope. GF and BF only. Section 2.1 and task library counts updated. New decision added to Section 9. |
| v1.3 | 01 Jun 2026 | M10b-2 complete. Final task library locked: GF 193 active tasks, BF 201 active tasks. Project Plan Agent architecture fully defined — SOW extraction, conditional task activation, WRICEF CR flow, Path A/B setup. Section 4.1 expanded. New section 4.2 added. Section 9 updated with 6 new decisions. |
| v1.4 | 01 Jun 2026 | M10b-5 complete — 394 tasks mapped to 12 agents. Three new sections added: Change Request workflow (Section 5), Escalation workflow (Section 6), Session Attendance and No-Show Escalation (Section 7). New DB tables documented: project_escalations, session_attendance, meetings attendance fields. Section 9 updated with 7 new decisions. |
| v1.5 | 03 Jun 2026 | M10b-6b, M10b-6a, Seed, M10b-7 all confirmed complete. |
| v1.6 | 04 Jun 2026 | M10b-8 complete. Command Center rebuilt (6 sections). M10b-6c BF Conversion Module code complete. Backend had 393 routes and 1294 tests passing. Frontend had 103 Playwright tests passing / 48 skipped. Production BF migration still required manual Supabase SQL Editor apply. Microsoft was believed connected at the UI level, later corrected by v1.6.2 API verification. |
| v1.6.1 | 05 Jun 2026 | CLI environment update: Supabase CLI linked to production and dry-run reaches remote DB; jq installed and verified; Railway CLI v5.0.0 authenticated and linked to octiss-production / sap-pmo-agent. BF migration remains pending until migration filenames are normalized to Supabase timestamp format. |
| v1.6.2 | 05 Jun 2026 | Production verification update: backend 394 routes / 1302 tests passing; frontend 107 Playwright tests passing / 48 skipped; GF/BF production test projects verified; BF migration and `service_role` grants applied; GF task save/readback and BF checklist/SUM/downtime flows verified. Microsoft 365 production API remains blocked (`configured=false`, `connected=false`, `verified=false`) until Railway MS365 env/OAuth are configured. |
| v1.6.3 | 05 Jun 2026 | MS365 Railway env vars added in Railway production. Tester workspace status endpoint now reports `configured=true`, while OAuth account connection/verification remains pending (`connected=false`, `verified=false`). |
| v1.6.4 | 05 Jun 2026 | Microsoft OAuth Network Error fixed in backend `8f1ecfb`: MSAL-reserved `offline_access` removed from runtime request scopes, `/microsoft/auth` alias added, and production auth URLs verified to use the `-3f52` callback. |
| v1.6.5 | 05 Jun 2026 | MS365 fully connected in production. OneDrive, Outlook, Calendar, and Project Document Folders are configured/connected. SharePoint is N/A for the personal Hotmail account and is not required for beta. Beta is now unblocked. |
| v1.6.6 | 05 Jun 2026 | Phase View active project restoration fixed in frontend `5ccd18f` and selector/project-list timing hardened in `ef425b5`. `/phase/{phase}` now resolves from the shared project list and auto-restores only on routes that require an active project. Tester1 production projects endpoint returns both BF Test Project and GF Test Project. Frontend Playwright suite: 108 passed / 48 skipped. |
| v1.6.7 | 05 Jun 2026 | Command Center and top-level project workspaces now use shared active-project resolution in frontend `9050455`. `/dashboard` restores the last active project after fresh login, GF/BF switching updates Command Center, and no remembered project shows a prominent selector. Frontend Playwright suite: 110 passed / 48 skipped. |
| v1.6.8 | 05 Jun 2026 | Project Initiation, independent task notes/dates/assignment saves, Project/SAP agent context, TRACK owner defaults, and GF/BF generic production seeding completed. Frontend `c214c01`; backend `fb67edf`. Frontend Playwright suite: 110 passed / 48 skipped. Backend compile + direct project-task route tests passed locally; production Project Initiation migration/seed applied. |

---

## 1. Product Vision

### 1.1 What Octiss Is

Octiss is an AI-powered SAP PMO Intelligence Platform. It guides SAP Project Managers, Programme Managers, and PMO leads through every phase of an SAP implementation — task by task, phase by phase — using 12 specialist AI agents embedded directly into the project workflow.

**Octiss is not a dashboard with tools. Octiss is a guided SAP Activate journey.**

The platform walks the PM through the methodology in the correct sequence, surfacing the right agent, the right document template, and the right intelligence at exactly the right moment.

### 1.2 One-Liner

> **Intelligence for Every Project. Clarity for Every Decision.**

### 1.3 Who It Is For

- SAP Project Managers managing S/4HANA implementations
- SAP Programme Managers overseeing multi-project or multi-wave programmes
- PMO Leads running governance across a portfolio of SAP projects
- System Integrator delivery teams needing structured SAP Activate guidance

### 1.4 What It Is Not

- Not a generic project management tool (not Jira, Asana, or MS Project)
- Not a chatbot — agents are contextual and task-triggered, not open-ended chat
- Not a SAP tool — it sits alongside SAP Cloud ALM, it does not replace it

---

## 2. Methodology Coverage

### 2.1 Supported Methodologies

> **DECISION (01 Jun 2026):** Only GF and BF are in scope for Octiss v1.0. SF (Bluefield) is deferred to a future version.

| Code | Methodology | SAP Branding | Status | Use Case |
|---|---|---|---|
| GF | New Implementation | Rise with SAP (Private Cloud) | ✅ Active | Net-new S/4HANA — no legacy system migrated |
| BF | System Conversion | Grow with SAP (Private Cloud) | ✅ Active | Convert existing ECC/S/4HANA to S/4HANA |
| SF | Selective Data Transition | Bluefield (Private Cloud) | ⏸ Deferred | Not in scope for v1.0. GF and BF only. SF to be added in a future version. |

### 2.2 Discover Phase — Excluded

> **DECISION:** Discover phase is excluded from Octiss scope. Octiss starts at PREPARE.

Discover is a pre-sales and pre-contract phase. By the time a PM uses Octiss, the decision to implement has been made, the contract is signed, and the methodology is confirmed.

### 2.3 Active Phases in Octiss

| # | Phase | Purpose |
|---|---|---|
| 1 | Prepare | Project initiation, governance, team mobilisation, technical setup, planning |
| 2 | Explore | Fit-to-Standard workshops, BPML, solution design, gap analysis, data migration strategy |
| 3 | Realize | Configuration, build, unit testing, SIT, UAT, data migration runs |
| 4 | Deploy | Cutover planning, dress rehearsal, go-live execution, hypercare initiation |
| 5 | Run | Hypercare support, issue triage, knowledge transfer, project closure |

---

## 3. Task Architecture

### 3.1 Five-Level Hierarchy

> **DECISION:** Task architecture is 5 levels. Levels 1-3 from SAP Activate. Levels 4-5 generated dynamically by the Project Plan Agent on project creation.

| Level | Name | Source | Example |
|---|---|---|---|
| 1 | Phase | SAP Activate — fixed | Explore |
| 2 | Deliverable Group | SAP Activate — fixed | Fit-to-Standard Workshops |
| 3 | Methodology Task | SAP Activate — fixed | Conduct Fit-to-Standard Workshop |
| 4 | Module Instance | **Generated on project creation** | Conduct FTS Workshop — FICO |
| 5 | Sub-Task / Activity | **Generated on project creation** | Prepare Workshop Agenda — FICO |

Levels 4 and 5 are generated by the **Project Plan Agent** — NOT manually entered by the PM.

### 3.2 SAP Activate Task Library — Source & Counts

**Source:** SAP Roadmap Viewer (me.sap.com/roadmapviewer) — April 2026  
**Discover phase excluded from all counts.**

| Methodology | Deliverable Groups | Tasks |
|---|---|---|
| GF — New Implementation | 95 | 312 |
| BF — System Conversion | 87 | 292 |
| SF — Selective Data Transition | 90 | 299 | ⏸ Deferred — not loaded |
| **Active Total (GF + BF)** | **182** | **394** |
| GF — Active (KEEP + TRACK) | 95 | 193 | ✅ Final — locked |
| BF — Active (KEEP + TRACK) | 87 | 201 | ✅ Final — locked |
| SF (Deferred) | 90 | 299 | ⏸ Not in scope |

### 3.3 Task Decision Categories

| Category | Owner | Octiss Behaviour | Example |
|---|---|---|---|
| **KEEP** | PM | PM owns and executes. Full task screen with agent CTA. | Create Project Charter |
| **TRACK** | Basis / IT / SAP | PM monitors as dependency. Overdue = auto RAID risk raised by RAID Agent. | Initial System Access to SAP S/4HANA Landscape |
| **MERGE** | PM | Too granular. Consolidated into single PM-level task. | Multiple micro-tasks merged |
| **EXCLUDE** | SAP Internal | Not included in Octiss. PM has zero ownership. | Self-Enable on SAP Fiori |

### 3.4 Module Expansion Rules — Tasks That Expand by Module (Level 4)

**Confirmed from analysis of 7 real SAP project plans.**

#### Explore Phase — Expands by Module
| Task (Level 3) | Schedule CTA? |
|---|---|
| Fit-to-Standard / Design Workshops | YES |
| Validation Workshops | YES |
| BPML Sign-off | NO |
| Business Requirement Document (BRD) Development | NO |
| WRICEF List Preparation and Sign-off | NO |
| Master Data Template Sessions | YES |

#### Realize Phase — Expands by Module
| Task (Level 3) | Schedule CTA? |
|---|---|
| System Configuration (per module) | NO |
| Functional Unit Testing (FUT) | NO |
| System Integration Testing (SIT) | NO |
| Data Uploader Template Development | NO |
| Data Template Fill and Review | NO |
| Train the Trainer (TTT) | YES |
| User Acceptance Testing (UAT) | YES |
| Transport Execution (DEV to QAS) | NO |
| Show & Tell Workshops | YES |

#### Deploy Phase — Expands by Module
| Task (Level 3) | Schedule CTA? |
|---|---|
| End User Training Workshops | YES |
| Cutover Activities | NO |
| Go-Live Readiness Check | NO |

### 3.5 Project-Wide Tasks (Do NOT Expand by Module)

These appear exactly once regardless of module count:

- Project Awarded / Code Creation *(TRACK)*
- PM Nomination & Onboarding
- Handover from Sales to PM
- Customer Clarification Meetings *(Schedule CTA)*
- Project Charter
- Project Schedule *(generated by Project Plan Agent)*
- Project Kick-off Meeting *(Schedule CTA)*
- Resource Mobilisation
- System Provisioning / IS Ready *(TRACK — Basis)*
- Network & Connectivity *(TRACK — Basis)*
- Workshop Calendar Preparation *(Schedule CTA)*
- Customer Team Onboarding and Enablement *(Schedule CTA)*
- Phase Gate Reviews / Q-Gate / K-Gate *(Governance Agent CTA)*
- RAID Register Updates *(RAID Agent CTA)*
- Phase Closure Certificate
- Phase Technically / Financially Closed
- Cutover Strategy
- Go-Live Declaration
- Provisional Acceptance Certificate
- Hypercare Daily Issue Triage *(Schedule CTA — recurring)*
- Weekly Hypercare Status Report *(Communication Agent CTA)*
- Final Acceptance Certificate
- Project Technically / Financially Closed

### 3.6 Scheduling CTA — Complete Task List

14 task types across all phases surface a Schedule Session button. Meeting Intelligence Agent fires on click and pre-populates agenda from task context. Calendar send via connected Outlook.

| Phase | Task | Session Type | Attendees |
|---|---|---|---|
| Prepare | Project Kick-off Meeting | Full kick-off | All team + customer leadership |
| Prepare | Customer Clarification Meeting | Scope clarification | PM, customer sponsor, key stakeholders |
| Explore | Workshop Calendar Session | Planning meeting | PM + all module leads |
| Explore | Customer Team Onboarding | Enablement workshop | Customer team + PM |
| Explore | FTS / Design Workshop — per module | Fit-to-Standard workshop | PM, consultant (module), key user (module) |
| Explore | Validation Workshop — per module | Validation workshop | PM, consultant (module), business lead |
| Explore | Master Data Template Session — per module | Working session | Consultant, key user, data owner |
| Realize | Show & Tell Workshop — per module | Demo and review | PM, consultant, business users |
| Realize | Train the Trainer — per module | TTT workshop | Consultant, key users, trainers |
| Realize | UAT Workshop — per module | Testing workshop | PM, key users, consultant, QA |
| Deploy | End User Training — per module | Training workshop | Trainers + end users (per module) |
| Deploy | Go-Live Readiness Review | Readiness meeting | All leads + customer |
| Run | Hypercare Daily Triage | Daily standup (recurring) | PM + all module leads |
| Run | Weekly Status Report Meeting | Weekly governance | PM + customer sponsor |

### 3.7 Phase Duration Benchmarks (Greenfield)

Derived from real project plans. Project Plan Agent uses these as defaults — adjustable by PM.

| Phase | Typical Duration | Notes |
|---|---|---|
| Prepare | 20 – 30 days | Varies with team mobilisation speed and system access delays |
| Explore | 90 – 130 days | Driven by number of modules and workshop cycles |
| Realize | 150 – 210 days | Longest phase — config, testing, data migration per module |
| Deploy | 25 – 40 days | Cutover execution, go-live, immediate hypercare |
| Run / Hypercare | 20 – 70 days | Varies by contract |
| **Total (typical GF)** | **300 – 400 days** | Full implementation from Prepare to project closure |

### 3.8 Standard Module Library

PM selects modules during project creation. Project Plan Agent generates Level 4 tasks only for selected modules.

| Module | Name | Workstream | Always TRACK? |
|---|---|---|---|
| FICO | Financial Accounting & Controlling | Finance | NO |
| MM | Materials Management | Logistics / Procurement | NO |
| SD | Sales & Distribution | Logistics / Sales | NO |
| PP | Production Planning | Manufacturing | NO |
| QM | Quality Management | Manufacturing | NO |
| PM | Plant Maintenance | Asset Management | NO |
| WM | Warehouse Management | Logistics | NO |
| HCM / HR | Human Capital Management | HR | NO |
| PS | Project System | Project Management | NO |
| ABAP | Custom Development | Technical | YES |
| Basis | System Administration | Technical | YES |
| Fiori | SAP Fiori / UX | Technical / UX | NO |
| DMS | Document Management | Technical | NO |
| Integration / BTP | Integration & BTP | Technical | YES |
| Security / Auth | Authorization & Security | Technical | YES |
| Analytics / SAC | SAP Analytics Cloud | Analytics | NO |
| Group Reporting | Group Reporting | Finance | BF specific |
| DataSphere | SAP DataSphere | Analytics | BF specific (future) |

> **Note:** ABAP, Basis, Integration/BTP, and Security/Auth are always TRACK — they appear in every project but are executed by technical teams. No scheduling CTA for these modules.

---

## 4. The 12 Specialist AI Agents

> **DECISION:** Octiss has 12 agents, not 11. The Project Plan Agent was missing from the original list. It is the most critical agent — all others reference the plan it generates.

| # | Agent Name | Phase / Scope | AI Model | Role |
|---|---|---|---|---|
| 1 | SAP Activate Governance | All Phases | Opus | Phase gate reviews, quality gates, governance health scoring, methodology compliance |
| 2 | **Project Plan Agent** | Prepare — Project Creation | Opus | **CRITICAL:** Generates full 5-level project plan on project creation. Takes methodology + modules + dates + SOW. All other agents reference this plan. |
| 3 | Prepare Agent | Prepare | Sonnet | Project initiation, governance setup, team onboarding, technical preparation tracking |
| 4 | Explore Agent | Explore | Opus | FTS workshop facilitation, BPML, gap analysis, solution design, WRICEF catalogue |
| 5 | Realize Agent | Realize | Sonnet | Configuration tracking, sprint planning, build-phase oversight, testing coordination |
| 6 | Deploy Agent | Deploy | Opus | Cutover planning, go-live readiness, dress rehearsal, deployment execution |
| 7 | Run / Hypercare Agent | Run | Sonnet | Post go-live monitoring, issue triage, hypercare management, support model |
| 8 | Stakeholder Communication Agent | All Phases | Sonnet | Stakeholder-aware message drafting, escalation support, communication strategy, calendar sends |
| 9 | Meeting Intelligence Agent | All Phases | Sonnet | Meeting minutes, action item extraction, follow-up tracking, workshop documentation, scheduling |
| 10 | RAID / Risk Agent | All Phases | Sonnet | Risk, action, issue, dependency management. Auto-triggers when TRACK task goes overdue. |
| 11 | Brownfield Conversion Agent | All Phases — BF/SF only | Opus | System conversion intelligence, custom code remediation, delta configuration |
| 12 | Daily PM Briefing Agent | All Phases — Command Center | Sonnet | Automated 8AM daily briefing via n8n. Priorities, blockers, overdue tasks, project pulse. |

### 4.1 Project Plan Agent — Detail

**Trigger:** Fires automatically when PM creates a new project in Octiss.

**The SOW / Foundation Document upload is the most important step in project creation.** Everything flows from it — modules, scope, WRICEF count, licensed tools, go-live date, exclusions. Without it, a Q&A flow substitutes.

---

#### Setup Path A — With Documents (Recommended)

PM uploads one or more foundation documents at project creation:

| Document | Priority | Key Extractions |
|---|---|---|
| SOW / Contract | 1st | Scope statement, modules, WRICEF count, exclusions, go-live date, payment milestones, licensed tools |
| Digital Discovery Assessment (DDA) | 2nd | System landscape, process gaps, fit-to-standard areas, technical readiness, data complexity |
| Handover Document | 3rd | What was promised vs sold, customer expectations, key risks from sales, team structure |
| Internal Kick-off Document | 4th | Project code, team assignments, preliminary dates, SI internal commitments |

**Extraction method:** Free-text extraction using Claude Opus. SOW scope is written in narrative paragraphs or numbered lists — not structured tables. Agent reads the full document and extracts structured scope data intelligently.

**After extraction:** Agent presents an editable confirmation screen. PM reviews, corrects, and confirms before plan is generated.

---

#### Setup Path B — Without Documents (Q&A Fallback)

If PM skips document upload, Octiss asks a structured question sequence:

| # | Question | Input Type |
|---|---|---|
| 1 | Methodology confirmed? | GF / BF |
| 2 | Which modules are in scope? | Checklist of 18 modules |
| 3 | Is SAP Analytics Cloud (SAC) licensed? | Y / N |
| 4 | Is WalkMe licensed? | Y / N |
| 5 | Is SAP Build licensed? | Y / N |
| 6 | Is Joule / Business AI in scope? | Y / N |
| 7 | Are integrations in scope? | Y / N → if yes: number of interfaces |
| 8 | Is data migration in scope? | Y / N → if yes: tool-based or manual |
| 9 | What is the approved WRICEF count? | Number → if unknown: enter 0 |
| 10 | Is Group Reporting in scope? | Y / N |
| 11 | Any additional waves planned? | Y / N |
| 12 | Confirmed go-live date? | Date picker |

---

#### Conditional Task Activation Rules

After scope is confirmed (either path), agent applies these rules:

| Task Group | Activates When |
|---|---|
| SAC / Analytics tasks | SAC confirmed as licensed in SOW or Q3=Yes |
| WalkMe tasks | WalkMe confirmed in SOW or Q4=Yes |
| SAP Build tasks | SAP Build confirmed as licensed or Q5=Yes |
| Joule / AI tasks | Joule confirmed in SOW or Q6=Yes |
| ECT / Tricentis tasks | **EXCLUDED by default** — PM activates manually if needed. Comes with Cloud ALM but rarely used. |
| BTP / Integration tasks | Integrations confirmed in SOW or Q7=Yes |
| Data Migration tasks | Data migration confirmed in SOW or Q8=Yes |
| WRICEF tasks | Always activated. Count from SOW or Q9. |
| Group Reporting tasks | Group Reporting confirmed in SOW or Q10=Yes |
| SAP Cloud ALM setup tasks | **EXCLUDED** — Octiss is the PM tool, not Cloud ALM |

---

#### WRICEF Change Request Flow

WRICEF is count-based, not a simple on/off:

```
SOW confirms: WRICEF approved count = 15
  ↓
Project Plan Agent creates 15 WRICEF tracking tasks
  ↓
During Explore — consultant identifies item #16
  ↓
RAID Agent auto-flags:
  "WRICEF count exceeds SOW-approved limit (15)"
  ↓
CTA surfaces on task screen:
  "Draft Change Request for additional WRICEF item"
  ↓
Stakeholder Communication Agent drafts the CR
  ↓
PM reviews → approves → sends to customer
  ↓
Customer approves → WRICEF count updated to 16
  ↓
New task added to project plan
```

---

#### Plan Generation Process

After scope confirmation:
1. Takes active task library (GF: 193 tasks / BF: 201 tasks)
2. Applies conditional activation rules — removes tasks not in scope
3. Expands module-specific tasks (Level 4) — one per confirmed module
4. Generates sub-tasks (Level 5) per module instance
5. Adds WRICEF tracking tasks (per confirmed count)
6. Adds integration tasks (per confirmed interface count)
7. Assigns durations from real project benchmarks (Section 3.7)
8. Sets dependencies between tasks and phases
9. Assigns workstream owners

**Outputs:**
- Complete 5-level project plan loaded into Octiss
- Phase-by-phase task list with module breakdown
- Exportable to MS Project and Excel
- Basis for all phase-driven navigation and agent CTAs

---

---

## 5. Change Request Workflow

### 5.1 Overview

Change Requests are detected and managed automatically across three agents:

| Agent | Role |
|---|---|
| RAID / Risk (10) | Detects scope change and flags it |
| Stakeholder Communication (8) | Drafts formal CR document and email |
| SAP Activate Governance (1) | Validates methodology and go-live impact |

### 5.2 Three CR Trigger Types

| Trigger | Example | Detection |
|---|---|---|
| WRICEF Count Exceeded | Item #16 added when SOW approves 15 | ✅ Auto — RAID fires when count exceeded |
| New Scope Item Added | New process outside agreed scope | ✅ Auto — RAID flags if outside SOW |
| Timeline or Budget Impact | New work pushes go-live date | ✅ Auto — RAID calculates downstream impact |

### 5.3 WRICEF Change Request Flow

```
SOW confirms: WRICEF approved count = 15
  ↓
Project Plan Agent creates 15 WRICEF tracking tasks
  ↓
During Explore — consultant identifies item #16
  ↓
RAID Agent auto-flags:
  "WRICEF count exceeded SOW-approved limit of 15.
   A Change Request is required."
  ↓
CTA: [Draft Change Request] [Log as Risk] [Notify Sponsor]
  ↓
Agent 8 drafts CR with: CR number, description, scope impact,
timeline impact (original vs revised go-live), cost impact,
decision required from customer sponsor
  ↓
PM reviews → approves → sends via Outlook
  ↓
Customer approves → count updated → new task added to plan
```

### 5.4 CR Document Contents (Auto-Generated by Agent 8)

| Field | Content |
|---|---|
| CR Number | Auto-generated (CR-001, CR-002 etc.) |
| Project | From project context |
| Date | Auto |
| Phase | Current active phase |
| Description | What is being added or changed |
| Scope Impact | What is being added to agreed scope |
| Timeline Impact | Original vs revised go-live date |
| Cost Impact | Additional effort days and estimated cost |
| Decision Required From | Customer sponsor name |
| Approval Status | Pending / Approved / Rejected |

### 5.5 Tasks with CR Trigger Flag (`triggers_cr = TRUE`)

- WRICEF List Finalisation (if count exceeded)
- Rationalization and Approval of WRICEF (if new items added)
- Manage Project Issues and Changes (always available)
- Sprint Planning (if new backlog items outside SOW)
- Any task where PM manually flags scope change

---

## 6. Escalation Workflow

### 6.1 Two Escalation Types

| Type | Direction | Example |
|---|---|---|
| Internal | PM → SI Firm hierarchy | Consultant not delivering BPML on time |
| External | PM → Customer hierarchy | Key user not attending workshops |

### 6.2 Four-Level Escalation Ladder

#### Internal (SI Firm)

| Level | From → To | Tone | Timeframe | Trigger |
|---|---|---|---|---|
| 1 | PM → Workstream Lead | Informal | Same day | Task overdue 1-2 days |
| 2 | PM → Delivery Manager | Formal written email | 24-48hrs after L1 | No response / task blocking |
| 3 | PM → Account Manager / Director | Very formal with impact | 48-72hrs after L2 | No response / go-live at risk |
| 4 | PM → Steering Committee | Executive escalation | Scheduled SteerCo | Contract risk / commercial impact |

#### External (Customer)

| Level | From → To | Tone | Timeframe | Trigger |
|---|---|---|---|---|
| 1 | PM → Customer Project Lead | Informal | Same day | Sign-off delayed / key user unavailable |
| 2 | PM → Customer Sponsor | Formal with timeline | 24-48hrs after L1 | No response / workshop cancelled |
| 3 | PM → Executive Sponsor | Very formal with financial impact | 48-72hrs after L2 | Go-live at risk / contract obligation not met |
| 4 | PM → Steering Committee | Executive to Executive | Scheduled or emergency SteerCo | Contract breach / programme failure risk |

### 6.3 Escalation Cycle — How Octiss Manages It

```
STEP 1 — RAID Agent detects trigger
  Task overdue / sign-off delayed / access not provided
  Flags with recommended escalation level

STEP 2 — PM selects: recipient, level, channel
  Email / WhatsApp / Formal letter

STEP 3 — Agent 8 drafts escalation
  Tone automatically adjusted by level:
  L1 → Informal, short, friendly nudge
  L2 → Formal, structured, timeline included
  L3 → Very formal, financial impact stated
  L4 → Executive brief, programme risk framed

STEP 4 — Auto-escalation cycle
  Response deadlines set automatically:
  L1 = 24 hours | L2 = 48 hours
  L3 = 72 hours | L4 = next SteerCo date
  
  If no response by deadline:
  RAID Agent recommends next level automatically
  PM confirms → Agent 8 drafts next level immediately

STEP 5 — Resolution logged
  PM marks resolved → RAID log updated → task status updated
```

### 6.4 Agent Responsibilities

| Agent | Role |
|---|---|
| RAID / Risk (10) | Detects trigger, monitors response window, recommends next level, auto-escalates if no response |
| Stakeholder Communication (8) | Drafts every escalation level, adjusts tone, sends via Outlook, drafts WhatsApp variant |
| SAP Activate Governance (1) | Validates go-live and contract impact at L3/L4, produces SteerCo impact statement |

### 6.5 New Database Table — project_escalations

```sql
project_escalations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id UUID REFERENCES projects(id),
  task_id UUID REFERENCES project_tasks(id),
  meeting_id UUID,
  escalation_type TEXT,        -- INTERNAL / EXTERNAL
  level INT,                   -- 1 / 2 / 3 / 4
  trigger_reason TEXT,
  recipient_name TEXT,
  recipient_role TEXT,
  channel TEXT,                -- EMAIL / WHATSAPP / LETTER
  raised_date TIMESTAMPTZ,
  response_deadline TIMESTAMPTZ,
  response_received BOOLEAN DEFAULT FALSE,
  response_date TIMESTAMPTZ,
  status TEXT,                 -- OPEN/RESPONDED/RESOLVED/ESCALATED_FURTHER
  next_level_due TIMESTAMPTZ,
  financial_impact TEXT,
  raid_id UUID,
  draft_content TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
)
```

---

## 7. Session Attendance and No-Show Escalation

### 7.1 The Problem

Key users and decision makers not attending mandatory workshops is one of the most common causes of SAP project delays. Octiss manages the full attendance lifecycle — before, during, and after every session.

### 7.2 Three Moments of Management

#### Before the Session — Confirmation Chase

```
T-5 days: Calendar invite sent via connected Outlook
  Octiss tracks: Accepted / Pending / Declined

T-3 days: Agent 9 checks confirmations
  If mandatory attendee unconfirmed →
  Agent 9 sends reminder / Agent 8 drafts WhatsApp nudge

T-1 day: If still unconfirmed →
  RAID Agent escalates to PM:
  "X mandatory attendees unconfirmed for tomorrow.
   Recommend WhatsApp nudge. If no response by 5PM
   → escalate to Customer Project Lead."
  [Draft WhatsApp] [Draft Escalation] [Reschedule]
```

#### During the Session — Attendance Recording

```
PM marks attendance at session start.
Agent 9 assesses completeness:

If mandatory attendee absent:
  "Cannot complete [topic] without [person].
   A) Proceed with partial scope — document gaps
   B) Postpone session — reschedule
   C) Proceed and follow up separately"
  [Proceed Partial] [Postpone] [Mark Incomplete]
```

#### After the Session — No-Show Escalation

```
RAID Agent auto-creates escalation entry:
  Issue: Mandatory attendee absent
  Session: [name and date]
  Person: [name and role]
  Impact: [downstream tasks blocked]
  
  Level 1 escalation drafted immediately:
  → Customer Project Lead (external)
  → Delivery Manager (internal)
  
  Response deadline: 48 hours
  If no response → Level 2 auto-recommended
```

### 7.3 Recurring No-Show Pattern Detection

```
RAID Agent tracks attendance across ALL sessions.
If same person misses 2+ sessions:

  "Pattern detected: [Name] absent from [N] sessions:
   - [Session 1] — [date]
   - [Session 2] — [date]
   
   Risk: [Workstream] stream at risk.
   
   Recommended: Formal escalation to Customer Sponsor
   requesting either:
   A) Guaranteed availability going forward
   B) Alternative key user nomination"
   
  [Draft Formal Escalation] [Log as Programme Risk]
  [Add to SteerCo Agenda]
```

### 7.4 No-Show Escalation Types

| Situation | Level | Escalation To |
|---|---|---|
| Key user absent from workshop | 1 | Customer Project Lead |
| Consultant absent from session | 1 | Delivery Manager |
| Decision maker absent from sign-off | 2 | Customer Sponsor |
| Recurring no-shows — same person | 2→3 | Customer Sponsor → Executive |
| SteerCo member absent | 3 | Programme Director |
| Multiple stream leads absent | 3 | Customer Sponsor + Account Director |

### 7.5 Agent Responsibilities

| Agent | Role |
|---|---|
| Meeting Intelligence (9) | Sends invites, tracks confirmations, T-3/T-1 reminders, records attendance, flags incomplete sessions |
| RAID / Risk (10) | Detects no-shows, tracks recurring patterns, auto-raises escalations, monitors responses |
| Stakeholder Communication (8) | Drafts all communications — nudges, formal escalations, pattern escalation letters |

### 7.6 Database Additions Required

#### Additions to project_meetings table

```sql
invited_attendees    JSONB,   -- list of invitees with roles
confirmed_attendees  JSONB,   -- who confirmed
actual_attendees     JSONB,   -- who attended
absent_attendees     JSONB,   -- who did not attend
absence_reason       TEXT,
follow_up_required   BOOLEAN DEFAULT FALSE,
session_complete     BOOLEAN DEFAULT FALSE,
escalation_raised    BOOLEAN DEFAULT FALSE
```

#### New table — session_attendance

```sql
session_attendance (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id UUID REFERENCES projects(id),
  meeting_id UUID,
  person_name TEXT NOT NULL,
  person_role TEXT,
  person_type TEXT,            -- CUSTOMER / SI_TEAM / SAP
  invited BOOLEAN DEFAULT TRUE,
  confirmed BOOLEAN DEFAULT FALSE,
  attended BOOLEAN DEFAULT FALSE,
  absence_reason TEXT,
  total_sessions_invited INT DEFAULT 0,
  total_sessions_attended INT DEFAULT 0,
  consecutive_absences INT DEFAULT 0,
  pattern_flagged BOOLEAN DEFAULT FALSE,
  escalation_id UUID REFERENCES project_escalations(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
)
```


## 8. Core UX Principles

### 5.1 The Three Rules

**Rule 1 — Phase Gates Everything**
The user sees only their current active phase. Phases unlock sequentially: Prepare → Explore → Realize → Deploy → Run. A phase is marked complete via a formal phase gate review. There is no skipping.

**Rule 2 — Tasks Drive the Screen**
Within each phase, tasks appear in sequence based on the project plan. Each task has a status: Not Started / In Progress / Complete. The screen shows only what is relevant to the active task.

**Rule 3 — Everything is a CTA, Nothing is a Menu**
Agents, documents, RAID logs, meeting tools, and workbench features do not live in the sidebar. They surface as Call-to-Action buttons on the relevant task screen, exactly when needed.

### 5.2 Sidebar — Final Structure

| Item | Decision | Reason |
|---|---|---|
| Octiss logo / branding | KEEP | Always visible |
| Active Project selector | KEEP | Context switcher — critical for multi-project users |
| SAP Activate Phase tracker | KEEP | Phase progress indicator only |
| Command Center | KEEP | PM daily home |
| Notifications | KEEP | Always relevant |
| Settings | KEEP | Account and workspace |
| Sign out | KEEP | Always visible |
| Agents | REMOVE | CTA on task screens only |
| Outbox | REMOVE | CTA when communication task active |
| RAID | REMOVE | CTA when risk task active |
| Reports | REMOVE | CTA on Command Center only |
| Voice History | REMOVE | Inline on relevant screens |
| More Tools / Workbench | **REMOVE ENTIRELY** | All tools surface as inline CTAs |
| Stakeholders | REMOVE | CTA on stakeholder tasks only |

### 5.3 First-Time Setup Gate

After first login, 5 setup steps must be completed before ANY navigation is available.

| # | Setup Step | What It Does |
|---|---|---|
| 1 | Workspace Profile | Set name, organisation, role |
| 2 | Document Storage | Connect OneDrive / SharePoint |
| 3 | Email Route | Configure email for review packs and reminders |
| 4 | Calendar Route | Connect Outlook / Google Calendar for invites |
| 5 | Automation Safety | Confirm Dry Run mode |

### 5.4 User Journey Flow

```
LOGIN
  ↓
FIRST-TIME SETUP (5/5 gate — blocks all navigation)
  ↓
CREATE / SELECT PROJECT
  → Select methodology (GF / BF / SF)
  → Select modules in scope
  → Enter project dates
  ↓
PROJECT PLAN AGENT FIRES
  → Generates full 5-level task plan automatically
  ↓
COMMAND CENTER
  → Active phase, current task, Daily PM Briefing (8AM)
  ↓
TASK SCREEN
  → Task description, acceptance criteria, status
  → CTA buttons surface contextually:
    [Open Agent] [Log Risk] [Generate Document] 
    [Schedule Session] (only what is relevant)
  ↓
COMPLETE TASK → next task unlocks
  ↓
COMPLETE ALL TASKS IN PHASE → Phase Gate Review
  ↓
PHASE GATE APPROVED → next phase unlocks
```

---

## 9. TRACK Task Behaviour

### 9.1 What a TRACK Task Is
Technical activities executed by Basis, IT, or SAP — not by the PM. PM is accountable for ensuring they complete on time. Delays block the project.

### 9.2 How TRACK Tasks Work in Octiss
- Appear in project plan assigned to the relevant technical workstream (e.g., Basis)
- PM sees status of all TRACK tasks on Command Center as dependencies
- PM sets expected completion date and responsible person
- **If a TRACK task goes overdue → RAID / Risk Agent automatically creates a dependency risk**
- PM receives notification and Daily Briefing flags the blocker
- PM can raise formal escalation via Stakeholder Communication Agent from the TRACK task screen

### 9.3 Why This Matters
Delayed system access is one of the top 5 causes of SAP project delays. TRACK tasks with auto-RAID escalation give PMs a proactive risk management tool no other SAP PM platform provides.

---

## 10. Pricing & Tier Structure

| Tier | Monthly | Annual | Target User | Project Limit |
|---|---|---|---|---|
| Project | $79 | $854 (save 10%) | Single PM, one project | 1 active project |
| Program | $199 | $2,149 (save 10%) | PM managing multiple projects | Up to 5 projects |
| Portfolio | $599 | $6,469 (save 10%) | PMO lead / portfolio manager | Up to 20 projects |

- **Billing provider:** Paddle (pending company registration in Pakistan)
- **No free trial. No money-back guarantee.**
- **Beta testers:** Portfolio tier at no charge

---

## 11. What Is Excluded from Octiss from Octiss

### 11.1 Discover Phase
Excluded entirely. See Section 2.2.

### 11.2 SAP Task Categories Excluded Excluded
- SAP system provisioning tasks — handled by SAP / Basis
- SAP tool self-enablement tasks — individual team member responsibility
- SAP Employee-only tasks — not applicable to customer PM
- SAP Partner-only tasks — SI firm internal
- Pre-implementation strategy tasks — Discover phase content

### 11.3 Upgrade Methodology
The SAP Activate Upgrade methodology was downloaded during task library extraction but is excluded from Octiss v1.0. May be added in a future version.

### 11.4 Digital Twin / Multilingual Features / Multilingual Features
SOW v1.5 vision — locked until after M11 commercial launch. No build until SOW v1.6 written. Multilingual support (EN, UR, AR, HI, TA, TE, ES, FR, DE) is post-M11.

---

## 12. Decisions Log

> Add every architectural decision here immediately when agreed. Never let a decision get lost in a session.

| Date | Decision |
|---|---|
| 05 Jun 2026 | Project Initiation is now the required bridge between project creation and Prepare. New projects redirect to `/projects/{id}/initiation`; PM can upload/confirm SOW scope, maintain the project team roster, and map task ownership before Prepare begins. Incomplete projects show a Command Center warning. |
| 05 Jun 2026 | Task Screen saves are separated by intent: PM notes append to a timestamped log, dates save through explicit Save Dates, assignment/track owner saves separately, and status autosave no longer wipes unsaved note/date/owner edits during refetch. Dates display as `DD-MMM-YYYY` on the new task/phase/command-center surfaces. |
| 05 Jun 2026 | Agent context must include task name, phase, deliverable, methodology, project SOW scope, modules, team roster, and SAP knowledge. If filtered vector search returns no SAP chunks, retrieval falls back to layer scans so PM-facing agent output does not claim zero SAP context. |
| 05 Jun 2026 | Production GF/BF test projects were re-initialized with generic SOW scope and team data only. GF has 9 team rows, 1 SOW seed, 7 modules, and 18/18 TRACK rows with owners. BF has 9 team rows, 1 SOW seed, 8 modules, and 13/13 TRACK rows with owners. |
| 05 Jun 2026 | Active-project context is now centralized for Command Center and top-level project workspaces. AuthContext preserves the last selected project across logout/login, `useActiveProject` resolves against the projects API, and the Layout project-scope gate prevents blank project-scoped pages by restoring a valid project or showing a selector. Settings remains intentionally ungated for account, setup, integration, and access-management flows. |
| 05 Jun 2026 | Phase View active-project resolution fixed. The project selector stores selected project IDs in shared AuthContext/localStorage; Phase View now uses a shared resolver from the projects API and auto-restores only on routes that require an active project, with project-list loading prioritized before phase detail calls. Production `/api/v1/projects` for tester1 returns BF Test Project and GF Test Project, so GF is linked to the tester1 organization. |
| 05 Jun 2026 | MS365 fully connected in production. OneDrive, Outlook, Calendar all configured. SharePoint unavailable — personal Hotmail account has no SPO license. Decision: SharePoint is not required for beta. Personal OneDrive is sufficient for document storage. Beta is now unblocked. |
| 05 Jun 2026 | Microsoft OAuth URL generation must keep `offline_access` as a required/display permission but must not pass it directly to MSAL runtime request scopes. MSAL treats it as reserved and returns a backend 500, which browsers surface as Network Error. Backend commit `8f1ecfb` fixed this and added `/api/v1/integrations/microsoft/auth` as a compatibility alias for `/connect`. |
| 05 Jun 2026 | MS365 Railway env vars added — production Microsoft integration now configured. Tester workspace API reports `configured=true`, `connected=false`, `verified=false`; OAuth connect/verification remains the next step. |
| 05 Jun 2026 | Earlier production verification corrected the Microsoft state: UI/settings text is not sufficient proof. The source of truth is `/api/v1/integrations/microsoft/status`; at that time production API reported `configured=false`, `connected=false`, `verified=false`. |
| 05 Jun 2026 | BF Conversion Module is production verified. Migration was applied through the production pooler, `service_role` grants were added to `bf_sum_checklists` and `bf_sum_executions`, and GF/BF production screens were tested against dedicated production test projects. |
| 05 Jun 2026 | Railway CLI v5.0.0 fully authenticated and linked to octiss-production / sap-pmo-agent. Auth via interactive `railway login` on local Windows Terminal - inherited by Codex automatically. Link requires flag form with project/env/service IDs because positional ID form is not supported in v5. Link command and all IDs documented in Handoff Section 20. All Railway operations now automated: logs, status, env vars, redeploy. Final CLI gap closed. |
| 05 Jun 2026 | CLI environment partially automated. Supabase CLI linked to production project qrxfprybbpqugptakeke; `supabase db push --dry-run` reaches the remote database and reports it up to date, but current migration filenames are skipped because they do not match Supabase's `<timestamp>_name.sql` pattern. jq installed at 1.8.1 and JSON parsing test passed. Railway CLI login is blocked in the Codex non-interactive shell until `RAILWAY_API_TOKEN` / `RAILWAY_TOKEN` or interactive `railway login` is provided. |
| 04 Jun 2026 | M10b-6c BF Conversion Module code complete. Backend commit 44d00df adds BF endpoints, `bf_sum_checklists`, `bf_sum_executions`, downtime estimate fields, migration SQL, and route tests. Frontend commit a5e04cc adds BF screen, Task Screen CTAs, SUM tracker, downtime calculator, and cutover comms pack CTA. This was superseded by the 05 Jun 2026 production migration/grant verification. Backend: 1294 tests passing. Frontend: 103 Playwright tests passing / 48 skipped. |
| 04 Jun 2026 | Task Screen and Phase View grouping fixed. Playwright fixtures updated to real production UUIDs. All 61 Project Alpha rows link to legacy roadmap_tasks, which is correct by design. New projects via Project Plan Agent will link to activate_tasks. Commits: 82f09d5 (FE), f0c8b88 (BE), 3f6718f (FE). |
| 04 Jun 2026 | Public docs repo created: github.com/mohsinsardar-ai/octiss-docs. Both SUD v1.6 and Master Handoff v1.0 now accessible as raw URLs for all Claude and Codex sessions. Commit: 7e03eee. |
| 04 Jun 2026 | Permanent rule added: every Codex session must end with a doc update to both Handoff and SUD, pushed to both repos. This keeps session continuity current and prevents repeated debugging of already-fixed issues. |
| 01 Jun 2026 | M10b-5 complete. All 394 active tasks mapped to 12 agents. GF: 9 agents. BF: 10 agents including Agent 11 (19 BF-specific tasks). Zero unmapped. File: Octiss_M10b5_Agent_Mapping_v1.0.xlsx. |
| 01 Jun 2026 | Key mapping rule: Defining/creating a deliverable = Phase Agent. Getting sign-off/communicating it = Agent 8 (Stakeholder Communication). Applies across all phases. |
| 01 Jun 2026 | Transport Request management: PM owns TR template and records (Agent 5). Basis executes transport movement (TRACK — Agent 10). |
| 01 Jun 2026 | Change Request workflow: RAID Agent detects, Agent 8 drafts CR, Agent 1 validates impact. Three trigger types: WRICEF count exceeded, new scope item, timeline/budget impact. |
| 01 Jun 2026 | Escalation workflow: 4-level ladder internal and external. Response deadlines L1=24hrs, L2=48hrs, L3=72hrs, L4=next SteerCo. Auto-cycle managed by RAID Agent. New table: project_escalations. |
| 01 Jun 2026 | Session attendance and no-show escalation: T-5/T-3/T-1 cycle, attendance recording, post-session escalation. Recurring pattern at 2+ absences triggers programme escalation. New table: session_attendance. |
| 01 Jun 2026 | Custom Code Analysis (BF): Agent 11 primary + Agent 10 CTA for timeline + Agent 8 CTA for escalation. |
| 04 Jun 2026 | M10b-8 complete. Command Center rebuilt to 6-section clean layout. Task Screen with CTA rendering built. Agent Chat Panel with context injection built. Backend agent chat endpoint added. Frontend: baeab4b. Backend: 321c765. |
| 04 Jun 2026 | Azure App metadata was prepared for Octiss Production. Later API verification on 05 Jun 2026 showed Railway MS365 env/OAuth is still not configured/connected/verified in production. |
| 04 Jun 2026 | Setup 5/5 READY confirmed in production after safe settings configuration. Microsoft connection itself remains blocked until the status API reports configured/connected/verified true. |
| 04 Jun 2026 | Task names fix: project_tasks for Project Alpha use legacy roadmap_task_id. Backend now enriches /api/v1/project-plan/tasks with real task names from activate_tasks or roadmap_tasks. Commits: 6f88948 (backend) + b41176d (frontend). |
| 04 Jun 2026 | Reconnect Microsoft button replaced with non-clickable "Microsoft Connected" badge when connected and verified. Run Microsoft Verification remains visible. Commit: 2adc40d. |
| 04 Jun 2026 | Sidebar phase tracker fixed to route to `/phase/{phaseCode}` and Daily Briefing fixed to use only Agent 12 daily_briefings table output. Commit: 16794ab. |
| 04 Jun 2026 | Beta testers remain on hold until Microsoft 365 production verification, User Manual completion, and Welcome Pack refresh are complete. |

| 03 Jun 2026 | M10b-6a complete — project_escalations, session_attendance, project_meetings additions live in production. Commit: 29f66c49. |
| 03 Jun 2026 | M10b-6b complete — Project Plan Agent built. 5 endpoints live. 1285 tests passing. Commit: c51f8f8. |
| 03 Jun 2026 | Seed complete — 484 tasks loaded to production Supabase. GF: 193+54. BF: 201+36. 160 deliverables, 5 phases. Zero duplicates. 03 Jun 2026. |
| 03 Jun 2026 | M10b-7 complete — Sidebar rebuilt. 17 items removed. 7 items kept. 141 Playwright tests passing. Commit: 828cee7. |
| 03 Jun 2026 | Production migration process: migrations committed to repo, applied manually via Supabase SQL Editor by Mohsin. Standard process for all future migrations. |
| 03 Jun 2026 | Credential management: .env in backend repo root. Loaded via python-dotenv with override=True and UTF-8 BOM handling. Source: production_credentials.txt in Octiss Production folder. |
| 03 Jun 2026 | Route count corrected: 365 routes confirmed (not 375). |
| 03 Jun 2026 | M10b-6c BF Conversion Module design confirmed. Source: real project checklists not SAP Activate library. Checklist-based approach confirmed as superior for BF. |
| 03 Jun 2026 | Downtime Calculator confirmed as core M10b-6c feature. Inputs: DB size, Z-object count, QAS SUM actual duration, hardware. Output: PRD estimate, cutover window, rollback matrix. |
| 03 Jun 2026 | Pre-SUM / SUM / Post-SUM checklist structure confirmed. Four environments: Sandbox, DEV, QAS, PRD. PRD has formal Go/No-Go gate before execution. |
| 03 Jun 2026 | BF additional setup questions: conversion cycles count, Sandbox availability, SUM window type, Z-object count, DB size in GB. |
| 03 Jun 2026 | BF conversion architecture: SAP Activate = governance skeleton (Layer 1). Real project checklists = execution detail (Layer 2). Both layers required. |
| 01 Jun 2026 | Discover phase excluded from Octiss scope. Octiss starts at Prepare. |
| 01 Jun 2026 | Bluefield (Selective Data Transition) added as 3rd methodology alongside GF and BF. |
| 01 Jun 2026 | Task architecture confirmed as 5-level: Phase → Deliverable Group → Methodology Task → Module Instance → Sub-Task. Levels 4-5 generated dynamically by Project Plan Agent on project creation. |
| 01 Jun 2026 | Project Plan Agent added as Agent #2. Octiss now has 12 specialist agents, not 11. Project Plan Agent is the most critical agent — all others reference the plan it generates. |
| 01 Jun 2026 | 4-category task classification confirmed: KEEP (PM executes), TRACK (Basis/IT executes, PM monitors), MERGE (too granular), EXCLUDE (SAP internal). |
| 01 Jun 2026 | TRACK tasks auto-trigger RAID / Risk Agent when overdue. PM receives notification and Daily Briefing flags the blocker. |
| 01 Jun 2026 | Sidebar reduced to: Logo, Active Project selector, Phase tracker, Command Center, Notifications, Settings, Sign out. All tools and agents surface as CTAs on task screens only. More Tools / Workbench removed entirely. |
| 01 Jun 2026 | First-Time Setup (5/5 gate) blocks all navigation until complete. |
| 01 Jun 2026 | SAP Activate task library sourced from official SAP Roadmap Viewer (me.sap.com/roadmapviewer), April 2026. GF: 312 tasks, BF: 292 tasks, SF: 299 tasks (all excluding Discover). |
| 01 Jun 2026 | Upgrade methodology excluded from Octiss v1.0 scope. |
| 01 Jun 2026 | Module expansion logic confirmed: FTS Workshops, Validation Workshops, BRD, WRICEF Lists, Configuration, FUT, SIT, UAT, TTT, Show & Tell, Data Templates, Transport Execution, End User Training, Cutover Activities — all expand per module. |
| 01 Jun 2026 | Beta testers on hold — do not send credentials until minimum viable beta flow is working: Setup → Create Project → Phase View → Task Screen → Agent CTA. |
| 01 Jun 2026 | Real SAP project plans (7 files) analysed for module expansion patterns and duration benchmarks. All findings captured in SUD v1.1 Section 3. |
| 01 Jun 2026 | Scheduling confirmed as part of the product. 14 task types get Schedule Session CTA. Meeting Intelligence Agent fires on click. Calendar send via connected Outlook through Stakeholder Communication Agent. |
| 01 Jun 2026 | Standard module library confirmed: 18 modules. ABAP, Basis, Integration/BTP, Security/Auth are always TRACK. |
| 01 Jun 2026 | GF phase duration benchmarks: Prepare 20-30d, Explore 90-130d, Realize 150-210d, Deploy 25-40d, Run 20-70d. Total typical GF = 300-400 working days. Project Plan Agent uses these as defaults. |
| 01 Jun 2026 | M10b-2 complete. Final verified task library locked. GF 193 active tasks (175 KEEP + 18 TRACK), BF 201 active tasks (186 KEEP + 15 TRACK). 54 GF merge consolidations, 36 BF merge consolidations. File: Octiss_Final_Task_Library_v1.0.xlsx. |
| 01 Jun 2026 | SOW / Foundation Document upload is mandatory at project creation. Accepted documents: SOW/Contract (priority 1), Digital Discovery Assessment/DDA (priority 2), Handover Document (priority 3), Internal KO Document (priority 4). Document upload is Path A. Q&A fallback is Path B. |
| 01 Jun 2026 | SOW extraction uses free-text extraction via Claude Opus. SOW scope is written in narrative paragraphs or numbered lists — not structured tables. Agent reads full document and extracts scope intelligently. |
| 01 Jun 2026 | WRICEF is count-based from SOW. Exceeding SOW-approved count triggers auto RAID flag. Stakeholder Communication Agent auto-drafts Change Request. PM reviews, approves, and sends to customer. On approval, count updated and new task added to plan. |
| 01 Jun 2026 | ECT/Tricentis excluded by default — comes bundled with SAP Cloud ALM but rarely used by PMs. PM can activate manually if needed. SAP Cloud ALM setup tasks excluded entirely — Octiss is the PM tool. |
| 01 Jun 2026 | Conditional task activation confirmed — SAC, WalkMe, SAP Build, Joule, BTP integrations, data migration, Group Reporting all activate only when confirmed in SOW or answered Yes in Q&A flow. |

| 01 Jun 2026 | SF (Bluefield / Selective Data Transition) deferred from Octiss v1.0 scope. Current scope is GF and BF only. SF task library extracted and available but not loaded into DB. SF to be activated in a future version post-M11 commercial launch. |
| 01 Jun 2026 | SUD moved to GitHub (em-intelligence-labs-dashboard/docs/Octiss_SUD_v1.1.md) as the permanent reference. Every Claude and Codex session must read this file at session start. |

---

## 13. Current Build Status

### 13.1 What Is Live
- Backend: FastAPI on Railway — **399 routes**. Local compile and direct project-task route tests passed; previous full backend pytest baseline remains **1302 tests passing**.
- Frontend: React/Vite on Vercel — octiss-production.vercel.app — LIVE
- Database: Supabase Singapore (Pro) — all tables created, grants applied
- 12 agents built and live — **Agent #2 Project Plan Agent live**
- WhatsApp interface live
- n8n Daily PM Briefing trigger at 8AM — live
- 5 beta tester accounts created (tester1-5@octiss.com) — Portfolio tier
- **Production test projects verified**: GF Test Project `b82f0d75-bdc6-41ea-8387-1f48ff7d5afd` with 244 Activate-backed tasks; BF Test Project `e9cec48e-01f4-4506-a212-a9037ed76db9` with 235 Activate-backed tasks
- **Production test projects re-initialized**: GF has 9 generic team rows, 1 SOW seed, 7 modules, and 18/18 TRACK rows with owners. BF has 9 generic team rows, 1 SOW seed, 8 modules, and 13/13 TRACK rows with owners.
- Legacy demo `Sample Project Alpha` is not the production verification baseline
- **Sidebar rebuilt** — phase tracker, project selector, setup gate live
- **Setup 5/5 READY** — all setup steps complete for the production tester workspace after safe settings configuration
- **Microsoft 365 production integration live** — connected=true; OneDrive, Outlook, Calendar configured; Project Document Folders connected; dry-run active; SharePoint N/A because the personal Hotmail account has no SPO license
- **Command Center rebuilt** — 6-section clean layout (commit a39e52b + 1545871)
- **Command Center active project restore fixed** — `/dashboard` uses the shared active-project resolver, restores the last active project after fresh login, syncs route project ids before project-scoped pages render, switches GF/BF correctly, and shows a prominent selector when no project was remembered (commits 9050455 + c214c01)
- **Task Screen fixed and production-verified** — Agent Assist CTA rendering, agent panel, independent note/date/assignment saves, timestamped PM note log, status updates, direct task route loading, stable detail save/readback, dirty-field preservation, and text assignee labels
- **Phase View grouping fixed** — tasks group by deliverable_name with General fallback (commit 82f09d5)
- **Phase View active project restore fixed** — `/phase/{phase}` resolves active/fallback project from the shared project list, project-list loading is prioritized before phase detail calls, and the sidebar selector lists both GF Test Project and BF Test Project (commits 5ccd18f + ef425b5)
- **Playwright fixtures updated** — real production project_tasks UUIDs used in task route tests (commit 3f6718f)
- **11 M10b/Project Initiation DB tables live in production**: activate_phases, activate_deliverables, activate_tasks, project_tasks, project_sow_extractions, project_wricef, project_escalations, session_attendance, bf_sum_checklists, bf_sum_executions, project_team_members
- **Project Initiation module built** — backend commit fb67edf, frontend commit c214c01. New projects redirect to `/projects/{id}/initiation`; SOW upload/confirmation, team roster CRUD, task ownership mapping, and incomplete setup warning are live in code.
- **BF Conversion Module production-verified** — backend commit 44d00df, frontend commit a5e04cc, grants commit a36553e. QAS BF checklist, SUM tracker, downtime calculator, and Agent 8 cutover CTA path verified.

### 13.2 Known Issues (Active — Being Fixed)

| # | Issue | Priority | Status |
|---|---|---|---|
| 1 | Sidebar phase tracker links to old `/projects/{id}/prepare` route instead of new `/phase/prepare` | ✅ Fixed | 16794ab |
| 2 | Daily Briefing section pulls from old Project Alpha risks/actions data instead of Agent 12 daily_briefings table | ✅ Fixed | 16794ab |
| 3 | "Open Task" CTA on Command Center may route to old project workspace | ✅ Fixed | 82f09d5 |
| 4 | BF Conversion Module production migration/grants | ✅ Fixed | Applied through production pooler; `service_role` grants committed in a36553e |
| 5 | Phase View shows "Select a project" even while sidebar visually has project context | ✅ Fixed | 5ccd18f + ef425b5 |
| 6 | Command Center shows "Choose an active project" even while sidebar visually has project context | ✅ Fixed | 9050455 + c214c01 |
| 7 | Task notes/dates/assignment saves collide or get wiped by status refetch | ✅ Fixed | c214c01 + fb67edf |
| 8 | Agent responses can lack SAP/project context | ✅ Fixed | fb67edf |

### 13.3 What Is Not Yet Built
- Billing / Paddle integration (post company registration)
- Premium UI/UX overhaul
- User Manual (pending Premium UI/UX overhaul)
- Welcome Packs update with User Manual
- Beta tester onboarding (next after Welcome Packs)

### 13.4 Active Module — M10b — M10b: SAP Activate Workflow Engine

| Step | Work | Owner | Status | Commit |
|---|---|---|---|---|
| M10b-1 | Verify GF task library against SAP portal | Mohsin | ✅ Complete | SAP portal |
| M10b-2 | Task review — KEEP / TRACK / MERGE / EXCLUDE | Mohsin + Claude | ✅ Complete | GF 193 + BF 201 |
| M10b-3 | Module expansion patterns from real project plans | Mohsin + Claude | ✅ Complete | 7 plans analysed |
| M10b-4 | DB schema — 5-level hierarchy + agent_id + flags | Codex | ✅ Complete | `53bad706` |
| M10b-5 | Map all 394 tasks to 12 agents | Mohsin + Claude | ✅ Complete | File saved |
| M10b-6a | project_escalations + session_attendance tables | Codex | ✅ Complete | `29f66c49` |
| M10b-6b | Project Plan Agent + 5 endpoints + seed script | Codex | ✅ Complete | `c51f8f8` |
| Seed | Load 484 tasks to production Supabase | Mohsin | ✅ Complete | 03 Jun 2026 |
| M10b-7 | Rebuild sidebar as phase-progress tracker | Codex | ✅ Complete | `828cee7` |
| M10b-8 | Task screen + Command Center rebuild | Codex | ✅ Complete | `baeab4b` + `321c765` |
| M10b-6c | BF Conversion Module | Mohsin + Codex | ✅ Production verified | `44d00df` + `a5e04cc` + `a36553e` |

---

## 14. GitHub Repositories

| Repo | URL | Purpose |
|---|---|---|
| Backend | github.com/mohsinsardar-ai/sap-pmo-agent | FastAPI backend, agents, database migrations |
| Frontend | github.com/mohsinsardar-ai/em-intelligence-labs-dashboard | React/Vite dashboard, this docs folder |

---

## 15. Production Environment

| Service | URL / ID | Status |
|---|---|---|
| Backend (Railway) | sap-pmo-agent-production-3f52.up.railway.app | HEALTHY |
| Frontend (Vercel) | octiss-production.vercel.app | LIVE |
| Database (Supabase) | qrxfprybbpqugptakeke — Singapore Pro | ACTIVE |

---

---

## 16. BF Conversion Module — M10b-6c (Production Verified)

### 16.1 Overview

The standard SAP Activate BF library covers system conversion at a high level. Real Brownfield projects run Pre-SUM / SUM / Post-SUM cycles across multiple environments. The BF Conversion Module uses real project checklists as the primary source — not SAP documentation.

**Source:** anonymised Brownfield Wave 1 system conversion project plan
**Approach:** Conversion checklist-based, not project plan task-based

### 16.2 Architecture

```
Layer 1 — SAP Activate BF tasks (governance skeleton)
  Phase gates, quality gates, sign-offs, deliverables

Layer 2 — Real BF conversion cycles (muscle)
  Pre-SUM / SUM / Post-SUM per environment
  Based on real project checklists, not SAP docs
```

### 16.3 Conversion Cycle Structure

| Environment | Cycle | When |
|---|---|---|
| Sandbox | Pre-SUM → SUM → Post-SUM | Early Explore |
| DEV | Pre-SUM → SUM → Post-SUM | Realize Sprint 1 |
| QAS | Pre-SUM → SUM → Post-SUM | Realize Sprint 2 |
| PRD | Pre-SUM → SUM → Post-SUM | Deploy (Cutover) |

### 16.4 Five Components

**Component 1 — Pre-SUM Checklist (per environment)**
Complete checklist before each SUM execution:
- RFC connections verified
- Background jobs suspended
- Batch input sessions cleared
- Spool and update requests cleared
- SM21 system log reviewed
- SPAM/SAINT queue checked
- Kernel version verified
- Database backup completed
- Disk space verified (minimum 40% free)
- SUM downloaded and extracted
- SAPup started in shadow phase
- Formal Go/No-Go gate before execution

**Component 2 — SUM Execution Tracker**
Real-time tracking during SUM window:
- Phase tracker: PREP → SHADOW → SWITCH → CLEANUP
- Time elapsed vs estimated
- Go/No-Go checkpoint at each SUM phase
- Rollback trigger if timeline exceeded

**Component 3 — Post-SUM Validation Checklist**
Items to verify after SUM completes:
- System health checks
- Custom code functionality
- Integration connectivity
- User access verification
- Business process smoke tests per module

**Component 4 — Downtime Calculator**

| Input | Source |
|---|---|
| Database size (GB) | Basis team |
| Z-object count | Custom code scan results |
| QAS SUM actual duration | Recorded during QAS cycle |
| Hardware specs (CPU/RAM) | Basis team |

```
Estimation formula:
PRD downtime = QAS SUM duration × 1.3 to 1.5
(PRD is always larger than QAS)

Output:
- Estimated PRD downtime in hours
- Recommended cutover window (date/time)
- Minimum window required including validation
- Rollback decision point (time threshold)
```

**Component 5 — Cutover Communication Pack (Agent 8)**
- Business notification of downtime window
- IT team checklist distribution email
- Go-Live announcement template
- Rollback notification template (if needed)

### 16.5 Additional BF Project Setup Questions

When PM selects BF methodology, Project Plan Agent asks:

| # | Question | Input |
|---|---|---|
| 1 | How many conversion cycles planned? | Number (standard = 4: Sandbox, DEV, QAS, PRD) |
| 2 | Is a Sandbox system available? | Y / N |
| 3 | What is the planned SUM execution window? | Weekend / Public holiday / Planned downtime |
| 4 | Approximate Z-object count | Number (from custom code scan) |
| 5 | Database size (GB) | Number (from Basis team) |

### 16.6 Implementation Status

**Backend commit:** 44d00df  
**Frontend commit:** a5e04cc  
**Production grant commit:** a36553e
**Migration SQL:** `supabase/migrations/m10b6c_bf_conversion_module.sql`

Backend delivered:
- `bf_sum_checklists` and `bf_sum_executions` table migration SQL
- `projects.bf_downtime_estimate` and `projects.bf_downtime_estimate_updated_at`
- BF checklist endpoints for read/update
- SUM execution endpoints for start/phase/rollback
- Downtime calculator endpoint with optional Save Estimate
- Route tests passing

Frontend delivered:
- `BFChecklistScreen.jsx`
- Task Screen CTAs for Pre-SUM Checklist, SUM Tracker, Post-SUM Checklist, Downtime Calculator, and Cutover Communication Pack
- Cutover Communication Pack opens Agent Chat Panel with Agent 8 context
- Playwright coverage for checklist, SUM tracker, downtime calculator, and cutover CTA

Production status:
- Code is pushed to `main`
- Production migration was applied through the production pooler
- `bf_sum_checklists` and `bf_sum_executions` have `service_role` grants in production and in source SQL
- BF task environment inference opens QAS conversion path from the production-like conversion task
- Pre-SUM checklist renders and accepts PM review updates
- SUM tracker starts and shows an active execution
- Downtime calculator production result for 420GB / 850 Z-objects / 300 QAS mins / 8 CPU / 64GB RAM: 11.73 hours estimated, 14.7-hour minimum window, 423-minute rollback point, HIGH confidence
- Microsoft 365 is no longer a beta blocker: connected=true, OneDrive/Outlook/Calendar configured, Project Document Folders connected, SharePoint N/A for personal Hotmail account

*End of Document — Octiss SUD v1.6.7 — 05 June 2026*
*Next update: v1.7 after Premium UI/UX overhaul and User Manual completion*
