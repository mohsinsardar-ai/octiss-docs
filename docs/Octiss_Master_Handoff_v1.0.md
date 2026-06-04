# Octiss — Master Handoff Document
**Version:** v1.0 | **Date:** 04 June 2026
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
| Production Credentials | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\production_credentials.txt |

---

## 3. Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React/Vite, TanStack Query, Tailwind, Vercel |
| Backend | FastAPI, Python, Railway |
| Database | Supabase (PostgreSQL + pgvector) |
| Auth | Supabase Auth |
| AI Models | Anthropic Claude (Opus 4 for complex, Sonnet for standard) |
| Embeddings | Hugging Face BAAI/bge-m3 |
| MS365 | Microsoft Graph API (MSAL) |
| Automation | n8n |
| Voice | Voice transcription (built in M10) |

---

## 4. Current Build State (04 June 2026)

### What Is Live and Working
- Backend: 386 routes, 1288 tests passing
- Frontend: Deployed Vercel, 100 Playwright tests passing
- Setup 5/5 READY — all setup steps complete
- MS365 fully connected — OneDrive, Outlook, Calendar verified
- Azure App: "Octiss Production" — 7 permissions granted
- 484 tasks seeded in production (GF 193+54, BF 201+36)
- 8 new DB tables live in production
- Sidebar: Clean 7-item layout
- Command Center: 6-section clean layout (REBUILT)
- Task Screen: Built with CTA rendering and agent panel
- SAP Knowledge Base: Built in M8D with SAP official content
- Voice Copilot: Built in M10
- 12 specialist agents built and live

### Known Issues (Fix These First)

| # | Issue | Severity | Status |
|---|---|---|---|
| 1 | Sidebar phase tracker routes to old `/projects/{id}/prepare` instead of `/phase/prepare` | 🔴 Critical | Fix sent to Codex — commit 16794ab |
| 2 | Daily Briefing pulls from DEMO risks/actions data instead of Agent 12 daily_briefings table | 🟡 Medium | Fix sent to Codex — commit 16794ab |
| 3 | Task Screen CTA buttons and agent panel need live verification after fix 1 | 🟡 Medium | Pending test |
| 4 | Phase View showing all tasks as "Ungrouped Deliverable" | 🟡 Medium | Pending fix |
| 5 | DEMO project has legacy data — new projects will look different | ℹ️ Info | By design |

---

## 5. Milestone History

| Milestone | What Was Built |
|---|---|
| M1 | Project Knowledge RAG Foundation — document tables, chunk tables, vector search |
| M2 | Automatic ingestion — generated/internal artifacts, source tracking |
| M3 | Ingestion expansion — meetings/MOMs, actions, RAID, reports, roadmap tasks |
| M4 | Microsoft Graph / OneDrive / SharePoint file ingestion |
| M5 | PMO intelligence layer |
| M6 | Meeting intelligence — transcription, MOM extraction |
| M7 | Multi-agent orchestration |
| M8D | SAP Knowledge Base — SAP official content, chunk totals, source hierarchy |
| M9 | 11 specialist agents live |
| M10 | Enterprise hardening, billing foundation, voice copilot, rebrand to Octiss |
| M10b | SAP Activate Workflow Engine — task library, agent mapping, Project Plan Agent, sidebar rebuild, Command Center rebuild, task screen |

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
- Total seeded: 484 tasks
- 5 phases, 160 deliverable groups

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
1. Top Bar
   "Good morning [PM Name]"
   "[Project] · [GF/BF] · [Phase] Phase"
   "[Date]"

2. Daily Briefing (hidden if none exists)
   Only from Agent 12 daily_briefings table
   Never from risks or action_items

3. Active Phase Summary
   Phase name + progress bar
   "X of Y tasks complete"

4. Current Task Card
   Task name, phase/deliverable, status
   [Open Task →] button

5. Upcoming Tasks (5 items, clickable)
   Real task names
   Clickable — opens Task Screen
   COMPLETE locked until task is current

6. TRACK Alerts (hidden if none)
   Only shows when track_overdue = TRUE
```

---

## 10. Task Screen Layout

```
LEFT (60%):
  Task name + breadcrumb
  Status selector
  What this task involves (description)
  What you need (preparation checklist)
  Notes (free text, saved to DB)
  Dates (planned/actual, saved to DB)

RIGHT (40%) — CTA Panel:
  [Open {Agent Name}] — always if agent mapped
  [Draft Communication] — if triggers_stakeholder
  [Schedule Session] — if triggers_meeting
  [Log Risk/Issue] — if triggers_raid or TRACK
  [Draft Change Request] — if triggers_cr
  
  TRACK Info Box (if is_track = TRUE):
    "Executed by: [track_owner]"
    "Your role: Monitor and escalate"
    Due date, responsible person
    [Mark Complete] [Escalate]
```

---

## 11. Database Tables

### New M10b Tables (Live in Production)
- activate_phases
- activate_deliverables
- activate_tasks
- project_tasks
- project_sow_extractions
- project_wricef
- project_escalations
- session_attendance

### Key Legacy Tables
- projects
- roadmap_tasks (legacy — DEMO project uses these)
- project_templates, project_documents
- project_action_items, risks
- workspace_profiles
- daily_briefings (Agent 12 output)

---

## 12. Project Plan Agent — Setup Flow

```
Path A (with documents):
  PM uploads SOW/DDA/Handover
    ↓
  Claude Opus extracts scope (free-text)
    ↓
  PM confirms or adjusts extraction
    ↓
  Plan generated

