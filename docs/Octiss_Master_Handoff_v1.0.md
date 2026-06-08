# Octiss — Master Handoff Document
**Version:** v1.1 | **Date:** 08 June 2026
**Purpose:** Single reference for any new Claude or Codex session

---

## CRITICAL RULES — READ FIRST

```
1. NEVER mention real client or project names.
   Use only: "Project Alpha", "SI Firm",
   "PMgr", "Customer", "Consultant"

2. Product name: Octiss
   Company name: EM Intelligence Labs
   Owner: Mohsin Sardar

3. Single-user principle:
   PM is the ONLY user who logs into Octiss.
   Consultants and customers do NOT log in.
   All outputs are PM-review-only drafts
   until PM approves and sends.

4. Dry-run safety:
   Automation stays in dry-run mode until
   PM explicitly approves live sending.
   Nothing is sent externally without PM approval.
```

---

## 1. What Is Octiss

Octiss is an AI-powered SAP PMO Intelligence Platform. It guides SAP Project Managers through the SAP Activate methodology — Prepare → Explore → Realize → Deploy → Run — with 12 specialist AI agents that surface as contextual CTAs on task screens.

**NOT a dashboard. NOT a tool suite. A guided SAP Activate journey.**

The PM works through tasks in sequence. Each task has a task screen. The task screen shows what needs to be done and surfaces relevant agent CTAs only when needed.

---

## 2. Live URLs

| Item | URL / Path |
|---|---|
| Frontend | https://octiss-production.vercel.app |
| Backend | https://sap-pmo-agent-production.up.railway.app |
| Frontend Repo | mohsinsardar-ai/em-intelligence-labs-dashboard |
| Backend Repo | mohsinsardar-ai/sap-pmo-agent |
| Database | Supabase Singapore — qrxfprybbpqugptakeke |
| SUD v1.6 | docs/Octiss_SUD_v1.6.md (frontend repo) |
| Public Docs | mohsinsardar-ai/octiss-docs |
| Production Credentials | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\production_credentials.txt |

---

## 3. Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React/Vite, TanStack Query, Tailwind, Vercel |
| Backend | FastAPI, Python, Railway |
| Database | Supabase (PostgreSQL + pgvector) |
| Auth | Supabase Auth + Google/Azure SSO (implemented Jun 2026) |
| AI Models | Anthropic Claude (Opus 4 for complex, Sonnet for standard) |
| Embeddings | Hugging Face BAAI/bge-m3 |
| MS365 | Microsoft Graph API (MSAL) |
| Automation | n8n (Railway) |
| Voice | Web Speech API — EN/AR/UR live |
| Billing | Paddle (3 tiers: Project/Program/Portfolio) |

---

## 4. Current Build State (08 June 2026)

### What Is Live and Working
- Backend: 386 routes, 1235 tests passing (post-cleanup baseline)
- Frontend: Deployed Vercel, build passing
- Setup 5/5 READY — all setup steps complete
- MS365 fully connected — OneDrive, Outlook, Calendar verified
- Azure App: "Octiss Production" — 7 permissions granted
- 484 tasks seeded in production (GF 193+54, BF 201+36)
- Google SSO + Azure SSO: implemented and token exchange complete
- Voice Copilot: EN/AR/UR live
- Multi-language: English + Arabic RTL
- n8n: 6 workflows published
- All migrations applied
- Layer 1 codebase cleanup complete (07 Jun 2026)

### 10 Critical Issues — Status After Fix Session (07-08 Jun 2026)

| # | Issue | Status | Commit |
|---|---|---|---|
| 1 | Post-login routing wrong | ✅ Fixed | 73d8a40 |
| 2 | Navigation broken — Prepare → wrong page | ✅ Fixed | 9a5d721 |
| 3 | Phase status contradiction (100% vs NOT STARTED) | ✅ Fixed | 724b42e / 7568372 |
| 4 | SSO buttons say "Coming Soon" | ✅ Fixed | c32bcb0 / 2093e7c |
| 5 | SteerCo draft invisible | ✅ Fixed | 978dbf8 |
| 6 | n8n shows REQUIRES SETUP | ✅ Fixed | a605d52 / da952c5 |
| 7 | Settings not left sidebar structure | ✅ Fixed | 10b1bc6 |
| 8 | Communication CTA errors | ✅ Fixed | 8bc3c74 / cc81401 |
| 9 | Agent responses — walls of text | ⏳ Prompt ready — Codex credits reset Jun 11 |
| 10 | Task descriptions generic | ⏳ Audit needed — Codex credits reset Jun 11 |

