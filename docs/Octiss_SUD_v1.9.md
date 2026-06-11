# Octiss — Solution Understanding Document (SUD)
**Version:** v1.9 | **Date:** 11 June 2026 | **Status:** LIVE
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
| v1.0 | 01 Jun 2026 | Initial release |
| v1.1 | 01 Jun 2026 | 5-level task hierarchy, module expansion patterns |
| v1.2 | 01 Jun 2026 | SF (Bluefield) deferred |
| v1.3 | 01 Jun 2026 | Final task library locked. Project Plan Agent architecture defined. |
| v1.4 | 01 Jun 2026 | 394 tasks mapped to 12 agents. CR workflow, Escalation, Session Attendance added. |
| v1.5 | 03 Jun 2026 | M10b-6b, M10b-6a, Seed, M10b-7 confirmed complete. |
| v1.6 | 04 Jun 2026 | MS365 fully configured. Command Center rebuilt. BF Conversion Module planned. |
| v1.7 | 09 Jun 2026 | Project creation flow corrected. CTA state machine locked. WhatsApp removed. Phase Gate evidence-based. Three-panel UI locked. Q-Gate source confirmed. Document Generation feature added. UI/UX deferred. |
| v1.8 | 09 Jun 2026 | WRICEF Change Management Flow locked. Task-to-Template mapping completed for all 5 GF phases. 87 templates complete. Template design standards locked. Document flow principle locked. |
| v1.9 | 11 Jun 2026 | 12 Codex prompts executed. All major features built. Tests 1235→1311. Pending items updated. Item 17 (build 34 new templates) removed — complete. Q-Gate template added to repo. Both repos pushed clean. |

---

## 1. Product Vision

### 1.1 What Octiss Is

Octiss is an AI-powered SAP PMO Intelligence Platform. It guides SAP Project Managers through every phase of an SAP implementation — task by task, phase by phase — using 12 specialist AI agents embedded directly into the project workflow.

**Octiss is not a dashboard with tools. Octiss is a guided SAP Activate journey.**

### 1.2 One-Liner

> **Intelligence for Every Project. Clarity for Every Decision.**

### 1.3 Who It Is For

- SAP Project Managers managing S/4HANA implementations
- SAP Programme Managers overseeing multi-project programmes
- PMO Leads running governance across a portfolio of SAP projects
- System Integrator delivery teams needing structured SAP Activate guidance

### 1.4 What It Is Not

- Not a generic project management tool
- Not a chatbot — agents are contextual and task-triggered
- Not a SAP tool — sits alongside SAP Cloud ALM
- Not a WhatsApp integration — all communications via Outlook only

---

## 2. Methodology Coverage

| Code | Methodology | Status |
|---|---|---|
| GF | New Implementation (Rise with SAP) | ✅ Active |
| BF | System Conversion (Grow with SAP) | ✅ Active |
| SF | Selective Data Transition (Bluefield) | ⏸ Deferred |

**Discover phase excluded entirely. Octiss starts at Prepare.**

---

## 3. Task Architecture

### 3.1 Five-Level Hierarchy

| Level | Name | Source |
|---|---|---|
| 1 | Phase | SAP Activate — fixed |
| 2 | Deliverable Group | SAP Activate — fixed |
| 3 | Methodology Task | SAP Activate — fixed |
| 4 | Module Instance | Generated on project creation |
| 5 | Sub-Task / Activity | Generated on project creation |

### 3.2 Active Task Library

| Methodology | Active Tasks | Merge Tasks | Total |
|---|---|---|---|
| GF | 193 | 54 | 247 |
| BF | 201 | 36 | 237 |
| Total seeded | 394 active | 90 merge | 484 |

### 3.3 Task Categories

| Category | Owner | Behaviour |
|---|---|---|
| KEEP | PM | PM executes. Full task screen with agent CTA. |
| TRACK | Basis/IT/SAP | PM monitors. Overdue = auto RAID risk raised. ✅ Built |
| MERGE | PM | Consolidated into single task. |
| EXCLUDE | SAP Internal | Not included in Octiss. |

