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
| Backend | https://sap-pmo-agent-production-3f52.up.railway.app |
| MS365 Redirect URI | https://sap-pmo-agent-production-3f52.up.railway.app/api/v1/integrations/microsoft/callback |
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

## 4. Current Build State (05 June 2026)

### What Is Live and Working
- Backend: 393+ routes, 1302+ tests passing
- Frontend: Deployed Vercel, 110 Playwright passed / 48 skipped
- Setup 5/5 READY — MS365 connected
- GF Test Project: 244 Activate tasks ✅
- BF Test Project: 235 Activate tasks ✅
- Active project resolver: FIXED globally
- Project Initiation Module: BUILT ✅
- SOW upload + extraction: LIVE ✅
- Project team roster: LIVE ✅
- Task ownership mapping: LIVE ✅
- Notes: timestamped log entries ✅
- Dates: DD-MMM-YYYY everywhere ✅
- Agent context: SOW + team + SAP chunks ✅
- BF phase tracker: FIXED ✅
- Login copyright footer: ADDED ✅

### Known Issues
| # | Issue | Status |
|---|---|---|
| 1 | AI Roadmap shows NVIDIA content | 📋 UX overhaul |
| 2 | Settings page too cramped | 📋 UX overhaul |
| 3 | Task descriptions still generic | 📋 UX overhaul |
| 4 | Social login not yet enabled | 📋 UX overhaul |
| 5 | BF Tests G1-H9 not yet done | ⏳ Next test session |

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
- bf_sum_checklists
- bf_sum_executions
- project_team_members

### Key Legacy Tables
- projects
- roadmap_tasks (legacy — Project Alpha uses these)
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
| Redirect URI | https://sap-pmo-agent-production-3f52.up.railway.app/api/v1/integrations/microsoft/callback |
| Permissions | User.Read, Files.ReadWrite, Files.Read.All, Sites.Read.All, Mail.Send, Calendars.ReadWrite, offline_access |
| Production API Status | `configured=true`, `connected=true` |
| OAuth URL Route | `/api/v1/integrations/microsoft/connect` and `/api/v1/integrations/microsoft/auth` both return auth URLs |
| Capabilities | OneDrive CONFIGURED; Outlook CONFIGURED; Calendar CONFIGURED; Project Document Folders CONNECTED; SharePoint N/A — personal Hotmail account has no SPO license |
| Automation Mode | Dry-run active |
| Next Step | Proceed with Premium UI/UX overhaul; SharePoint is not required for beta |
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
| eefa8e3 | Frontend | Login copyright footer |
| c214c01 | Frontend | BF tracker + active project fixes |
| fb67edf | Backend | Notes log, dates, agent context, initiation module |
| 0808c46 | Frontend | Docs update |
| adc985f | Public Docs | Docs update |
| 90504559 | Frontend | Global active project resolver |
| 5ccd18f | Frontend | Phase View project resolver |
| 44472fa | Frontend | Railway docs update |
| 3da6017 | Public Docs | Railway docs update |
| 5553a7692 | Frontend | MS365 docs update |
| 86bd0d57 | Public Docs | MS365 docs update |
| 8f1ecfb | Backend | MS365 MSAL scope fix |
| a5e04cc | Frontend | M10b-6c BF Conversion UI |
| 44d00df | Backend | M10b-6c BF Conversion Module |
| 82f09d5 | Frontend | Fix Task Screen + Phase View grouping |
| f0c8b88 | Backend | Deliverable enrichment |
| 3f6718f | Frontend | Playwright fixtures real UUIDs |
| 16794ab | Frontend | Fix sidebar routing + Daily Briefing |

---

## 17. Pre-Beta Checklist