---

## 5. Milestone History

| Milestone | What Was Built |
|---|---|
| M1 | Project Knowledge RAG Foundation |
| M2 | Automatic ingestion — generated/internal artifacts |
| M3 | Ingestion expansion — meetings, actions, RAID, reports |
| M4 | Microsoft Graph / OneDrive / SharePoint ingestion |
| M5 | PMO intelligence layer |
| M6 | Meeting intelligence — transcription, MOM extraction |
| M7 | Multi-agent orchestration |
| M8D | SAP Knowledge Base |
| M9 | 11 specialist agents live |
| M10 | Enterprise hardening, billing, voice copilot, Octiss rebrand |
| M10b | SAP Activate Workflow Engine — task library, agents, sidebar, Command Center, task screen |
| Cleanup | Layer 1 (Project Tally era) artifacts removed — 07 Jun 2026 |
| 10-Fix | 8/10 critical owner-review issues fixed — 07-08 Jun 2026 |

---

## 6. The 12 Specialist Agents

| # | Agent | Model | Primary Role |
|---|---|---|---|
| 1 | SAP Activate Governance | Opus | Phase gates, quality gates, methodology compliance |
| 2 | Project Plan Agent | Opus | SOW extraction, conditional task activation, plan generation |
| 3 | Prepare Agent | Sonnet | Prepare phase task guidance |
| 4 | Explore Agent | Opus | FTS workshops, BPML, gap analysis, WRICEF |
| 5 | Realize Agent | Sonnet | Configuration, testing, TR management |
| 6 | Deploy Agent | Opus | Cutover, go-live readiness |
| 7 | Run/Hypercare Agent | Sonnet | Hypercare, issue triage, closure |
| 8 | Stakeholder Communication | Sonnet | Sign-offs, escalations, CRs, formal comms |
| 9 | Meeting Intelligence | Sonnet | Workshop scheduling, MOM, attendance |
| 10 | RAID/Risk Agent | Sonnet | Risks, issues, TRACK overdue, escalation detection |
| 11 | Brownfield Conversion | Opus | BF conversion, custom code, SUM execution |
| 12 | Daily PM Briefing | Sonnet | 8AM daily briefing via n8n |

---

## 7. Task Architecture

### 5-Level Hierarchy
```
Level 1 — Phase (Prepare/Explore/Realize/Deploy/Run)
Level 2 — Deliverable Group
Level 3 — Methodology Task (SAP Activate library)
Level 4 — Module Instance (per module in scope)
Level 5 — Sub-Task (per project)
```

### Task Categories
- KEEP: PM executes
- TRACK: Basis/IT executes, PM monitors
- MERGE: Consolidated tasks
- EXCLUDE: SAP internal tasks

### Active Task Library (Seeded in Production)
- GF (Greenfield): 193 active + 54 merge = 247 tasks
- BF (Brownfield): 201 active + 36 merge = 237 tasks
- Total seeded: 484 tasks, 5 phases, 160 deliverable groups

---

## 8. Three Core UX Principles

```
Principle 1 — Phase Gates Everything
  Future phases locked until current complete
  Sidebar shows locked phases greyed out

Principle 2 — Tasks Drive the Screen
  One current task highlighted at all times
  Everything else is upcoming
  PM works through tasks in sequence

Principle 3 — Everything is a CTA
  Agents appear ONLY when task needs them
  Communications only when sign-off needed
  Risk only when risk is relevant
  Schedule only when workshop needed
  Nothing appears that isn't needed NOW
```

---

## 9. Command Center Layout (6 Sections Only)