---

## 4. The 12 Specialist AI Agents

| # | Agent | Model | Role |
|---|---|---|---|
| 1 | SAP Activate Governance | Opus | Phase gates, quality gates, methodology compliance |
| 2 | Project Plan Agent | Opus | SOW extraction, conditional task activation, plan generation |
| 3 | Prepare Agent | Sonnet | Prepare phase task guidance |
| 4 | Explore Agent | Opus | FTS workshops, BPML, gap analysis, WRICEF |
| 5 | Realize Agent | Sonnet | Configuration, testing, TR management |
| 6 | Deploy Agent | Opus | Cutover, go-live readiness |
| 7 | Run/Hypercare Agent | Sonnet | Hypercare, issue triage, closure |
| 8 | Stakeholder Communication | Sonnet | Sign-offs, escalations, CRs, formal comms via Outlook |
| 9 | Meeting Intelligence | Sonnet | Workshop scheduling, MOM, attendance |
| 10 | RAID/Risk Agent | Sonnet | Risks, issues, TRACK overdue, escalation detection |
| 11 | Brownfield Conversion | Opus | BF conversion, custom code, SUM execution |
| 12 | Daily PM Briefing | Sonnet | 8AM daily briefing via n8n |

---

## 5. Project Creation Flow — LOCKED

```
PM clicks New Project
  ↓
STEP 1 — Document Upload (FIRST)
  SOW/Contract → DDA → Handover Document → Internal KO Document
  ↓
PATH A — Documents uploaded
  Claude Opus extracts all project fields
  ↓
CONFIRMATION SCREEN — PM reviews and corrects
  ↓
Project Plan Agent generates full 5-level plan

PATH B — No documents (fallback only)
  12 structured Q&A questions
  ↓
Project Plan Agent generates full 5-level plan
```

---

## 6. Features Built — 11 June 2026

### 6.1 Project Plan Export ✅ BUILT
- GET /api/v1/projects/{project_id}/plan/export/xlsx
- GET /api/v1/projects/{project_id}/plan/export/mpp
- Excel: openpyxl, TableStyleMedium2, frozen header, status colouring
- MPP: MS Project compatible .xml (native .mpp not feasible with Python libraries)
- Frontend: Export Excel + Export MS Project buttons with loading states

### 6.2 Calendar Mapping ✅ BUILT
- Auto-sync all project tasks with dates to Outlook Calendar via Graph API
- Task date updates → calendar entry updates automatically
- Task completion → calendar entry marked with ✓
- POST /api/v1/projects/{project_id}/calendar/sync
- Frontend: [Sync to Calendar] button on Command Center

### 6.3 Team Member Registry ✅ BUILT
- Extended existing team_members table
- GET/POST/PUT/DELETE /api/v1/projects/{project_id}/team-members
- Frontend: Add/edit form, organisation dropdown, sortable table, active toggle

### 6.4 Task-to-Template DB Mapping ✅ BUILT
- task_template_map table with activate_task_id → template_id mapping
- All 5 GF phases seeded: Prepare, Explore, Realize, Deploy, Run
- GET /api/v1/tasks/{task_id}/templates endpoint live

### 6.5 Auto-RAID from Overdue TRACK ✅ BUILT
- Overdue TRACK task → auto-creates Risk in RAID register
- Risk: Medium probability, High impact, System source
- Duplicate prevention built in
- POST /api/v1/projects/{project_id}/raid/auto-check

### 6.6 CR Generation Flow ✅ BUILT
- Project-scoped CR endpoints live
- WRICEF auto-trigger: exceeds SOW count → CR draft created automatically
- Agent 8 drafts CR email → PM approves → sends via Outlook (Graph API)
- Frontend: CR fields, [Send via Outlook] CTA, RAID page entry point