| Item | Status |
|---|---|
| Fix sidebar routing | ✅ 16794ab |
| Fix Phase View grouping | ✅ 82f09d5 |
| Fix Task Screen CTAs | ✅ 82f09d5 |
| BF Conversion Module | ✅ 44d00df |
| MS365 connected + configured | ✅ 8f1ecfb |
| Active project resolver global fix | ✅ 90504559 |
| BF phase tracker fixed | ✅ c214c01 |
| Notes timestamped log | ✅ fb67edf |
| Dates DD-MMM-YYYY everywhere | ✅ fb67edf |
| Agent context SOW + team + SAP | ✅ fb67edf |
| Project Initiation Module | ✅ fb67edf |
| GF + BF test projects seeded | ✅ fb67edf |
| Login copyright footer | ✅ eefa8e3 |
| Playwright 110 passed | ✅ |
| BF manual tests G1-H9 | ⏳ Next session |
| Premium UI/UX overhaul | 📋 After Q1 answer |
| Social login (Microsoft + Google) | 📋 In UI/UX session |
| User Manual | 📋 Post UI/UX |
| Welcome Packs updated | 📋 Post Manual |
| Beta tester credentials sent | 🔒 On hold |

---

## 18. What To Do In Next Session

RULE: No testing until ALL features built.
Test Round 2 is the final pre-beta gate.

BUILD QUEUE — execute in this order:

### Priority 1 — Fix Project Creation Flow
Show two options at project creation:
A) Create with SOW (recommended)
B) Create without SOW (Q&A fallback)
Agent 2 handles both paths.
This is a frontend + backend fix.

### Priority 2 — Task Exclude/Delete
Add to Phase View and Task Screen:
[Exclude Task] = hidden, recoverable,
logged in audit trail.
[Delete Task] = permanent + confirmation.

### Priority 3 — n8n Setup
Step by step (Mohsin does manually):
1. Open https://n8n-production-c148.up.railway.app
2. Import workflow files from
   n8n/workflows/module8/
3. Replace {BACKEND_URL} and
   {N8N_INTERNAL_API_KEY} placeholders
4. Activate all 6 workflows

### Priority 4 — Automation Setup in Initiation
Add Step 4 to Project Initiation flow.
PM sets schedule per workflow.
PM sets delivery preferences.

### Priority 5 — Automation Settings Screen
Settings → Automation section.
Left sidebar nav (Claude console style).
Per-workflow cards with schedule control.

### Priority 6 — CTA Workflows 7-16
Build all document automation workflows.
All CTA-triggered, PM-approved.
Fit-Gap, BPML, Sign-off, MOM,
SteerCo, Phase Gate, CR, Attendance.

### Priority 7 — Google Ecosystem
Google Drive, Gmail, Google Calendar.
Same pattern as Microsoft.
PM chooses ecosystem at initiation.

### Priority 8 — Kanban Board View
New view — status columns or phase swimlanes.
Task cards with WBS, owner, due date.
Drag to change status.

### Priority 9 — Calendar View
Monthly/weekly/daily calendar.
Tasks on planned due dates.
Colour coded by phase.

### Priority 10 — Premium UI/UX Overhaul
Waiting for Mohsin Q1 design reference.
Covers all GF test feedback items.
Includes: social login, Agent Assist,
WBS numbering, Settings restructure,
dates, task descriptions, Kanban styling.

### Priority 11 — Test Round 2
Full end-to-end test of everything.
Every workflow from SOW to MOM.
User Manual written from this test.

### Priority 12 — Beta Onboarding
After Test Round 2 clean.
5 testers. Welcome packs sent.

---

## 19. Codex Environment — CLI Capabilities

Last verified: 05 June 2026