```
1. Top Bar — "Good morning [PM Name]" + project + date
2. Daily Briefing — only from Agent 12 daily_briefings table
3. Active Phase Summary — phase name + progress bar
4. Current Task Card — task name + [Open Task] button
5. Upcoming Tasks — 5 items, clickable
6. TRACK Alerts — only when track_overdue = TRUE
```

---

## 10. Task Screen Layout

```
LEFT (60%):
  Task name + breadcrumb
  Status selector
  Description, preparation checklist
  Notes (free text), Dates

RIGHT (40%) — CTA Panel:
  [Open Agent] — if agent mapped
  [Draft Communication] — if triggers_stakeholder
  [Schedule Session] — if triggers_meeting
  [Log Risk/Issue] — if triggers_raid or TRACK
  [Draft Change Request] — if triggers_cr
  [Send Template] — NEW: if template linked to task
  
  TRACK Info Box (if is_track = TRUE):
    Executed by, PM role, due date
    [Mark Complete] [Escalate]
```

---

## 11. Database Tables

### M10b Tables (Live in Production)
- activate_phases, activate_deliverables, activate_tasks
- project_tasks, project_sow_extractions, project_wricef
- project_escalations, session_attendance

### Key Existing Tables
- projects, organizations, users
- project_templates, project_document_files
- project_action_items, risks
- workspace_profiles, daily_briefings
- communication_drafts, generated_artifacts
- roadmap_tasks (deprecated Layer 1 — still exists in DB,
  router endpoints removed, agent deleted, sow_intake
  rewired to ProjectPlanAgent)

### NEW Tables Needed (June 11 Codex)
- delivery_templates (template library for distribution)
- project_template_sends (audit of what was sent to whom)

---

## 12. Project Plan Agent — Setup Flow

```
Path A (with documents):
  PM uploads SOW → Opus extracts scope →
  PM confirms/adjusts on confirmation screen →
  Plan generated

Path B (Q&A fallback):
  12 questions → Plan generated

Conditional task activation:
  SAC → only if licensed | WalkMe → only if licensed
  SAP Build → only if licensed | Joule → only if licensed
  Integrations → if in scope | Data migration → if in scope
  WRICEF → always, count from SOW
```

**IMPORTANT — SOW Confirmation Screen must be verified
during E2E pass. Designed per SUD Section 4.1 but not
yet confirmed working in production.**

---

## 13. Key Workflows

### Change Request Flow
- RAID Agent detects scope change → Agent 8 drafts CR
- Three triggers: WRICEF exceeded, new scope, timeline impact

### Escalation Workflow
- 4-level ladder (internal SI + external customer)
- L1=24hrs, L2=48hrs, L3=72hrs, L4=SteerCo

### Session Attendance
- T-5/T-3/T-1 pre-session confirmation chain
- No-show → RAID auto-escalation

### Template Distribution (NEW — to be built June 11)
- PM clicks [Send Template] on relevant task
- Selects recipient from project team
- Octiss sends blank template via Outlook (MS365)
- Optionally: Agent pre-fills with project context
- PM reviews and approves before send

### Phase Auto-Sync (NEW — built June 2026)
- Task PATCH endpoint auto-syncs phase_workflow status
- NOT STARTED → IN PROGRESS (first task started)
- → PENDING PM CONFIRMATION (all tasks complete)
- Never downgrades PM-confirmed COMPLETE

---

## 14. MS365 Configuration

| Item | Value |
|---|---|
| Azure App | Octiss Production |
| Client ID | 3a027ac4-a5f3-452c-b283-42a29c8be118 |
| Redirect URI | https://sap-pmo-agent-production.up.railway.app/api/v1/integrations/microsoft/callback |
| Permissions | User.Read, Files.ReadWrite, Files.Read.All, Sites.Read.All, Mail.Send, Calendars.ReadWrite, offline_access |
| Status | All granted + admin consent applied |

---

## 15. File Locations