### 6.7 Q-Gate Interactive Screen ✅ BUILT
- 4 DB tables: qgate_checklist_items, project_qgate_items, qgate_signoff_drafts, qgate_phase_signoffs
- Three-panel layout: phase pills + summary | checklist items | evidence-based sign-off doc
- Real template: data/Q_Gate_Checklist_Octiss_Template.xlsx (4 phases, Prepare/Explore/Realize/Deploy)
- Phase unlock on customer sign-off
- GET/PUT /api/v1/projects/{project_id}/qgate/{phase}
- POST /api/v1/projects/{project_id}/qgate/{phase}/generate-draft

### 6.8 Attendance Management ✅ BUILT
- Existing session_attendance table extended
- POST/GET/PUT /api/v1/sessions/{session_id}/attendance
- Frontend: [Record Attendance] CTA on workshop/session tasks
- Attendance linked to Q-Gate evidence and MOM generation

### 6.9 BF Conversion Module ✅ BUILT
- Project-scoped BF endpoints
- SBX/DEV/QAS/PRD environment support
- Pre-SUM / SUM Execution / Post-SUM checklist phases per environment
- BF-only sidebar entry (shows only for BF methodology projects)
- Agent 11 CTA on all BF screens

---

## 7. Features Pending

### 7.1 Document Generation and Send to Team (NOT BUILT)

```
Task Screen CTA: [Generate Document]
  ↓
Agent pre-fills template with SOW-seeded context
  ↓
Document appears in RIGHT PANEL
PM reviews and edits inline
  ↓
[Send to Team] — PM selects recipient from Team Registry
Agent 8 drafts covering email
Sent via Outlook
Stored in OneDrive
n8n logs the send event
```

**What is missing:**
- Template rendering engine: template + task context → filled document
- Send-document CTA wired on Task Screen per state machine
- Document generation endpoint: POST /api/v1/tasks/{task_id}/generate-document
- n8n routing for document delivery logging

**Dependency:** task_template_map is now built (Prompt 05) ✅ — this unblocks document generation.

---

## 8. Core UX Layout — LOCKED

### 8.1 Default State — Two Panels

```
┌──────────┬────────────────────────────────────────┐
│  LEFT    │           CENTRE PANEL                 │
│  SIDEBAR │         (Task Working Area)            │
│  ~20%    │              ~80%                      │
└──────────┴────────────────────────────────────────┘
```

### 8.2 Triggered State — Three Panels

```
┌──────────┬─────────────────────┬──────────────────┐
│  LEFT    │   CENTRE PANEL      │   RIGHT PANEL    │
│  SIDEBAR │   (~45%)            │   (~35%)         │
│  ~20%    │   Task details      │   Generated doc  │
│          │   CTAs visible      │   Agent response │
│          │                     │   [X] dismiss    │
└──────────┴─────────────────────┴──────────────────┘
```

---

## 9. Task Screen CTA State Machine — LOCKED

CTAs are sequential — not a static list. Only the next required action shown at any time.

### Example — FTS Workshop

```
STATE 1: [Draft Communication]
STATE 2: [Schedule Session] — after comm approved
STATE 3: [Send Documents to Team] — after session scheduled
STATE 4: [Open Explore Agent] — after documents sent
STATE 5: [Generate MOM] — after session complete
STATE 6: [Mark Task Complete] — after MOM approved
```

---

## 10. WRICEF Change Management Flow — LOCKED

| Scenario | Trigger | Action |
|---|---|---|
| A — WRICEF Dropped | PM marks item dropped | No CR. Scope reduction logged. |
| B — Added within net count | Added = dropped count | Agent asks: notify customer or proceed silently? |
| C — Added exceeding net count | Active > SOW approved | Auto CR mandatory. Agent 8 drafts. PM approves. |

**Audit trail always preserved in WRICEF Inventory.**

---

## 11. Document Flow Principle — LOCKED

> **Agents give consultants a running start, not a blank page.**

- Agents pre-populate every template with maximum SOW and project context
- Consultant fills ONLY what requires on-the-ground knowledge
- Blank templates are NEVER sent

---

## 12. Template Library — COMPLETE (11 June 2026)

**All 87 delivery templates built and saved.**

