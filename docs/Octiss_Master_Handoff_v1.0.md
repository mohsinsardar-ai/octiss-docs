# Octiss — Master Handoff Document
**Version:** v1.0 | **Date:** 06 June 2026
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

## 4. Current Build State (06 June 2026)

### What Is Live and Working
- Backend: 393+ routes, 1302+ tests passing
- Frontend: Deployed Vercel octiss-production.vercel.app
- Playwright: 110 passed / 48 skipped
- MS365: connected — OneDrive, Outlook, Calendar configured
- n8n: 6 workflows published and active
- GF Test Project: 244 Activate tasks ✅
- BF Test Project: 235 Activate tasks ✅
- Project Initiation Module: ✅ Live
- Project Creation: SOW/Q&A choice screen ✅
- Task Exclude/Delete: ✅ Live
- Automation Setup in Initiation: ✅ Live
- Settings: Left sidebar nav ✅
- CTA Workflows 7-16: ✅ Built
- Google Ecosystem: ✅ Built and configured
- Kanban Board (/board): ✅ Live
- Calendar View (/calendar): ✅ Live
- /board Kanban route: OK registered
- /calendar Calendar route: OK registered
- Task exclude/delete UI: OK Phase View + Task Screen
- PM Inbox: OK Command Center
- Ecosystem selector: OK saves to DB
- Login SSO + footer: OK fixed
- Empty states: OK Command Center, Phase View, Board, Calendar, Notifications, Project Team, Agent Assist, BF Checklist
- Micro-interactions: OK page/card/status transitions, task exclude/restore, note save, Agent Assist slide-in, Kanban drag/drop, modal/skeleton transitions
- Focused Playwright after empty states + micro-interactions: OK passed
- Voice Copilot: OK verified; Web Speech API EN/AR/UR transcription sends transcript to agent query flow
- Multi-language Phase 1: OK English + Arabic RTL via react-i18next
- Backend test suite: OK 1316 passed / 0 failed; n8n production workflow placeholder check skipped with reason
- Social SSO: Microsoft + Google ✅ Live
- Agent context: SOW + team + SAP chunks ✅
- Notes: timestamped log entries ✅
- Dates: DD-MMM-YYYY everywhere ✅
- WBS numbering: Phase View ✅
- Agent Assist panel: renamed ✅
- Linear UI tokens + Inter font: ✅ Live
- Linear shell/sidebar: ✅ Live
- Linear Command Center: ✅ Live
- Linear Phase View + Task Screen: ✅ Live
- Linear Board + Calendar: ✅ Live
- Linear Project Creation + Initiation: ✅ Live
- Linear BF checklist/SUM/downtime screens: ✅ Live
- Focused Playwright after UI overhaul: ✅ Passed

### Known Issues / Pending
| # | Item | Status |
|---|---|---|
| 1 | New migration needs Supabase SQL Editor apply | 20260606153000_project_ecosystem_choice.sql |
| 2 | Settings deep sub-panel token audit | Pending after empty-state pass |
| 3 | Test Round 2 | After UI polish |
| 4 | User Manual | After Test Round 2 |
| 5 | Beta onboarding | On hold |

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
| Next Step | Proceed with Codex 9-task verification, then Premium UI/UX overhaul |
| Home page | https://octiss.com |

---

## 14b. Google Configuration