| File | Path |
|---|---|
| Backend repo | D:\Mohsin Personal\OneDrive\Automation\SAP PMO Agent\sap-pmo-agent |
| Frontend repo | D:\Mohsin Personal\OneDrive\Automation\EM Intelligence Labs Dashboard |
| Task Library | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Beta\Octiss_Final_Task_Library_v1.0.xlsx |
| Agent Mapping | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Beta\Octiss_M10b5_Agent_Mapping_v1.0.xlsx |
| Beta Tracker | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Beta\Octiss_Beta_Tester_Tracker.xlsx |
| Beta SOW Files | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Beta\[Tester SOW files] |
| Expected Outcomes | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Beta\CONFIDENTIAL_Octiss_Beta_Expected_Outcomes_v1.0.docx |
| Production Credentials | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\production_credentials.txt |
| SUD v1.6 | D:\Mohsin Personal\OneDrive\Automation\EM Intelligence Labs Dashboard\docs\Octiss_SUD_v1.6.md |
| This Handoff | D:\Mohsin Personal\OneDrive\Automation\EM Intelligence Labs Dashboard\docs\Octiss_Master_Handoff_v1.0.md |

---

## 16. Recent Commits (Latest First)

| Commit | Repo | Description |
|---|---|---|
| cc81401 | Frontend | fix: improve CTA error messages |
| 8bc3c74 | Backend | fix: add workflow-drafts endpoint, remove legacy name |
| 10b1bc6 | Frontend | fix: normalize Settings to Linear left nav |
| da952c5 | Frontend | fix: separate n8n status from dispatch readiness |
| a605d52 | Backend | fix: add n8n_configured to integrations settings |
| 978dbf8 | Frontend | fix: surface SteerCo PMO drafts in ProjectReports |
| 2093e7c | Frontend | fix: SSO OAuth session exchange on mount |
| c32bcb0 | Backend | fix: POST /auth/social SSO token exchange |
| 7568372 | Frontend | fix: PENDING_PM_CONFIRMATION amber state |
| 724b42e | Backend | fix: phase auto-sync from task completion |
| 9a5d721 | Frontend | fix: phase navigation + deliverable grouping |
| 73d8a40 | Frontend | fix: auto-select first project on dashboard |
| 97756da | Docs | docs: update handoff after Layer 1 cleanup |
| 4d8735d | Backend | cleanup: remove Layer 1 RAG test file |
| 9d847f1 | Backend | cleanup: remove 6 Layer 1 endpoints from roadmap.py |
| fc7603e | Backend | cleanup: rewire sow_intake, remove roadmap_agent + backups |

---

## 17. Technical Debt (Do Not Forget)

### RoadmapAgent — 4 Files Still Broken (Behind try/except)
These files call RoadmapAgent which was deleted in cleanup.
They fail gracefully (try/except) but need proper rewiring:
```
app/agents/onboarding_agent.py (line 1197)
app/agents/synthesis_agent.py (line 118)
app/routers/onboarding_templates.py (line 186)
app/routers/whatsapp.py (lines 65-413)
Rewire all to ProjectPlanAgent equivalents — June 11
```

### Team Member Entry
Currently manual UI only. No Excel upload built.
Decision: Build simple Add Team Member form (Option B)
Codex session June 11.

### SOW Confirmation Screen
Designed in SUD Section 4.1 but not verified in production.
Must be tested during E2E verification pass.

---

## 18. Beta Testing Plan

### Tester Profiles
| Tester | Project | Customer | Modules | WRICEF | Special |
|---|---|---|---|---|---|
| Mohsin Sardar | Project Atlas | Atlas Retail Group | FI, CO, MM, SD | 8 | Simplest baseline |
| Zeeshan Mustafa | Project Beacon | Beacon Industrial Mfg | FI, CO, MM, SD, PP, QM | 14 | Manufacturing |
| Huma Naseem | Project Crest | Crest Healthcare | FI, CO, MM, SD, HCM, PS | 12 | SAP Build licensed |
| Saad Ali | Project Delta | Delta Engineering | FI, CO, MM, SD, PP, PM, QM | 18 | SAC licensed, complex |
| Tester 5 | TBD | TBD | TBD | TBD | Details pending |

### Beta Tester Flow
```
Step 1: Receive welcome pack (credentials + SOW + Beta Test Script)
Step 2: Login → lands on Command Center
Step 3: Create new project → upload SOW
Step 4: Confirm extraction on SOW confirmation screen
Step 5: Add team members via Add Team Member form
Step 6: Project plan generates automatically
Step 7: Work through tasks, agents, reports, voice
Step 8: Provide feedback via agreed channel
```