Path B (Q&A fallback):
  12 questions about methodology,
  modules, tools, WRICEF count,
  go-live date, integrations
    ↓
  Plan generated from answers

Conditional task activation:
  SAC tasks → only if SAC licensed
  WalkMe tasks → only if WalkMe licensed
  SAP Build → only if licensed
  Joule → only if licensed
  Integration tasks → if integrations in scope
  Data migration → if in scope
  WRICEF tasks → always, count from SOW
```

---

## 13. Key Workflows Designed (DB Ready, UI Pending)

### Change Request Flow
- RAID Agent detects scope change
- Agent 8 drafts formal CR document
- Agent 1 validates methodology impact
- Three triggers: WRICEF exceeded, new scope, timeline impact

### Escalation Workflow
- 4-level ladder (internal SI + external customer)
- L1=24hrs, L2=48hrs, L3=72hrs, L4=SteerCo
- RAID detects → Agent 8 drafts → Agent 1 validates L3/L4
- Table: project_escalations (live in production)

### Session Attendance
- T-5/T-3/T-1 pre-session confirmation
- Agent 9 sends invites/reminders
- No-show → RAID auto-escalation
- Recurring pattern (2+) → programme escalation
- Table: session_attendance (live in production)

---

## 14. MS365 Configuration

| Item | Value |
|---|---|
| Azure App | Octiss Production |
| Client ID | 3a027ac4-a5f3-452c-b283-42a29c8be118 |
| Redirect URI | https://sap-pmo-agent-production.up.railway.app/api/v1/integrations/microsoft/callback |
| Permissions | User.Read, Files.ReadWrite, Files.Read.All, Sites.Read.All, Mail.Send, Calendars.ReadWrite, offline_access |
| Status | All granted + admin consent applied |
| Home page | https://octiss.com |

---

## 15. File Locations

| File | Path |
|---|---|
| Backend repo | D:\Mohsin Personal\OneDrive\Automation\SAP PMO Agent\sap-pmo-agent |
| Frontend repo | D:\Mohsin Personal\OneDrive\Automation\EM Intelligence Labs Dashboard |
| Task Library | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Beta\Octiss_Final_Task_Library_v1.0.xlsx |
| Agent Mapping | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Beta\Octiss_M10b5_Agent_Mapping_v1.0.xlsx |
| Beta Tracker | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Beta\Octiss_Beta_Tester_Tracker.xlsx |
| Production Credentials | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\production_credentials.txt |
| SUD v1.6 | D:\Mohsin Personal\OneDrive\Automation\EM Intelligence Labs Dashboard\docs\Octiss_SUD_v1.6.md |
| This Handoff | D:\Mohsin Personal\OneDrive\Automation\EM Intelligence Labs Dashboard\docs\Octiss_Master_Handoff_v1.0.md |

---

## 16. Recent Commits (Latest First)

| Commit | Repo | Description |
|---|---|---|
| 16794ab | Frontend | Fix sidebar routing + Daily Briefing source |
| a39e52b | Frontend | Enforce 6-section Command Center |
| baeab4b | Frontend | Task-led Command Center rebuild |
| 321c765 | Backend | Task agent chat endpoint |
| b41176d | Frontend | Display linked task names |
| 6f88948 | Backend | Enrich task names from roadmap_tasks |
| 2adc40d | Frontend | Microsoft Connected badge |
| 828cee7 | Frontend | Sidebar rebuild (M10b-7) |
| c51f8f8 | Backend | Project Plan Agent + 5 endpoints |
| 29f66c49 | Backend | Escalation + attendance tables |
| 53bad706 | Backend | M10b-4 schema (6 new tables) |
| 7ce4b96 | Frontend | SUD v1.5 |

---

## 17. Pre-Beta Checklist

| Item | Status |
|---|---|
| Fix sidebar routing → /phase/:phaseCode | ✅ Commit 16794ab |
| Fix Daily Briefing data source | ✅ Commit 16794ab |
| Verify Phase View loads with real tasks | ⏳ Test needed |
| Verify Task Screen CTAs render correctly | ⏳ Test needed |
| Verify Agent Panel fires and responds | ⏳ Test needed |
| Fix Phase View "Ungrouped Deliverable" | ⏳ Fix needed |
| M10b-6c BF Conversion Module | 📋 Post bug fixes |
| User Manual | 📋 Post stable UI |
| Update Welcome Packs | 📋 Post User Manual |
| Send beta tester credentials | 🔒 On hold |

---

## 18. What To Do In Next Session

**Step 1 — Test the product after commit 16794ab**
```
Hard refresh: Ctrl+Shift+R
Test 1: /dashboard → Command Center clean?
Test 2: Click Prepare in sidebar → PhaseView?
Test 3: Click Open Task → Task Screen?
Test 4: Click agent CTA → Agent Panel?
Test 5: Upcoming task → clickable, COMPLETE locked?
```

**Step 2 — Fix Phase View deliverable grouping**
```
All tasks show as "Ungrouped Deliverable"
Should group by activate_deliverables.name
via activate_task_id → deliverable_id join
```

**Step 3 — Full systematic screen testing**
```
Go through every screen methodically
Document every issue found
Fix all issues before beta
```

**Step 4 — M10b-6c BF Conversion Module**
```
After all bugs fixed
Pre-SUM/SUM/Post-SUM checklists
Downtime Calculator
Based on real project checklists
```

**Step 5 — User Manual**
```
After UI is stable
Real screenshots
Step-by-step guide for beta testers
```