| Item | Value |
|---|---|
| Google Cloud Project | Mohsin PMO Automation |
| Project ID | gen-lang-client-0123370567 |
| OAuth Client | Octiss Production |
| APIs Enabled | Google Drive, Gmail, Calendar |
| Redirect URI | https://sap-pmo-agent-production-3f52.up.railway.app/api/v1/integrations/google/callback |
| Supabase Callback | https://qrxfprybbpqugptakeke.supabase.co/auth/v1/callback |
| Budget Alert | $5/month — alerts at 50/90/100% |
| Gemini API | NOT enabled — safe |
| Agent Platform API | NOT enabled — safe |
| Railway Env Vars | GOOGLE_CLIENT_ID, GOOGLE_CLIENT_SECRET, GOOGLE_REDIRECT_URI |

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
| d758515 | Backend | Resolve n8n placeholder test failure |
| 702ce47 | Frontend | Multi-language Phase 1 EN/AR RTL |
| c876fb9 | Frontend | Voice Copilot Web Speech verification |
| f76fead | Frontend | Agent Assist panel stabilization |
| 1c11852 | Frontend | Task assist and BF checklist empty interactions |
| 0413f3f | Frontend | Board, calendar, and team empty states |
| 33cc64a | Frontend | Command Center, Phase View, and Notifications empty states |
| aae64cf | Frontend | Empty-state and micro-interaction primitives |
| 9ccea73 | Frontend | Focused Playwright specs updated for Linear UI |
| 0db73cd | Frontend | Linear project setup, initiation, and BF conversion screens |
| 6b28b03 | Frontend | Linear board, calendar, and settings shell |
| 4aa2f86 | Frontend | Linear phase view, task screen, and Agent Assist |
| c73a1d5 | Frontend | Linear tokens, shell, sidebar, login, and Command Center |
| cf7c502 | Backend | Ecosystem selector DB/API support |
| 1a96d93 | Frontend | Verification pass fixes |
| 2acd505 | Frontend | All 9 build tasks + docs |
| dbfcf8c | Frontend | All 9 build tasks feature |
| cd317a3 | Backend | All 9 build tasks backend |
| 929ffa9 | Public Docs | Docs update |
| 5f0f891 | Frontend | Docs n8n complete |
| bf564a3 | Public Docs | Docs n8n complete |
| 54f5b0f | Backend | n8n workflow files updated |
| 90504559 | Frontend | Global active project resolver |
| fb67edf | Backend | Notes, dates, agent context |
| c214c01 | Frontend | BF tracker fixes |
| eefa8e3 | Frontend | Login copyright footer |
| 8f1ecfb | Backend | MS365 MSAL scope fix |
| a5e04cc | Frontend | BF Conversion UI |
| 44d00df | Backend | BF Conversion Module |
| 82f09d5 | Frontend | Task Screen + Phase View fix |
| f0c8b88 | Backend | Deliverable enrichment |
| 16794ab | Frontend | Fix sidebar routing |

---

## 17. Pre-Beta Checklist

| Item | Status |
|---|---|
| Fix sidebar routing | ✅ 16794ab |
| Fix Phase View grouping | ✅ 82f09d5 |
| BF Conversion Module | ✅ 44d00df |
| MS365 connected | ✅ 8f1ecfb |
| Active project resolver | ✅ 90504559 |
| Project Initiation Module | ✅ fb67edf |
| n8n 6 workflows live | ✅ 06 Jun |
| All 9 build tasks complete | ✅ dbfcf8c |
| Migrations applied in Supabase | ✅ 06 Jun |
| Azure Supabase provider enabled | ✅ 06 Jun |
| Google Cloud OAuth setup | ✅ 06 Jun |
| Google Supabase provider enabled | ✅ 06 Jun |
| Google Railway env vars added | ✅ 06 Jun |
| Google budget alert $5/month | ✅ 06 Jun |
| Social SSO Microsoft + Google | ✅ Live |
| Verification pass - 9 tasks | OK 1a96d93 |
| Linear UI/UX overhaul Phase 1 | OK 0db73cd |
| Focused Playwright after Linear UI | OK 9ccea73 |
| Empty states + micro-interactions audit | OK f76fead |
| Focused Playwright after empty states | OK f76fead |
| Voice Copilot verified | OK c876fb9 |
| Multi-language Phase 1 | OK 702ce47 |
| Backend test suite clean | ✅ |
| Settings deep sub-panel UI audit | Next |
| Test Round 2 | After UI polish |
| User Manual | After Test Round 2 |
| Beta Tester Onboarding | 🔒 On hold |

---

## 18. What To Do In Next Session

### Mohsin Actions First:
Review live UI at octiss-production.vercel.app
Take screenshots of:
  Login, Sidebar, Command Center,
  Phase View, Task Screen
Share with Claude for UI polish prompt.

### Codex Queue (in order):

Priority 1 - UI/UX Polish Pass
  Based on Mohsin screenshot review:
  - More generous padding everywhere
  - Stronger typography hierarchy
  - Better visual depth and contrast
  - Linear feel not just dark colours

Priority 2 - Test Round 2
  Full end-to-end test using:
  Octiss_Beta_Test_Tracker_v2.0_Single.xlsx
  Every screen, every flow
  Seeded beta projects

Priority 3 - User Manual
  After Test Round 2 clean
  Real screenshots of final Linear UI

Priority 4 - Beta Tester Onboarding
  5 testers + welcome packs
  🎉 Beta Launch

### Backlog (fix when convenient):
- Digital Twin Phase 2 (MS Teams bot)
  post 3-5 paying customers
- Full 8-language support post-revenue
- Gantt View (post-beta)
- Codebase cleanup (post stable UI)
- DB cleanup — retire DEMO project

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