### Key Documents Ready
- 4 x SOW documents (Word) — one per tester
- CONFIDENTIAL Expected Outcomes doc — Mohsin only
- User Manual — to be produced before beta launch
- Beta Test Script — to be produced before beta launch
- Welcome Pack emails — to be drafted before beta launch

---

## 19. Delivery Template Distribution — NEW FEATURE

### Decision (08 Jun 2026)
Build Template Distribution feature before beta launch.
Templates are linked to relevant tasks. PM sends blank
or agent-pre-filled templates to consultants via Outlook.

### Phase 1 — Before Beta (Blank Distribution)
```
[Send Template] CTA on task screen
PM selects recipient from project team
Octiss sends blank template via Outlook
```

### Phase 2 — Post Beta (Agent Pre-Fill)
```
Agent reads project context from SOW + task
Pre-fills template with project name, modules, dates
PM reviews → approves → Octiss sends via Outlook
```

### Templates Needed (Mohsin to Upload)
| Template | Format | Linked Phase | Linked Task |
|---|---|---|---|
| BPML | Excel | Explore | BPML preparation and sign-off |
| FSD (Functional Spec) | Word | Realize | FSD development per WRICEF item |
| SDD (Solution Design) | Word | Explore | Solution design document |
| TSD (Technical Spec) | Word | Realize | TSD per WRICEF item |
| Cutover Plan | Excel | Deploy | Cutover planning |
| TR Strategy | Word | Realize | Transport Request strategy |
| Weekly Status Report | Word/PPTX | All phases | Weekly reporting |
| SteerCo Deck | PPTX | All phases | Monthly SteerCo |

### Octiss Color Scheme for Templates
- Primary: #1E3A5F (dark navy)
- Accent: #2E6DA4 (blue)
- Secondary text: #444444
- Background: #F5F7FA
- White: #FFFFFF

---

## 20. Reporting Templates — NEW FEATURE

### Decision (08 Jun 2026)
SteerCo PPTX and Weekly Status Word templates to be
designed in Octiss brand. Agent fills content.
PM reviews and approves before distribution.

### SteerCo PPTX Deck — Slides Designed
- Slide 1: Cover (Project name, date, prepared by)
- Slide 2: Executive Summary (RAG status, key message)
- Slide 3: Phase Progress (all 5 phases, % complete)
- Slide 4: Key Accomplishments This Period
- Slide 5: Risks and Issues (top 3-5)
- Slide 6: Decisions Required
- Slide 7: Upcoming Milestones
- Slide 8: Actions and Owners
- Slide 9: Next SteerCo Date

### Weekly Status Report — Word Template
- Header: Project name, week, PM name, date
- Section 1: Overall RAG status
- Section 2: Phase progress summary
- Section 3: Accomplishments this week
- Section 4: Issues and risks
- Section 5: Actions and owners
- Section 6: Plan for next week

---

## 21. Pre-Beta Checklist (Updated 08 Jun 2026)

| Item | Status |
|---|---|
| Layer 1 codebase cleanup | ✅ Done 07 Jun 2026 |
| 8/10 critical issues fixed | ✅ Done 07-08 Jun 2026 |
| Issue 9 — agent response formatting | ⏳ Jun 11 (Codex prompt ready) |
| Issue 10 — task descriptions | ⏳ Jun 11 (audit needed) |
| RoadmapAgent rewire (4 files) | ⏳ Jun 11 |
| Team Member add form | ⏳ Jun 11 |
| BF Conversion Module (M10b-6c) | ⏳ Jun 11-12 |
| Delivery template library build | ⏳ Jun 11-12 |
| SteerCo PPTX template | ⏳ In design (Mohsin to upload originals) |
| Weekly Status Word template | ⏳ In design |
| E2E verification pass | ⏳ Jun 12-13 (Mohsin) |
| User Manual | ⏳ Jun 13 (Claude drafts) |
| Beta Test Script | ⏳ Jun 13 (Claude drafts) |
| Welcome pack emails | ⏳ Jun 14-15 |
| Tester 5 details and SOW | ⏳ Pending from Mohsin |
| Send beta tester credentials | 🔒 Jun 15-16 |