| CLI | Available | Version | Auth Status |
|-----|-----------|---------|-------------|
| gh | YES | 2.92.0 | Authenticated - mohsinsardar-ai |
| git | YES | 2.53.0.windows.2 | Configured |
| node | YES | v24.14.1 | N/A |
| npm | YES | 11.11.0 | N/A |
| python | YES | 3.12.10 | N/A |
| pip | YES | 26.1.1 | N/A |
| railway | YES | v5.0.0 | Authenticated - mohsin.sardar@gmail.com. Relinked this session to octiss-production / sap-pmo-agent using the flag form in Section 20. |
| vercel | YES | 53.2.0 | Authenticated; Octiss production project visible |
| supabase CLI | YES | 2.98.2 | Authenticated and production project visible; current backend checkout was not linked during this session. Production DB migration/seed was applied through Railway `DATABASE_URL` with `asyncpg`. Run `supabase link --project-ref qrxfprybbpqugptakeke` before `supabase db push`. |
| playwright | YES | 1.59.0 | N/A |
| curl | YES | 8.19.0 | N/A |
| httpx | YES | 0.28.1 | N/A |
| jq | YES | 1.8.1 | N/A |

### What Codex Can Automate End-to-End
(Zero manual steps from Mohsin in the current CLI auth state):
- Create and push to GitHub repos
- Commit and push all code changes
- Frontend deploys - auto via Vercel on push
- Backend deploys - auto via Railway on push
- Check Railway logs: `railway logs --tail 20`
- Check service status: `railway status`
- Check env vars: `railway variables`
- Redeploy service: `railway redeploy`
- Run Supabase CLI dry-runs and push timestamped migrations through `supabase db push`
- Run full Playwright test suite
- Parse JSON API responses with jq
- Sync public octiss-docs repo
- Inspect Vercel production project state
- Run HTTP checks with curl and httpx

### What Requires Mohsin to Act
(Manual steps that cannot be automated from this non-interactive shell):
- Approve any external send - email, WhatsApp, calendar invite. Dry-run is default. Nothing sends without PM approval.
- Existing Supabase migration files need Supabase timestamp prefixes before `supabase db push` can apply them; the backend repo is already linked.
- Anthropic API key rotation - if key expires.
- Supabase DB password rotation - if changed.
- Azure app permission changes - via Azure portal.
- New paid service signup - requires Mohsin decision per bootstrap philosophy.

---

## 20. Founder Context

### What Still Requires Mohsin to Act
- External sends remain PM-approval-only. Codex can draft and dry-run, but cannot send email, WhatsApp, or calendar invites without explicit approval.
- Supabase migrations: PARTIALLY AUTOMATED. Supabase CLI is authenticated and production is visible, but the current backend checkout was not linked during the 05 Jun 2026 session. Run `supabase link --project-ref qrxfprybbpqugptakeke` before `supabase db push`. The Project Initiation table migration was applied through Railway `DATABASE_URL` with `asyncpg`.
- Railway first-time auth: run `railway login` once from local Windows Terminal. After that, Codex inherits auth automatically. If auth expires, repeat this step.
- Anthropic API key rotation, Supabase DB password rotation, Azure app permission changes, and new paid service signups still require Mohsin.

### Railway Production Link

| Item | Value |
|---|---|
| CLI | railway |
| Available | YES |
| Version | v5.0.0 |
| Auth Status | Authenticated + linked to sap-pmo-agent |
| Workspace | mohsinsardar-ai's Projects |
| Project | octiss-production |
| Project ID | 6a04c677-c1ef-46bf-8ec4-fd1cba153b70 |
| Environment | production |
| Environment ID | 7ec35ab1-fd55-46f9-8dc5-0a4474351882 |
| Service | sap-pmo-agent |
| Service ID | 195ffd46-d9af-4556-bd98-d9b4aea83e1c |
| Backend URL | https://sap-pmo-agent-production-3f52.up.railway.app |

### Railway Link Command (Future Sessions)

If `railway status` shows "No linked project", run this exact command to relink:

```bash
railway link \
  --project 6a04c677-c1ef-46bf-8ec4-fd1cba153b70 \
  --environment 7ec35ab1-fd55-46f9-8dc5-0a4474351882 \
  --service 195ffd46-d9af-4556-bd98-d9b4aea83e1c
```

Note: `railway login` must be run once from local Windows Terminal, not the Codex shell, before this link command will work. Auth persists across sessions after that.