| Phase | GF + Shared | BF Only | Total |
|---|---|---|---|
| Prepare | 15 | 4 | 19 |
| Explore | 14 | 8 | 22 |
| Realize | 13 | 9 | 22 |
| Deploy | 9 | 4 | 13 |
| Run | 7 | 2 | 9 |
| Cross-phase | 2 | 0 | 2 |
| **Total** | **60** | **27** | **87** |

**Location:** D:\Mohsin Personal\OneDrive\EM Intelligence Lab\OCTISS OFFICIAL TEMPLATES\
**Registry:** Octiss_Template_Library_Registry_v1.0.xlsx

---

## 13. Pending Features — Complete List (v1.9)

| Priority | Feature | Status |
|---|---|---|
| 1 | Issue 10 — Task descriptions generic | 🔴 Audit needed |
| 2 | E2E verification — all 16 screens | 🔴 Not done |
| 3 | Document Generation and Send to Team | 🔴 Not built |
| 4 | Q-Gate seeder verification | 🟡 Template in repo — needs live test |
| 5 | Phase gate unlock E2E | 🟡 Built — needs live test |
| 6 | Schedule Session → Outlook E2E | 🟡 Not verified |
| 7 | Auto-RAID E2E live test | 🟡 Built — needs live test |
| 8 | CR generation flow live test | 🟡 Built — needs live test |
| 9 | Beta tester prep | 📋 On hold |
| 10 | Group J flowchart PPTX (31st) | 📋 Pending |
| 11 | UI/UX design pass | 🔒 Deferred — last step |

---

## 14. What Is Excluded from Octiss

- Discover phase
- WhatsApp — removed entirely
- SAP Cloud ALM setup tasks
- ECT/Tricentis — excluded by default
- SAP employee-only tasks
- Digital Twin — post-revenue
- Multilingual support — post-M11
- SF (Bluefield) — deferred
- SAP Upgrade methodology — deferred

---

## 15. MS365 Configuration

| Item | Value |
|---|---|
| Azure App | Octiss Production |
| Client ID | 3a027ac4-a5f3-452c-b283-42a29c8be118 |
| Permissions | User.Read, Files.ReadWrite, Files.Read.All, Sites.Read.All, Mail.Send, Calendars.ReadWrite, offline_access |
| Status | All granted + admin consent applied |

---

## 16. Production Environment

| Service | URL / ID | Status |
|---|---|---|
| Backend (Railway) | sap-pmo-agent-production-3f52.up.railway.app | HEALTHY |
| Frontend (Vercel) | octiss-production.vercel.app | LIVE |
| Database (Supabase) | qrxfprybbpqugptakeke — Singapore Pro | ACTIVE |

---

## 17. Decisions Log (v1.9 Additions)

| Date | Decision |
|---|---|
| 11 Jun 2026 | All 12 Codex prompts executed. Tests 1235→1311 (+76). Both repos pushed clean to origin/main. |
| 11 Jun 2026 | Q-Gate checklist template added to backend repo at data/Q_Gate_Checklist_Octiss_Template.xlsx. 4 phases: Prepare (166 rows), Explore (148 rows), Realize (178 rows), Deploy (149 rows). Seeder reads Col 3 == "Question" rows only — skips section headers. |
| 11 Jun 2026 | MPP export confirmed as MS Project compatible .xml — native .mpp not feasible with Python libraries. Acceptable for production use as MS Project opens .xml natively. |
| 11 Jun 2026 | SUD v1.8 Section 16 item 17 (Build 34 new templates) removed — all 87 templates were completed on 09 Jun 2026. Section 16 now reflects accurate pending state. |
| 11 Jun 2026 | .gitignore updated on both repos to ignore Codex worktree files (-MohsinSardar-Khi-SAPPMO* pattern). |
| 11 Jun 2026 | Windows on development machine is in Notification mode. OneDrive sync paused. TMC IT must activate Windows. Files safe on D: drive locally. |

---

*End of Document — Octiss SUD v1.9 — 11 June 2026*
*Supersedes: Octiss_SUD_v1.8.md*
*Read alongside: Octiss_Master_Handoff_v1.2.md*
*Next update: v2.0 after E2E verification complete and Document Generation feature built*