---

## 22. What To Do In Next Session (June 11)

**IMPORTANT: Codex credits reset June 11 at 12:04 PM PKT**

### Codex Session 1 — Issues 9 and 10
```
Paste the saved Issue 9 prompt (already written).
After Issue 9 committed, audit and fix Issue 10.
```

### Codex Session 2 — RoadmapAgent Rewire
```
Rewire these 4 files to ProjectPlanAgent:
  app/agents/onboarding_agent.py
  app/agents/synthesis_agent.py
  app/routers/onboarding_templates.py
  app/routers/whatsapp.py
```

### Codex Session 3 — Team Member Add Form
```
Build simple Add Team Member form:
  Name, Role, Module, Email, Team (SI/Customer)
  Saves to team_members table
  Appears in project setup flow after plan generation
```

### Codex Session 4 — Delivery Template Library
```
Build Template Distribution feature:
  delivery_templates table
  project_template_sends table
  [Send Template] CTA on task screen
  Recipient selector from team_members
  Send via Outlook (MS365 already connected)
  Templates loaded from Octiss template library
```

### Codex Session 5 — BF Conversion Module
```
M10b-6c BF Conversion Module:
  Pre-SUM checklist
  SUM execution tracker
  Post-SUM checklist
  Downtime Calculator
```

### After Codex — E2E Verification (Mohsin)
```
Log in as fresh user on octiss-production.vercel.app
Test every screen in sequence (see Section 18)
Document every issue
Fix before beta launch
```


---

## 19. Project Delivery Template Library

Octiss ships 31 professionally branded project delivery templates (Word, Excel, PPTX) built by EM Intelligence Labs on 08 June 2026. These are the physical artifacts that Octiss agents reference, generate, and route to stakeholders via n8n automation.

### Template Inventory

| # | Template Name | Type | SAP Activate Purpose |
|---|---|---|---|
| 1 | FSD — Workflow | Word | Functional Spec for workflow enhancements |
| 2 | FSD — Conversion | Word | Functional Spec for data conversion objects |
| 3 | FSD — Report | Word | Functional Spec for custom reports |
| 4 | FSD — Enhancement | Word | Functional Spec for enhancements |
| 5 | FSD — Form | Word | Functional Spec for custom forms |
| 6 | FSD — Interface | Word | Functional Spec for interfaces/integrations |
| 7 | Project Charter | Word | Project initiation, scope, team structure |
| 8 | Minutes of Meeting | Word | Workshop and meeting minutes |
| 9 | Phase Acceptance & Closure Certificate | Word | Phase sign-off and closure |
| 10 | RAID Management Methodology | Word | Risk/Assumption/Issue/Decision methodology |
| 11 | Change Control Methodology | Word | CR process and CCB methodology |
| 12 | Document Management Methodology | Word | Document categorisation and control |
| 13 | Stakeholder Communication Methodology | Word | Governance committees and communication standards |
| 14 | Project Structure & Scheduling Methodology | Word | WBS, planning levels, workstreams |
| 15 | Solution Design Document | Word | Process levels and solution design |
| 16 | Enterprise Structure Document | Word | Org structure, plants, purchasing/sales areas |
| 17 | Master Data Management Strategy | Word | Data cuts, CMDT roles, migration tools |
| 18 | Testing Strategy | Word | UT/SFT/FIT/UAT/Regression test approach |
| 19 | UAT Document | Word | Test scenarios, cases, execution, sign-off |
| 20 | Training Enablement Strategy | Word | CE/SA workstreams, TTT, training plan |
| 21 | Access Management & UX Strategy | Word | Role types, RAM, Fiori UX, responsibility matrix |
| 22 | Technical Landscape Strategy | Word | Platform options, landscape tables, integration design |
| 23 | BPML | Excel | Business Process Master List with LOB reference and summary |
| 24 | Product Backlog | Excel | MoSCoW priority backlog with WRICEF tracking |
| 25 | Test Script | Excel | Test scenarios, pass/fail CF, auto-summary |
| 26 | RAM (Role Assignment Matrix) | Excel | Role definitions, derived roles, auth objects |
| 27 | Q-Gate Checklist | Excel | SAP Activate quality gate assessment — all 4 phases |
| 28 | Project Kickoff Deck | PPTX | 25-slide full kickoff presentation |
| 29 | Project Status Report | PPTX | 10-slide weekly status report with RAG dashboard |
| 30 | Flowchart & Diagram Templates | PPTX | Process flowchart templates (⏳ pending) |
| 31 | [Reserved] | — | — |

