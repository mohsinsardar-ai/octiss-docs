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

## 4. Current Build State (04 June 2026 UTC / 05 June 2026 PKT)

### What Is Live and Working
- Backend: 394 routes, 1302 tests passing
- Frontend: Vercel production, 107 Playwright tests passing / 48 skipped
- Production URLs: `https://octiss-production.vercel.app` -> `https://sap-pmo-agent-production-3f52.up.railway.app`
- Setup 5/5 READY for the production tester workspace after safe integration settings were configured
- Microsoft 365 Railway env vars are present in production; tester workspace status endpoint reports `configured=true`
- Production GF test project: `b82f0d75-bdc6-41ea-8387-1f48ff7d5afd` (`GF Test Project`, 244 Activate-backed tasks)
- Production BF test project: `e9cec48e-01f4-4506-a212-a9037ed76db9` (`BF Test Project`, 235 Activate-backed tasks)
- Activate task filtering is fixed: production GF/BF screens no longer mix in legacy demo task rows
- 10 M10b DB tables live in production, including BF checklist/SUM tables with `service_role` grants
- Sidebar: Clean 7-item layout
- Command Center: 6-section clean layout (REBUILT)
- Task Screen: Built with CTA rendering, PM Review Agent panel, stable detail save/readback, and text assignee compatibility
- BF Conversion Module: production DB migration applied; QAS task CTA, Pre-SUM checklist, SUM tracker, downtime calculator, and Agent 8 cutover pack path verified
- BF downtime calculator production result for 420GB / 850 Z-objects / 300 QAS mins / 8 CPU / 64GB RAM: 11.73 hours estimated, 14.7-hour minimum window, 423-minute rollback point, HIGH confidence
- SAP Knowledge Base: Built in M8D with SAP official content
- Voice Copilot: Built in M10
- 12 specialist agents built and live
- PM-review-only and dry-run safety remain the default for generated content and automation outputs

### Known Issues (Fix These First)

| # | Issue | Severity | Status |
|---|---|---|---|
| 1 | Microsoft 365 production integration status is configured but not connected/verified | Blocker | API reports `configured=true`, `connected=false`, `verified=false`; OAuth connect/verification still required |
| 2 | Project Alpha has legacy demo data | Info | Use GF/BF test projects above for production verification |
| 3 | Beta tester onboarding | On hold | Resume after Microsoft connector is genuinely verified |
| 4 | User Manual and Welcome Packs | Pending | Start after the production UI remains stable |

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
| Production API Status | `configured=true`, `connected=false`, `verified=false` |
| Next Step | Complete Microsoft OAuth connect/verification so the backend status endpoint returns `connected=true` and `verified=true` |
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
| current docs update | Frontend/Public Docs | End-of-session Handoff/SUD update for production verification and MS365 blocker; final docs commit hashes live in git history because the file cannot self-reference its own final hash |
| a36553e | Backend | BF conversion migration grants for production `service_role` access |
| 724bf0f | Backend | Project task text assignees stored safely in `agent_context.assigned_to_label` |
| 2599cf4 | Frontend | Task detail autosave race fixed so status refreshes do not wipe unsaved edits |
| ad6d809 | Backend | Workspace setup status endpoint and Activate-backed production task filtering |
| 3c40b91 | Frontend | Production backend URL corrected and BF task environment inference added |
| 44472fa | Frontend Docs | Railway CLI automation status |
| 3da6017 | Public Docs | Railway CLI automation status |
| 5d59571 | Public Docs | Handoff + SUD updated after BF Conversion Module |
| a49f7ab | Frontend Docs | Handoff + SUD updated after BF Conversion Module |
| a5e04cc | Frontend | BF Conversion Module screen, task CTAs, SUM tracker, downtime calculator, and cutover comms pack CTA |
| 44d00df | Backend | BF Conversion Module endpoints, service, migration SQL, and route tests |
| ccfcb17 | Public Docs | CLI inventory and permanent docs update rule baseline |
| cdb4885 | Frontend Docs | Handoff + SUD CLI inventory and permanent docs update rule baseline |
| 3f6718f | Frontend | Update Playwright fixtures to real production UUIDs |
| 82f09d5 | Frontend | Fix Task Screen + Phase View deliverable grouping |
| f0c8b88 | Backend | Deliverable enrichment + task detail consistency |
| 7e03eee | Docs | Initial public docs repo |
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
| Fix sidebar routing | ✅ 16794ab |
| Fix Daily Briefing data source | ✅ 16794ab |
| Fix Phase View Ungrouped Deliverable | ✅ 82f09d5 |
| Fix Task Screen CTAs render correctly | ✅ 82f09d5 |
| Verify Agent Panel fires and responds | ✅ Production GF task verified |
| Public docs repo created | ✅ 7e03eee |
| Playwright fixtures use real UUIDs | ✅ 3f6718f |
| CLI capabilities documented in Handoff | ✅ Task 0A |
| Production backend URL points to Railway `-3f52` service | ✅ 3c40b91 |
| Workspace setup endpoint reports 5/5 | ✅ ad6d809 + production settings verification |
| Production task counts filtered to Activate-backed rows | ✅ GF 244 / BF 235 |
| Task detail save/readback survives status autosave | ✅ 2599cf4 + production GF verification |
| Text assignee labels do not 500 against UUID DB column | ✅ 724bf0f + production readback |
| M10b-6c BF Conversion Module code | ✅ Backend 44d00df, frontend a5e04cc |
| Apply BF Conversion Module migration in production | ✅ Applied through production pooler; grants committed in a36553e |
| Verify BF checklist / SUM / downtime in production | ✅ QAS BF task path verified; SUM active; downtime 11.73h result |
| Frontend Playwright regression suite | ✅ 107 passed / 48 skipped |
| Backend pytest suite | ✅ 1302 passed |
| Microsoft 365 production connector | Railway MS365 env vars present; API `configured=true`; OAuth connect/verification still pending (`connected=false`, `verified=false`) |
| User Manual | 📋 Post stable UI and MS365 verification |
| Update Welcome Packs | 📋 Post User Manual |
| Send beta tester credentials | 🔒 On hold |

---

## 18. What To Do In Next Session

Step 1 — Connect and verify Microsoft 365 production connector
Railway MS365 environment/OAuth values are present and `/api/v1/integrations/microsoft/status` returns `configured=true`. Complete OAuth connect/verification, then verify `connected=true` and `verified=true`.

Step 2 — Keep using the dedicated production test projects
Use `GF Test Project` (`b82f0d75-bdc6-41ea-8387-1f48ff7d5afd`) and `BF Test Project` (`e9cec48e-01f4-4506-a212-a9037ed76db9`) for all production regression checks. Do not use legacy `Sample Project Alpha` as a baseline.

Step 3 — User Manual
Create the User Manual after Microsoft is verified and the stable production UI has one more clean pass.

Step 4 — Welcome Packs and beta onboarding
Update Welcome Packs and send beta tester credentials only after the User Manual and Microsoft connector are complete.

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
| railway | YES | v5.0.0 | Authenticated - mohsin.sardar@gmail.com. Linked to octiss-production / sap-pmo-agent. Use flag form to link (see Section 20). |
| vercel | YES | 53.2.0 | Authenticated; Octiss production project visible |
| supabase CLI | YES | 2.98.2 | Authenticated + linked to qrxfprybbpqugptakeke |
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
- Supabase migrations: PARTIALLY AUTOMATED. Supabase CLI is linked to qrxfprybbpqugptakeke, and Codex can run `supabase db push`. Current migration filenames are skipped until renamed to Supabase timestamp format.
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