### Branding Standard Applied to All Templates

- **Primary:** #1E3A5F (navy) | **Accent:** #2E6DA4 (blue) | **Light:** #F5F7FA
- **Three-tier field system:**
  - Black text = pre-filled by Octiss (project name, dates, methodology)
  - `[Navy brackets]` = consultant fills in the substantive content
  - *Guidance: grey italic* = instructional text — delete before submitting
- Header/footer: "Octiss | EM Intelligence Labs" branding on every page
- Amber Template Guide banner on every document

### How Templates Connect to the Product

Agents in Octiss do NOT fill templates themselves. The agent guides the PM on what to check, request, and escalate. Templates are surfaced as CTAs on the Task Screen.

| Template | Triggered By | Agent |
|---|---|---|
| FSD variants | WRICEF task in Explore/Realize | Agent 4 — Explore |
| Project Charter | Charter task in Prepare | Agent 3 — Prepare |
| Minutes of Meeting | Post-workshop auto-generation | Agent 9 — Meeting Intelligence |
| Phase Acceptance Certificate | Phase gate completion | Agent 1 — SAP Activate Governance |
| BPML | Fit-to-Standard workshop task | Agent 4 — Explore |
| Test Script / UAT Document | Testing tasks in Realize | Agent 5 — Realize |
| Q-Gate Checklist | Phase gate quality review | Agent 1 — SAP Activate Governance |
| Project Kickoff Deck | Kickoff meeting task in Prepare | Agent 3 / Agent 9 |
| Project Status Report | Weekly reporting task | Agent 8 / Agent 12 |
| RAM | Access management task | Agent 8 — Stakeholder Communication |
| Training Strategy | Training preparation in Deploy | Agent 6 — Deploy |

### n8n Routing Rules for Approved Templates

Once PM approves a template in Octiss, n8n routes it automatically:

| Template | To (recipients) | CC |
|---|---|---|
| FSD (module-specific) | Module consultant (matching workstream) | Customer PM |
| MOM | All session attendees | Customer PM |
| Phase Closure Certificate | Customer PM + Executive Sponsor | SI Firm PM |
| Status Report | Steering Committee + Customer PM | — |
| UAT Document | Key users (per module) + Customer PM | Module consultant |
| BPML (final) | Customer PM + Business team | Module consultant |

All documents stored in the project's OneDrive folder structure by n8n after dispatch.

### File Locations (Local)

| Item | Path |
|---|---|
| Output templates | `D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Beta\Templates\` |
| Build scripts | `/home/claude/octiss-templates/` (session of 08 Jun 2026) |
| File naming convention | `[TemplateName]_Octiss_Template.[docx/xlsx/pptx]` |

### Status (08 June 2026)

| Group | Templates | Status |
|---|---|---|
| A — 6 FSD variants | Word | ✅ Complete |
| B — Charter, MOM, Phase Closure | Word | ✅ Complete |
| C — RAID, Change Control, Doc Mgmt, Stakeholder, Scheduling | Word | ✅ Complete |
| D — SDD, Enterprise Structure, Master Data | Word | ✅ Complete |
| E — Testing Strategy, UAT, Training | Word | ✅ Complete |
| F — Access/UX, Technical Landscape | Word | ✅ Complete |
| G — BPML, Product Backlog, Test Script, RAM | Excel | ✅ Complete |
| H — Q-Gate Checklist | Excel | ✅ Complete (v2 — Excel repair dialog fixed) |
| I — Kickoff Deck, Status Report | PPTX | ✅ Complete |
| J — Flowchart Templates | PPTX | ⏳ Pending |