# Octiss - Solution Understanding Document (SUD)
**Version:** v2.8 | **Date:** 18 June 2026 | **Status:** LIVE
**Owner:** Mohsin Sardar - EM Intelligence Labs
**Classification:** Confidential - Internal Use Only

> **PURPOSE:** Single source of truth for what Octiss should be and why.
> Every Claude session and every Codex prompt must read this document first.

---

## Changelog

| Version | Date | Changes |
|---|---|---|
| v1.0-v1.8 | 01-09 Jun 2026 | See previous SUD versions |
| v1.9 | 11 Jun 2026 | 12 Codex prompts. Tests 1235->1311. Both repos pushed. |
| v2.0 | 11 Jun 2026 | 26 total Codex prompts. Full feature build complete. E2E started. |
| v2.1 | 12 Jun 2026 | Full E2E completed (all 16 screens). CF-5 through CF-20 applied. New screens: RAID, WRICEF, CR, BF Conversion. New locked decisions: Partner terminology, Stakeholder architecture, WBS numbering, CTA intelligence. |
| v2.2 | 13 Jun 2026 | Full fix session. All 29 E2E issues addressed. CF-19d through CF-40 applied. New: Stakeholder screen, Calendar scheduling, Q-Gate 518 items, CSV import, react-markdown, WBS fix. Task count module-dependent. Q-Gate auto-initializes per project. |
| v2.3 | 14 Jun 2026 | Wizard + Task Screen pre-beta session. 12 frontend + 2 backend commits. New: CSV/Excel team import (wizard), Generate Project Plan CTA, Document Return Loop, wizard stakeholders persisted to team_members. New locked decisions: 3 implementation types, 4 role types, DD-Mon-YYYY dates, consolidated document CTA, Phase Gate Pack + Monitor/Escalate visibility rules. DB fixes: project_tasks.description, organisation constraint (SI Firm -> Partner), 17-member backfill. E2E screens 1-10 re-verified. |
| v2.4 | 15 Jun 2026 | Massive expansion session. Full E2E completed (all 16 screens PASS). 33 E2E issues fixed across 5 batches. CR-1 full rebuild (7-stage lifecycle, 6 types). WRICEF FSD bundle (6 types, full loop, exceptions). All 11 agent system prompts upgraded from generic to purpose-built SAP Activate specialists. 5 new agents (13-17) built full-stack. 4 new tracking screens (Documents/DataMig/Commercial/Training). 4 new intelligence features: Proactive Alerts (hourly n8n), Auto Status Report (weekly n8n), Pre-Meeting Pack, Health Dashboard. 10 new/changed DB tables. 17 total agents. 32+ new backend endpoints. Latest commits FE 52a8e5b / BE 18b3fdd. |
| v2.4 (cont.) | 15 Jun 2026 (afternoon/evening) | Continuation session. **CR-2** (Impact Analysis + CCB + CRF .docx, 19 new columns, AI Suggest Impact). **Smart Alert Actions** (context-aware buttons per alert type; "Noted" / "Mark resolved" with mandatory resolution_reason; RAID risk highlight). **E-01/02/05/06/10** all COMPLETE. **SOW module extraction fix** (SAP codes not categories). **7 critical bug fixes** incl. WC-table permission grants and a duplicate-MONTH_ABBR build freeze (prod was stuck on E-02 bundle). **Project Alpha** test project built/configured (12 SAP-code modules, 18 WRICEF, 15 team members); DB cleaned to 2 projects. Full E2E re-run on Project Alpha is the next objective. Latest commits FE 31d27a8 / BE a6a0519. See §22 (continuation features), §23 (bug fixes), §24 (Project Alpha + E2E + Automation Roadmap). |
| v2.5 | 16 Jun 2026 | **E2E session (Project Alpha).** Screens 1–4 verified. **Task Assignment Engine** (deterministic, no LLM; module normalization; 245/246 assigned). **5-stage Escalation Engine** (draft-only mailto, n8n hourly). **Template Library integration** (87 templates; `[bracket]`+`{{brace}}` placeholder fill; .docx/.xlsx/.pptx). New tables **communication_drafts**, **task_escalations**; **project_tasks** assignment columns + **assigned_to FK corrected** (auth.users → team_members); **session_attendance** missing columns added. CORS Content-Disposition exposed. Command Center KPI fixes (D-10/D-11), Settings role label (S-07), task-list assignee names (T-24) + module labels (T-28), PM Inbox Email Drafts. Latest commits FE e447e1f / BE 0197bf5. See §25 (Architecture Decisions — June 2026). |
| v2.6 | 17 Jun 2026 | T-59 fully closed (5 FE commits + 1 BE). Evidence Intelligence Layer: task_evidence + project_storage_config + project_email_config tables, OneDrive/MS365 Graph API providers, 4 evidence endpoints. 246 GF task descriptions rewritten (senior PM quality, markdown format, role placeholders). Task Detail page redesigned: TaskHeaderMeta pills, WhoToContact, PMEscalationPath, EvidenceSection checkboxes, SapActivateRef collapsed pill, agent auto-brief removed, Ask Phase Agent opt-in. Step 7 right panel consolidation deferred. Latest commits FE e26957a / BE eacac03. |
| v2.7 | 17 Jun 2026 | Task Detail full redesign complete (5 commits, c816a00). 5-tab layout: Overview (collapsible sections) / Team & Escalation (contacts + escalation ladder + TRACK) / Task Checklist (evidence checkboxes) / Documents (return loop) / Notes (PM notes only). Editable header pills: planned/actual dates + assignee with auto-save. Right panel Overview-only. Review Document CTA replaces Ask Phase Agent. n8n Evidence Intelligence (Hourly) workflow published. Latest FE c816a00 / BE eacac03. |
| v2.8 | 18 Jun 2026 | WRICEF Intelligence Layer complete: wricef_session_notes table, FSD Agent reads session notes, Meeting Intelligence extract_requirements(), Session Intelligence block in FsdPanel, summary bar, sessions badge, FSD lifecycle 6 stages, Create FSD Template rename, filename fix (WRICEF-XXX format), stage labels fixed (Upload Consultant FSD / PM Review), WRICEF sorted W-R-I-C-E-F. Document Filing Service: provider-abstracted auto-filing, 20-folder project structure, OneDrive/GDrive/Dropbox providers. Agent Architecture v1.1: Digital Twin Teams+GoogleMeet Phase 1, Whisper large-v3, evidence-based document review, MOM approval flow. Security: all 95 tables RLS enabled. Latest FE 42bb2d2 / BE cd9fcac. |
| v2.9 | 18 Jun 2026 | E2E Session. Screen 6 CR GREEN: 7-stage lifecycle, AI Suggest 7 fields (Agent 7 endpoint), CRF Octiss-branded 7-page .docx (role_type fix, org format match). Screen 7 RAID rebuilt: risk save fix (CHECK constraint m10bc), risk_category/risk_owner/risk_status added, Issues register (project_raid_issues), Decisions register (project_raid_decisions), Actions enhanced, agentic pre-fill on all 4 tabs. CR Intelligence Layer designed (SOW baseline = existing tables, SCOPE_DRIFT alert, 20/28 auto-fill). AI-Driven RAID Intelligence Layer designed: Engine 1 Evidence Scanner, Engine 2 Downstream Impact Mapper, Engine 3 Intelligence Feed Panel. Configurable Escalation Thresholds designed (intelligence_config JSONB). Latest FE 4198981 / BE c3ebeb6. |
| v3.0 | 20 Jun 2026 | E2E Session 2. Screens 8-13 GREEN. Q-Gate: category grouping (m10bf), evidence intelligence, AI Review button, mandatory-only sign-off gate. Documents: 34-doc pre-loaded register (project_document_register, NOT project_documents=RAG), document_seed.py, m10bg. Data Migration: full rebuild (m10bh), AI suggest objects, cutover readiness 0-100, RAID auto-linkage, 6 objects seeded. Commercial: full intelligence rebuild (commercial_intel.py), auto health GREEN/AMBER/RED, WRICEF protection tracker, revenue exposure, payment milestones, CR revenue tracker, T&M burn, status report integration, no new tables. Training: Train-the-Trainer wave model (m10bj), Wave 1 Partner→BPO + Wave 2 BPO→EndUser, AI suggest groups, readiness scoring 0-100, two gates, RAID auto-link, status report integration, training_intel.py, Actions panel (no chat). Notifications: all alert types firing (WRICEF/Task/Phase/Training). Global: agent panels → Actions panels rule, table discipline enforced, DD-MMM-YYYY date standard. FE 911fc8c / BE 6c1ad8a. |
| v3.1 | 20 Jun 2026 | E2E Session 3. Screens 14-16 GREEN — **ALL 16 testable screens GREEN** (Screen 17 BF Conversion pending a BF Test Project). Health Dashboard: 8-dimension health engine (Customer/Scope/Schedule/RAID/Commercial/Training/Migration/Document) + weighted overall score, trend, phase progress, top actions (m10bk, extends project_health_snapshots). **SteerCo pack regenerated as a 7-slide PowerPoint (.pptx) via Node.js/pptxgenjs** (CHANGED from python-docx): generate_steerco.js at backend root, project_health.py steerco-pack endpoint replaced, Dockerfile + Node.js 20 + pptxgenjs. Team Members: edit/add/delete (gap TM-G1 edit scrolls to top). Calendar/Inbox: Month/Week/Day views + Schedule Session modal + Meeting Pack (gap CAL-G1 date format mm/dd/yyyy). Post-E2E: **RAID Intelligence Layer Engine 1 deployed** — alert_service.py +5 checks (RAID_RISK_NO_MITIGATION, RAID_ISSUE_STALE, RAID_DECISION_STALE, RAID_WRICEF_UNPROTECTED, RAID_MILESTONE_BLOCKER), PM-configurable thresholds via intelligence_config JSONB on projects, firing live on Alpha. Next: Engine 2 Downstream Impact Mapper, Engine 3 RAID Intelligence Feed Panel, CR Intelligence Layer, post-E2E gap sweep, Screen 17. FE e77cc95 / BE 6a7a992 (+ Session 3 follow-on commits). |
| v3.2 | 20-Jun-2026 | Intelligence Sprint + Storage Intelligence (4b) complete: RAID Engines 1+2+3 (Evidence Scanner / Downstream Impact Mapper / Intelligence Feed Panel), CR Intelligence Layer (CR_SCOPE_DRIFT alert + /change-requests/prefill endpoint + Detect Scope Drift button), Storage Intelligence Layer (9-folder per-project structure + doc-type routing + B-7 verification, extends document_filing_service.py, m10bl). Schema: risks.impact_map JSONB, project_wricef.cr_ref TEXT, projects.onedrive_root_url, project_document_register.onedrive_verified + onedrive_verified_at. Bug fix: _safe_rows() for missing project_change_requests.wricef_refs. FE 40c10b0 / BE 3cce2ac. |

---

## 1. Product Vision

Octiss is an AI-powered SAP PMO Intelligence Platform. It guides SAP Project Managers through every phase of an SAP implementation - task by task, phase by phase - using **17 specialist AI agents** embedded directly into the project workflow, backed by a proactive intelligence layer (alerts, weekly status reports, pre-meeting packs, customer-health + scope-creep monitoring).

**One-Liner:** Intelligence for Every Project. Clarity for Every Decision.

---

## 2. Methodology Coverage - LOCKED

| Code | Implementation Type | Label | Status |
|---|---|---|---|
| greenfield_rise | New Implementation (Rise with SAP) | Private Cloud | Active |
| greenfield_grow | New Implementation (Grow with SAP) | Public Cloud | Active |
| brownfield | System Conversion | Brownfield | Active |
| SF | Selective Data Transition | — | Deferred |

Both greenfield_* types route to GF task generation. brownfield routes to BF.

---

## 3. Project Creation Flow - LOCKED

### Path A (PRIMARY - SOW Upload)
```
Upload SOW (.pdf/.docx/.doc) -> Claude Opus extracts scope ->
Confirmation screen -> Project Plan Agent generates module-dependent tasks ->
Q-Gate auto-initializes -> Phase View (Prepare).
```

### Path B (FALLBACK - Manual Q&A)
```
5-step wizard: Basics -> Modules -> Storage -> Team/Stakeholders -> Review.
All fields start BLANK. Project code auto PRJ-{timestamp}.
  Step 2: dates DD-Mon-YYYY (clickable picker).
  Step 3: MS365 defaults pre-selected; phase folder fields REMOVED (root only).
  Step 4: CSV/Excel import; Role Type (4 values); Organisation Customer/Partner;
          persisted to team_members on create.
  Step 5: review shows module names + team members (Name-Role(Module)).
-> Project Plan Agent -> Q-Gate auto-init -> Phase View (Prepare).
```

### Recovery Path - Generate Project Plan CTA
If a project has zero tasks, the Command Center shows a "Generate Project Plan" button that calls POST /api/v1/project-plan/generate and reloads the task list. The empty Command Center also shows a "Create Your First Project" CTA.

### Task Count - By Design (LOCKED 15 Jun 2026)
193 GF / 201 BF are the BASE counts for a standard project. Multi-module projects correctly generate MORE tasks — Fit-to-Standard workshops, UAT sessions and training delivery expand per module. This is correct behaviour, not a bug (E2E-53 closed as by design).

### Mock Test Files (Desktop) — Project Alpha (current canonical test project)
- `Project_Alpha_SOW_v1.0.docx` — GF Rise/Private Cloud, **12 SAP-code modules**, 18 WRICEF, 4 integrations, 15 data migration objects
- `Project_Alpha_Team_Registry_v1.0.csv` — **15 team members** (correct SAP module codes)
- Project Alpha ID `b2e3f098-e637-4e7b-9369-5de84ad354b7` — full detail in §24.

---

## 4. Stakeholder Architecture - LOCKED

The Stakeholder table is the intelligence layer driving agent behaviour and the task screen dropdowns.

| Field | Values | Purpose |
|---|---|---|
| Full Name | Free text | Identity |
| Email | Free text | Communication routing |
| Module Assignment | Project modules + PMO | Task/agent assignment |
| Role Type | Project Manager / Consultant / BPO / Steering Committee Member | Communication level |
| Organisation | Customer / Partner | Determines document routing |

- Wizard Step 4 stakeholders persisted to `team_members` on create (module_id resolved).
- Registry drives task screen **Assigned To**, **Responsible Person**, **Send Document To**, and the CR **Raised By** dropdowns (shown as Name - Role / Name—Module).
- Module display fixed (Batch 4): "CODE — Name" or just "CODE"; PMO replaces "Project / Cross Module"; no "MM - MM" duplicates.
- Role Type enforced as a 4-value dropdown (default Consultant).

### Agent Routing Rules
- Meeting Intelligence: Partner Consultants for workshops, Customer BPOs for sign-offs
- Stakeholder Communication: Phase sign-off -> Customer SteerCo, MOM -> all attendees
- RAID Agent: escalation path uses Customer PMgr vs Partner PMgr
- Q-Gate Sign-off: Customer Steering Committee Member or Customer PMgr signs
- CR Generation: Customer PMgr or SteerCo approves based on impact level
- FSD routing: Consultant = Partner member by module; Customer = Customer Project Manager

---

## 5. Terminology - LOCKED

| Old Term | New Term | Scope |
|---|---|---|
| SI Firm | Partner | All display labels, dropdowns, agent outputs, AND the team_members.organisation DB value |
| SI Firm Name | Partner Name | All form fields |

As of 14 June 2026 the team_members.organisation CHECK constraint allows Customer / Partner only; existing "SI Firm" rows were migrated to "Partner".

---

## 6. Agent Behaviour - LOCKED

### Agent Tone Rules
```
RULE 1 - Agents speak DIRECTLY to PM in second person
RULE 2 - System prompts NEVER visible to PM
RULE 3 - Follow-up responses skip context repetition
RULE 4 - Agents act FIRST, PM decides
```

### System Prompt Architecture (15 Jun 2026)
```
Batch 3B audit found all 11 agents were 9-line empty subclasses sharing ONE
generic prompt with zero phase/document/SAP knowledge.
Batch 3C: every agent now overrides _system_prompt() with:
  - Phase-specific SAP Activate knowledge
  - Owned-document list with quality review criteria
  - Section-level review checklists
  - Second-person PM tone
  - Specific, actionable guidance
Composition: RICH_PROMPT + super()._system_prompt()
  -> preserves the JSON output schema and the PMgr-review-only safety rails.
Agents 4 (Explore) + 6 (Deploy): confirmed Sonnet (locked — speed over Opus).
Agent 8 (Stakeholder): prose JSON schema { subject, body, tone, action_required, deadline }.
New agents 13-17: all claude-sonnet-4-6, SpecialistAgentBase pattern.
Filenames preserved (dynamic importlib pattern).
```

Per-agent highlights: Agent 1 phase-gate criteria all 5 phases + quality-gate scoring; Agent 3 Charter review + 7-point gate readiness; Agent 4 BPML + FSD review for all 6 types + 10-point Explore gate; Agent 5 SIT (zero critical before UAT) + UAT review; Agent 6 Go/No-Go 10-point + cutover review; Agent 7 hypercare exit 6-point + closure; Agent 8 3 comm tiers + 5 comm types; Agent 9 4 workshop types + MOM 6-point; Agent 10 PI methodology + escalation thresholds + WRICEF risk flags; Agent 11 ATC P1/P2/P3 + Pre/Post-SUM + SBX->DEV->QAS->PRD + downtime calc; Agent 12 health rubric + 7-section briefing + focus_today 3 actions.

### CTA Intelligence Rules
```
RULE 1 - Workshop/Session tasks (now also matches Fit-Gap):
  NOT STARTED: Schedule Session, Fit-Gap Session Pack
  IN PROGRESS: Session Attendance T-5/T-3/T-1, Open Meeting Intelligence Agent
  COMPLETE: Generate Document (MOM)
RULE 2 - Governance tasks (now matches Gate/Sign-Off/Governance/SteerCo/Steering):
  NOT STARTED: Generate & Review Document, Draft Review Email
  IN PROGRESS: Customer Sign-Off Request, Phase Gate Pack
  COMPLETE: SteerCo Deck Preparation
RULE 3 - Technical tasks (now matches Basis/ABAP):
  Show: Generate & Review Document, Draft Review Email
RULE 4 - Always show: Open [Phase] Agent CTA
RULE 5 - Default: Generate & Review Document, Open [Phase] Agent
Chat token budget: 1500 (cut from 2600). Loading copy "Agent is analysing your task...".
```

---

## 7. The 17 Specialist Agents

| # | Agent | Model | Role |
|---|---|---|---|
| 1 | SAP Activate Governance | Opus | Phase gates, quality gates |
| 2 | Project Plan Agent | Opus | SOW extraction, task generation (base prompt retained) |
| 3 | Prepare Agent | Sonnet | Prepare phase guidance |
| 4 | Explore Agent | Sonnet | FTS, BPML, gap analysis, WRICEF (locked Sonnet) |
| 5 | Realize Agent | Sonnet | Configuration, testing |
| 6 | Deploy Agent | Sonnet | Cutover, go-live (locked Sonnet) |
| 7 | Run/Hypercare Agent | Sonnet | Hypercare, closure |
| 8 | Stakeholder Communication | Sonnet | Sign-offs, CRs, Outlook (prose JSON schema) |
| 9 | Meeting Intelligence | Sonnet | Workshops, MOM, attendance |
| 10 | RAID/Risk Agent | Sonnet | Risks, issues, escalation |
| 11 | Brownfield Conversion | Opus | BF conversion, SUM execution |
| 12 | Daily PM Briefing | Sonnet | 8AM briefing via n8n |
| 13 | Document Generation Agent | Sonnet | Knows all 87 Octiss templates; generates any document on demand; tracks document register; reviews documents for completeness before Customer |
| 14 | Change Impact Agent | Sonnet | On CR raised/approved, analyses impact on tasks/WRICEF/timeline/budget; structured Change Impact Assessment; suggests task add/modify/remove; drafts notifications |
| 15 | Data Migration Agent | Sonnet | Tracks migration objects across SBX/DEV/QAS/PRD; monitors pass rates + error counts; flags data quality before production; advises readiness; migration sign-off docs |
| 16 | Commercial/Contract Agent | Sonnet | Monitors scope vs SOW; detects unprotected WRICEF scope creep; T&M burn vs budget; alerts when scope exceeded without CR; drafts commercial comms (sources: SOW scope, CR register, WRICEF register, task plan, team registry) |
| 17 | Training and Adoption Agent | Sonnet | Tracks training completion per user group; identifies adoption gaps; gap analysis; monitors post go-live usage; drafts comms to department heads |

---

## 8. Proactive Intelligence (NEW — 15 Jun 2026)

### Proactive Alerts (WC-2)
project_alerts table (indexes on project_id, status, severity). AlertService — 7 evaluators run hourly via n8n (`proactive_alerts_hourly`, LIVE):
```
TASK_OVERDUE             CRITICAL if >7 days, HIGH if 1-7 days
PHASE_GATE_AT_RISK       gate task due within 14 days
SIGN_OFF_PENDING         doc in review >3 days (CRITICAL >5)
WRICEF_OVER_BUDGET       WRICEF count > SOW budget
CRITICAL_RISK_UNMITIGATED PI>=20 with no response plan
HIGH_RISK_ESCALATION_DUE PI 16-19 escalation due
FSD_OVERDUE              FSD Generated/Sent for >7 days
Dedup: same alert_type + affected_item_id -> update existing ACTIVE alert
       (ACKNOWLEDGED treated same as ACTIVE). No duplicate rows.
Endpoints (5): list / evaluate / acknowledge / resolve / evaluate-all (n8n-key guarded).
ACKNOWLEDGE = PM seen it (still active). RESOLVE = PM fixed it (no longer active).
Email notifications = Phase 2 (not built).
```
Notifications screen rebuilt: severity-grouped (CRITICAL/HIGH/MEDIUM/LOW), alert cards with recommended-action box, Acknowledge + Resolve per card, auto-refresh every 5 minutes, green "All clear" empty state. Command Center banner: RED if CRITICAL, AMBER if only HIGH; links to Notifications.

### Weekly Status Report (WC-3)
project_status_reports table (UNIQUE project_id+report_date). StatusReportService pulls task completion, RAID/CR/WRICEF summaries, milestones, pending sign-offs; auto-computes RAG (overall + per workstream); Claude Sonnet writes executive summary, progress this period, key risks/issues, next period focus, decisions required; upserts by project+date. Endpoints (4): generate / latest / list / generate-all. Status Report screen (WORKSPACE): RAG dashboard + 8 sections + Send via Outlook mailto + Generate/View History. n8n Monday 08:00 (`weekly_status_report`, LIVE). No auto-send — PM reviews first.

### Pre-Meeting Intelligence Pack (WC-4)
project_meeting_packs table. MeetingIntelligenceService: RAG per workstream, top 5 risks by PI, decisions required (CRs + sign-offs >5 days), progress summary, pending approvals, upcoming milestones (30 days), open overdue actions (top 10); Claude Sonnet writes opening brief, what went well, what needs attention, key ask, 3 talking points, watch out for. Endpoints: generate / list / latest. Meeting Pack screen (WORKSPACE): type selector (SteerCo/Customer PM/WorkCom/Go-Live Readiness/General), date picker, 10 sections, Print button. Pre-Meeting Pack CTA added to Calendar header.

### Customer Health Score + Scope Creep (WC-5)
project_health_snapshots table (UNIQUE project_id+snapshot_date). HealthService:
```
Customer Health Score (0-100, 4 dimensions x 25pts):
  Engagement Score (25): long-pending-docs penalty
  Sign-off Velocity (25): <3d=25, <7d=20, <14d=15, <21d=10, >21d=5
  Escalation History (25): critical-alerts penalty
  CR Acceptance (25): approval-rate scoring
  Grade: GREEN >=80 / AMBER >=60 / RED <60. AI text insight per tier.
Scope Health (GREEN/AMBER/RED):
  GREEN within SOW budget; AMBER over budget but all CR-covered;
  RED unprotected (variance > CR-covered). FSD completion rate. Scope alerts w/ links.
Unprotected WRICEF = WRICEF variance minus CR-covered count.
Endpoints (4): calculate / latest / history / calculate-all.
  Router named project_health.py (avoids clobbering existing app health.py).
```
Health Dashboard screen (WORKSPACE): two-column — left circular score + grade + 4 breakdown bars + insight + detail stats; right scope status card + WRICEF tracker (SOW/current/variance/UNPROTECTED bold-red) + FSD completion bar + scope insight + alerts; trend vs prior snapshot. PM must enter SOW WRICEF budget on Commercial screen for scope health to compute.

---

## 9. Change Requests — CR-1 Rebuild (NEW — 15 Jun 2026)

```
CR-NNN auto-numbering per project.
7-stage lifecycle with strict transitions:
  New -> Initiated | Rejected
  Initiated -> In Progress | Rejected
  In Progress -> Approved | Rejected | On-Hold
  On-Hold -> In Progress | Rejected
  Approved -> Implemented (terminal after)
  Rejected -> terminal
6 CR types: Scope / Schedule / Budget / Resource / Technical / WRICEF.
Priority High/Medium/Low (coloured pills). Impact Area multi-select from modules.
Raised By: team_members registry (Name—Module display; auto-fills name/email/org).
Change Status modal: lifecycle-restricted transitions, Decision By dropdown,
  rejection reason mandatory, CCB escalation toggle (irreversible once set).
CR detail slide-in panel (40%): Impact Analysis inline edit, rejection reason box.
Filter bar: Status / Type / Priority / Search.
14 new columns on project_change_requests; CHECK constraints relaxed to title-case.
Root cause fixes: priority/status/change_type CHECK constraints; toast z-index portal
  (createPortal to body, z-[99999]); Raised By switched /team -> /team-members.
```
**CR-2 (Impact Analysis + CCB + CRF) is now COMPLETE** — see §22.

---

## 10. WRICEF FSD Lifecycle (Enhancement Bundle — 15 Jun 2026)

```
6 FSD Octiss templates: data/OCTISS OFFICIAL TEMPLATES/FSD_*_Octiss_Template.docx
  (W=Workflow, R=Report, I=Interface, C=Conversion, E=Enhancement, F=Form)
Per-WRICEF FSD lifecycle:
  Generate FSD CTA per row -> fsd_status badge -> FSD Document Panel (~35% slide-in)
  Type-aware template selection + pre-fill (Project Name/Code/Customer/PM names/
    WRICEF ID, "SI Firm" -> "Partner" throughout)
  Send to Consultant (Outlook mailto, Partner member by module)
  Upload Reviewed FSD -> Agent type-specific quality review (6 checklists + cross-type)
  Verdict: READY TO SEND TO CUSTOMER / NEEDS REVISION
  Per-section exception: PM reason mandatory (min 10 chars); internal to Octiss only,
    NEVER written to document; fsd_exceptions JSONB; fsd_sent_with_exceptions amber badges
  Approve & File -> OneDrive Signed Off -> Evidence Captured badge
10 new fsd_* columns on project_wricef.
fsd_status values: Not Started | Generated | Sent | Needs Revision | Ready to Send |
  Approved & Filed.
```

---

## 11. Task Management Rules - LOCKED

```
Task count: module-dependent (193 GF / 201 BF base; multi-module expands — by design).
Delete Task: REMOVED ENTIRELY - Exclude Task only.
WBS numbering: {Phase}.{DeliverableGroup}.{Task} (Prepare=1..Run=5).
Task dates: DD-Mon-YYYY everywhere.
Role Type: exactly 4 values (Project Manager, Consultant, BPO, Steering Committee Member).
Assigned To / Responsible Person: team member dropdown from registry.
Phase Gate Pack card: only on gate/governance tasks.
Monitor & Escalate panel: only when overdue or escalation triggered.
Notes field: placeholder includes 4 real example notes.
```

---

## 12. Document Flow - LOCKED

```
Three-Tier Field System:
  Tier 1 (black): Pre-filled by Octiss
  Tier 2 (navy []): Consultant fills content
  Tier 3 (grey italic): Guidance - deleted before submitting
Document Lifecycle (Return Loop):
  Not Started -> Generated -> Sent for Review -> Received -> Approved & Filed
  1. Generate & Review Document (single CTA)
  2. Send via Outlook (recipient from team_members registry)
  3. Upload Reviewed (.docx/.xlsx/.pdf) -> Received
  4. Review Uploaded Document - agent returns PASSED / NEEDS REVISION
  5a. Approve & File -> OneDrive Signed Off + Evidence Captured
  5b. Request Revision -> back to Sent for Review with feedback
OneDrive structure: {Root}/{Phase}/{In Progress | Review | Signed Off}
```

---

## 13. Core UX Layout + Sidebar - LOCKED

### Panels
```
Default: LEFT SIDEBAR (~20%) | CENTRE (~80%)
Triggered: LEFT (~20%) | CENTRE (~45%) | RIGHT PANEL (~35%, agent/document)
```

### Sidebar Navigation (current)
```
ACTIVE PROJECT
  [Project Switcher]
SAP ACTIVATE PHASE TRACKER
  Prepare / Explore / Realize / Deploy / Run
WORKSPACE
  Command Center
  Notifications      (rebuilt — proactive alerts + smart actions; /notifications)
  Drafts             (NEW — Review Drafts at /inbox, unread badge)
  Board
  RAID Register
  Q-Gate
  Stakeholders       (+ Mendelow 2x2 Engagement Matrix)
  Calendar
  Status Report      (NEW — weekly auto-report)
  Meeting Pack       (NEW — pre-meeting intelligence)
  Health Dashboard   (NEW — customer health + scope)
SCOPE
  WRICEF             (FSD lifecycle per item)
  Change Requests    (fully rebuilt CR-1)
  BF Conversion      (BF projects only)
  Documents          (NEW — document register + Agent 13)
  Data Migration     (NEW — 4-env tracker + Agent 15)
  Commercial         (NEW — SOW vs actual + Agent 16)
  Training           (NEW — user group tracker + Agent 17)
  Team Members
Settings / PMgr / Sign out
```

---

## 14. Features Built - Complete List (v2.4)

### Core Infrastructure
- Complete: Module-dependent task generation
- Complete: 17 specialist agents live (12 core + 5 new); all 11 specialist agents have purpose-built system prompts
- Complete: 87 delivery templates complete
- Complete: MS365 fully connected
- Complete: n8n workflows live (incl. hourly alerts + Monday status report)
- Complete: WBS numbering (Phase.Group.Task) · react-markdown agent rendering

### Project Management
- Complete: Project creation Path A (SOW upload + Claude Opus)
- Complete: Project creation Path B (manual wizard, phase-folder fields removed)
- Complete: Implementation type - 3 options · DD-Mon-YYYY dates
- Complete: CSV/Excel team import (wizard Step 4) · stakeholders persisted on create
- Complete: Generate Project Plan CTA + empty-state "Create Your First Project" CTA
- Complete: Phase View (deliverable grouping + WBS) · Task Screen (context-aware CTAs)
- Complete: Document Return Loop · RAID Register (5-point PI + AI pre-fill)
- Complete: Board (Kanban) · Calendar (task view + session scheduling + Outlook mailto)

### Proactive Intelligence (NEW)
- Complete: Proactive Alert System (7 evaluators, hourly n8n, Notifications + CC banner)
- Complete: Auto Weekly Status Report (Monday 8AM n8n, Status Report screen)
- Complete: Pre-Meeting Intelligence Pack (Meeting Pack screen + Calendar CTA)
- Complete: Customer Health Score dashboard
- Complete: Scope Creep Detection dashboard

### New Specialist Agents (NEW)
- Complete: Agent 13 Document Generation (87 templates)
- Complete: Agent 14 Change Impact
- Complete: Agent 15 Data Migration (4 environments)
- Complete: Agent 16 Commercial/Contract
- Complete: Agent 17 Training and Adoption

### Scope Management
- Complete: WRICEF Inventory + full FSD lifecycle (6 types, exceptions)
- Complete: FSD type-specific agent review (6 checklists)
- Complete: Change Requests full rebuild (CR-1; CR-NNN; 7-stage; 6 types)
- Complete: BF Conversion Module (SBX/DEV/QAS/PRD, Pre/SUM/Post)
- Complete: Documents screen · Data Migration screen · Commercial screen · Training screen

### Document Management
- Complete: Document Register screen (Agent 13)
- Complete: FSD generation from 6 Octiss templates · FSD review loop
- Complete: Document generation/rendering (.docx + .xlsx) · MOM auto-generation
- Complete: Send via Outlook · OneDrive phase/status folders

### Quality Gates
- Complete: Q-Gate Interactive Screen + canonical checklist auto-init (518 items)
- Complete: Q-Gate sign-off doc generation · Phase gate customer sign-off via Outlook

### Team & Stakeholders
- Complete: Stakeholder screen + Team Member Registry
- Complete: Role Type 4-value dropdown enforced · module display fix (PMO, no duplicates)
- Complete: CSV/Excel import · Partner terminology (display + DB) · persisted on create

### Exports
- Complete: Project Plan Export (.xlsx; columns realigned to locked spec; owner from registry; phase dates from project dates)
- Complete: Calendar Mapping (Graph API) + Schedule Session modal (shared component)

### E2E Session — 16 June 2026 (NEW)
- Screen 1 Login: 6/6 PASS
- Screen 2 Command Center: KPI fixes (D-10 alerts, D-11 WRICEF)
- Screen 3 Settings: Role label fix (S-07)
- Screen 4 Task List: Assignee names (T-24), module labels (T-28)
- Screen 4 Task Detail:
  - Agent auto-brief with real names and dates
  - What You Need To Do: workstream-specific content
  - Generate Official Document: template library integration (87 templates)
  - Template placeholder fill ([bracket] + {{brace}} formats)
  - Draft Review Email: saves to PM Inbox, correct subject/recipient
  - Escalation Ladder panel in task detail
  - communication_drafts table
  - task_escalations table (5-stage engine)
  - Task Assignment Engine (deterministic, 245/246 assigned)
  - project_tasks FK corrected (auth.users → team_members)
  - CORS Content-Disposition exposed for downloads
  - PM Inbox Email Drafts section

---

## 15. E2E Verification Status (15 June 2026) — ALL 16 PASS

| Screen / Step | Status |
|---|---|
| Login | PASS |
| Command Center | PASS (empty CTA + alert banner) |
| Path Selection | PASS |
| Wizard Steps 1-5 | PASS |
| Project Creation + Task Generation | PASS (multi-module expansion correct by design) |
| Task Screen | PASS |
| Phase View | PASS |
| Agent Assist | PASS (CTA intelligence) |
| RAID Register | PASS (AI pre-fill, PI Score, escalation) |
| Q-Gate | PASS (153 items on Prepare — correct with module expansion; 518 total seeded) |
| Calendar | PASS (scheduling modal, Outlook invite) |
| Stakeholders Screen | PASS |
| Board / Kanban | PASS |
| Change Requests | PASS (full CR-1 rebuild) |
| WRICEF | PASS (FSD bundle complete, all 6 types) |

All 16 screens PASS.

---

## 16. Database Tables (changes this session)

### Modified
| Table | Change |
|---|---|
| project_change_requests | CR-1: 14 columns. **CR-2 (continuation): +19** — 8 impact, 6 CCB, 5 CRF (see §22) |
| project_alerts | **+ resolution_reason** (mandatory on resolve) |
| project_wricef | 10 fsd_* columns (incl. fsd_exceptions JSONB, fsd_sent_with_exceptions) |

### New (all RLS enabled, no policies — service role bypasses RLS per pattern)
- project_document_register
- project_data_migration
- project_commercial
- project_training
- project_alerts (+ 3 indexes)
- project_status_reports (UNIQUE project_id+report_date)
- project_meeting_packs
- project_health_snapshots (UNIQUE project_id+snapshot_date)

**All migrations applied. PostgREST schema cache reloaded. No pending SQL.**

> **Continuation-session DB actions:** (1) granted `service_role` SELECT/INSERT/UPDATE/DELETE on all 8 WC-1..WC-5 tables (they were missing DML grants → "permission denied" on every WC screen). (2) Replaced Project Alpha's 5 broad-category modules with the **12 correct SAP codes** (FICO, MM, SD, PP, QM, PM, HCM, TRM, FM, BASIS, ABAP, Fiori) and cleared team_members.module_id for re-import. New module rows have `category = null` (cosmetic backfill pending).

---

## 17. Features Pending

### HIGH PRIORITY (next session — first action)
- **T-59: Fix D — Send Document To default recipient.** TaskScreen.jsx dropdown should
  default to the Customer team member matching `task.module_code` (not the first Customer
  BPO alphabetically). Logic: find Customer WHERE organisation='Customer' AND
  (role='BPO' OR role='Project Manager') AND module_code=task.module_code. Fallback:
  Customer PM. Fix the default selected value in the dropdown.

### E2E Screens Remaining
- Screen 5: WRICEF Register
- Screen 6: Change Requests (CR-1 + CR-2)
- Screen 7: RAID Register
- Screen 8: Q-Gate
- Screen 9: Documents
- Screen 10: Data Migration
- Screen 11: Commercial
- Screen 12: Training
- Screen 13: Notifications (WC-2)
- Screen 14: Health Dashboard (WC-5)
- Screen 15: Team Members (E-10)
- Screen 16: Calendar / Inbox
- Screen 17: BF Conversion Screen (BF Test Project)

### Completed this session (removed from pending)
- B-17: FSD per WRICEF — COMPLETE
- All E2E screens — ALL 16 PASS
- CR screen — COMPLETE (CR-1 rebuilt)
- Agent system prompts — ALL 11 COMPLETE

### Completed in the continuation session (removed from pending)
- CR-2 (Impact + CCB + CRF), E-01, E-02, E-05, E-06, E-10 — COMPLETE
- SOW module extraction fix; all WC screen permissions; 7 critical bug fixes — COMPLETE

### Still pending
| Item | Status | Notes |
|---|---|---|
| B-7 OneDrive storage verify | Pending | Built, unverified — test on Project Alpha |
| B-8 n8n delivery logging verify | Pending | Built, unverified |
| B-9 Phase gate sign-off Outlook verify | Pending | Built, unverified |
| B-11 MOM auto-generation verify | Pending | Built, unverified |
| CF-16d Q-Gate card layout | Pending | Fields truncated |
| CF-16e Q-Gate intelligence | Pending | Auto-populate owner/dates; phase auto-assess |
| E-11 task-assignment email | Pending | Automation Roadmap §24 |
| E-12 FSD→task complete sync | Pending | Automation Roadmap §24 |
| Industry module support | Pending | Level 1 awareness, post-beta |
| Module category backfill | Pending | Project Alpha module rows null category (cosmetic) |

### Future / Post-Beta
- Streaming agent responses (architectural — post-beta)
- OneDrive change detection + SOW change detection (Automation Roadmap §24)
- Set SOW WRICEF budget on Commercial screen for **Project Alpha** (action for Mohsin)
- Beta Prep: User Manual, Welcome Pack, Feedback Form
- UI/UX design pass (absolute last step before launch)

---

## 18. Template Library - COMPLETE

**87 templates complete. All saved.**

| Phase | GF + Shared | BF Only | Total |
|---|---|---|---|
| Prepare | 15 | 4 | 19 |
| Explore | 14 | 8 | 22 |
| Realize | 13 | 9 | 22 |
| Deploy | 9 | 4 | 13 |
| Run | 7 | 2 | 9 |
| Cross-phase | 2 | 0 | 2 |
| **Total** | **60** | **27** | **87** |

---

## 19. Production Environment

| Service | URL | Status |
|---|---|---|
| Backend | sap-pmo-agent-production-3f52.up.railway.app | HEALTHY |
| Frontend | octiss-production.vercel.app | LIVE |
| Database | qrxfprybbpqugptakeke - Singapore Pro | ACTIVE |
| n8n | n8n-production-c148.up.railway.app | 2 workflows LIVE |

---

## 20. Decisions Log (v2.4 Additions — 15 Jun 2026)

| Date | Decision |
|---|---|
| 15 Jun 2026 | Task count: 193 GF / 201 BF are BASE; multi-module expansion is correct by design (E2E-53 closed). |
| 15 Jun 2026 | All 11 specialist agents override _system_prompt(); RICH_PROMPT + super()._system_prompt(); files preserved (importlib). |
| 15 Jun 2026 | Agents 4 (Explore) + 6 (Deploy) confirmed Sonnet (speed over Opus latency). |
| 15 Jun 2026 | Agent 8 prose JSON schema { subject, body, tone, action_required, deadline }. |
| 15 Jun 2026 | Agents now total 17. New agents 13-17 all claude-sonnet-4-6 (SpecialistAgentBase). |
| 15 Jun 2026 | Streaming agent responses scoped post-batch (architectural change required). |
| 15 Jun 2026 | Customer Health Score: 4 dimensions x 25pts = 100; GREEN>=80/AMBER>=60/RED<60; daily snapshots. |
| 15 Jun 2026 | Scope Creep: unprotected WRICEF = variance minus CR-covered; GREEN/AMBER/RED per coverage; RED raises CRITICAL alert. |
| 15 Jun 2026 | Proactive Alerts: 7 types hourly via n8n; ACKNOWLEDGE vs RESOLVE; dedup by type + item_id (ACKNOWLEDGED as ACTIVE); email = Phase 2. |
| 15 Jun 2026 | Status Reports: Monday 8AM via n8n; Claude Sonnet narrative; no auto-send (mailto); upsert by project_id + report_date. |
| 15 Jun 2026 | Meeting Packs: on demand; 5 types; Sonnet briefing; printable. |
| 15 Jun 2026 | CR lifecycle strict transitions; rejection reason mandatory; CCB escalation irreversible; 6 CR types. |
| 15 Jun 2026 | FSD exceptions per-section, mandatory reason (min 10 chars); internal to Octiss only; fsd_sent_with_exceptions -> amber badges. |
| 15 Jun 2026 | n8n workflows live: proactive_alerts_hourly (60 min) + weekly_status_report (Monday 08:00); X-N8N-Key auth. |
| 15 Jun 2026 | Export columns realigned to locked spec; owner from team registry; phase dates from project dates. |
| 15 Jun 2026 | Agent chat token budget 2600 -> 1500; CTA matchers broadened (Fit-Gap/Gate/Basis/ABAP). |
| 15 Jun 2026 | Team member Role 4-value dropdown enforced; module display fix (PMO label, no duplicates). |
| 15 Jun 2026 | Calendar sessions persist to project_meetings (conforming channel/meeting_type/phase mappers); fallback to localStorage only on failure. |
| 15 Jun (cont.) | **SOW module extraction:** always SAP codes, never categories (FICO not Finance). Prompt has full mapping table. |
| 15 Jun (cont.) | **Industry modules:** standard core supported; industry solutions + add-ons (IS-*, SuccessFactors, Ariba, PPDS) out of scope for agent intelligence now — Level 1 awareness post-beta, Level 2 post-revenue. |
| 15 Jun (cont.) | **Alert resolution_reason** mandatory on resolve; type-specific radio options; stored on project_alerts. |
| 15 Jun (cont.) | **CR-2:** 8 impact fields (PM or AI); CCB manual/auto-on-High; CRF .docx PM-sends via Outlook; AI Suggest is suggestion-only. |
| 15 Jun (cont.) | **Team routing:** GET /team-members returns routing fields; supervisor mailto on escalation; Mendelow 2×2 matrix (Manage Closely / Keep Satisfied / Keep Informed / Monitor). |

---

## 21. Historical Reference (14 June — retained)

### Agent Caching — LOCKED
One agent call per task. Cached in session (module Map) + DB (`agent_context.last_agent_response`). Refresh ↺ is the only way to force a new call.

### Path B SOW Conversion — LOCKED
`creation_path === 'QA'` = no SOW. Uploading a SOW (Project Settings → SOW & Documents → POST /api/v1/projects/{id}/sow, Claude Opus extraction) flips it to `'SOW'` and clears all warnings. Existing task plan is NOT regenerated.

### Risk Register Methodology — LOCKED
```
Probability (5): Certain 5 / Likely 4 / Possible 3 / Unlikely 2 / Rare 1
Impact/Severity (5): Severe 5 / Major 4 / Moderate 3 / Minor 2 / Insignificant 1
PI Score = P x I -> Critical (20-25) / High (16-19) / Medium (9-15) / Low (6-8) / Insignificant (1-5)
Escalation: Critical -> STEERCOM Immediate; High -> WORKCOM 1 Week; Medium -> WORKCOM 2 Weeks;
            Low -> Project Manager 2 Weeks; Insignificant -> Project Manager Monitor
AI pre-fill (POST /api/v1/projects/{id}/raid/suggest-risk): deterministic P/I/PI/escalation;
  Claude Sonnet writes prose; up to 3 ranked suggestions; suggestion-only, PM always saves.
Issues / Decisions / Assumptions logs: Phase 2 (need new backend tables).
```

---

## 22. Continuation Session Features (CR-2, Smart Alerts, E-series, SOW fix)

### CR-2 — Impact Analysis + CCB + CRF
- 19 new columns on project_change_requests: 8 impact (impact_on_scope/schedule/budget/effort/resources, dependencies_affected, risks_introduced, benefits), 6 CCB (ccb_required, ccb_members JSONB, ccb_meeting_date, ccb_decision, ccb_decision_date, ccb_notes), 5 CRF (crf_generated, crf_document_url, crf_generated_at, crf_sent_at, crf_signed_at).
- CRFService → formal .docx CRF (project ID, CR details, 8-field impact analysis, CCB section, approval signatures) for Customer sign-off.
- 3 endpoints: generate-crf (docx), impact-analysis (PATCH), ccb (PATCH — Approved/Rejected auto-sets CR status).
- CR detail panel = 5 sections: Description · Impact · Impact Analysis (8 structured fields) · CCB Escalation · Change Request Form.
- AI Suggest Impact: Change Impact Agent (14) pre-fills the 8 fields; suggestion-only, amber review banner. CCB invite + CRF "Send to Customer" via Outlook mailto.

### Smart Alert Actions (Notifications)
- Context-aware buttons per alert type (Open Task / Draft email / Open RAID / Raise CR / Open Phase View / Check Q-Gate / Chase Sign-Off / Chase Consultant).
- WRICEF_OVER_BUDGET "Raise CR Now" pre-opens the CR modal with WRICEF type.
- "Noted — I'll handle this" replaces Acknowledge (passive, keeps card). "Mark resolved" → reason modal (type-specific radio, mandatory) saved to project_alerts.resolution_reason.
- RAID highlights the specific risk on navigation (auto Risks tab, amber fade, banner).

### E-Series (all COMPLETE)
- E-01 custom module pill (+ Other); E-02 module/team coverage check on Review; E-05 Request Revision Outlook modal; E-06 Evidence Captured → Q-Gate (status=1 by phase+topic); E-10 team routing (GET returns fields, supervisor mailto on escalation, Comm Freq column, Mendelow 2×2 matrix).

### SOW Module Extraction Fix
- sow_extractor.py MODULE EXTRACTION RULES: always SAP codes (FICO/MM/SD/PP/QM/PM/HCM/TRM/FM/BASIS/ABAP/Fiori/GTS/RE-FX/PPDS/WM/LE/EWM/BTP), never categories; full mapping table. Save path stores module_code = code verbatim.

---

## 23. Critical Bug Fixes (continuation session — do NOT regress)

1. **WC screens permission denied** — granted service_role DML on all 8 WC tables.
2. **Vercel build frozen since E-05** — duplicate MONTH_ABBR in TaskScreen.jsx killed every build (prod stuck on E-02). Removed dup; reuse toDisplayDate. *Lesson: check `echo $?`, never pipe build through `tail`.*
3. **Team Members trim() crash** — null-safe `text() = String(value ?? '').trim()`.
4. **CSV modules all PMO** — em-dash vs hyphen match + project modules were categories not codes. Fixed code-first match + SOW prompt + SQL replace of Project Alpha modules.
5. **Notifications wrong route** — repointed to /notifications; added Drafts → /inbox.
6. **Commercial save failing** — permission grant + staleTime:60000 + refetchOnWindowFocus:false.
7. **Edit pencil "did nothing"** — ref + scrollIntoView (form was above the fold).

---

## 24. Project Alpha + E2E Framework + Automation Roadmap

### Project Alpha (primary test project)
ID `b2e3f098-e637-4e7b-9369-5de84ad354b7` · PRJ-ALPHA-2026 · Greenfield Rise (Private Cloud) · Path A · Start 01-Apr-2026 / Go-Live 31-Mar-2027 · Fixed Price USD 2,400,000.
- 12 SAP-code modules (FICO, MM, SD, PP, QM, PM, HCM, TRM, FM, BASIS, ABAP, Fiori); 18 WRICEF (3 each W/R/I/C/E/F, named per SOW); 4 integrations (Bank MT940, 3PL WMS, EDI, HR Payroll); 15 data migration objects; 15 team members (CSV).
- Intentionally UNCOVERED for E-02: QM, PM, TRM, FM.
- DB cleaned to 2 projects: Project Alpha (b2e3f098) + BF Test Project (e9cec48e). **Old 81d46d8b retired — do not use.**
- Source files (Desktop): Project_Alpha_SOW_v1.0.docx, Project_Alpha_Team_Registry_v1.0.csv.

### E2E Framework (next session)
Run full E2E on Project Alpha — Login first, then screen by screen. PM shares screenshot → Claude logs Pass/Fail/Partial → Critical fixed immediately → compile Excel (Option C PMO format). BF Test Project for BF-only screens. tester1 login + error handling (SSO deferred). Upload all 3 docs at session start.

### Automation Roadmap (gaps identified)
1. OneDrive file-change detection (B-7 extended) — n8n nightly poll → agent review → notify. *Post-beta.*
2. E-11 — auto-draft Outlook email on task assignment.
3. E-12 — FSD Approved & Filed → auto-mark linked task Complete.
4. Phase gate auto-assessment — all phase tasks done → auto Q-Gate assessment (folds into CF-16e).
5. SOW change detection — revised SOW → flag new WRICEF / scope / date changes. *Post-beta.*

---

## 25. Architecture Decisions — June 2026

### Task Assignment
- **DETERMINISTIC (no LLM)** — the LLM approach caused worker timeout (60–90s) and silent
  death; rule-based assignment is instant and auditable.
- **Vocabulary mismatch fix:** task `module_code` uses long names (FINANCE,
  HUMANCAPITALMANAGEMENT) while the team registry uses SAP codes (FICO, HCM).
  `_norm_module()` normalizes on lookup (FINANCE→FICO, HUMANCAPITALMANAGEMENT→HCM,
  TECHNICALARCHITECTURE→BASIS, OPERATIONS→PP, LOGISTICS→MM).
- **FK was wrong** (auth.users) — corrected to team_members(id) ON DELETE SET NULL.

### Document Generation
- File-based template matching (NO DB status dependency).
- `template_library` table columns do **not** include 'status' — matching is by filename.
- Path in container: `/app/data/OCTISS OFFICIAL TEMPLATES/` (Dockerfile `COPY . .`).
- CORS **must** expose `Content-Disposition` for browser downloads (else the SPA falls back
  to `.docx` and corrupts xlsx/pptx downloads).

### Escalation
- All communications are **draft-only** Outlook mailto (never auto-send).
- `window.open(mailto:)` is blocked by browsers after async calls — use an `<a href>`
  anchor clicked by the user instead.
- FastAPI BackgroundTasks do not survive Railway restarts (acceptable for short jobs).

---

## 26. Evidence Intelligence Layer (17 June 2026)

### Architecture
Provider-abstracted evidence detection system. Two interfaces:
- StorageProvider: OneDrive (full), GDrive (stub), Dropbox (stub)
- EmailProvider: MS365 (full), Gmail (stub)

### DB Tables
- task_evidence: id, project_id, task_id, evidence_item,
  evidence_order, is_complete, completed_at, completed_by,
  completion_source (calendar/document/email/onedrive/
  gdrive/dropbox/cross_task), completion_reference, created_at
- project_storage_config: project_id, provider, root_path,
  access_token, refresh_token, token_expires_at
- project_email_config: project_id, provider, pm_email,
  access_token, refresh_token, token_expires_at

### Auto-tick Logic
1. calendar: project_meetings.roadmap_task_id = task_id
2. document: communication_drafts.status = 'sent' for task
3. onedrive: Graph API file search in project root_path
4. email: Graph API inbox search for BPO reply to project
5. manual: PM ticks checkbox directly
6. cross_task: E-12 cascade (post-beta)

### EVIDENCE_TEMPLATES (per workstream — role placeholders)
Defined in evidence_service.py. 8 workstreams covered:
Technical Architecture & Infrastructure, Project Management,
Application Design and Configuration, Solution Adoption,
Integration, Testing, Data Management, Extensibility.
DEFAULT fallback for unmatched workstreams.

### Known Caveats
- communication_drafts.status must reach 'sent' for document
  auto-tick (currently 'draft'/'PENDING_REVIEW')
- Calendar uses roadmap_task_id — verify linkage in testing
- project_team_members table has no organisation column —
  defaults to "" (Customer filter won't match these members)

### 4 Backend Endpoints
GET  /api/v1/projects/{id}/tasks/{tid}/evidence
  — auto-initializes evidence on first call
PATCH /api/v1/projects/{id}/tasks/{tid}/evidence/{eid}
  — manual tick/untick by PM
POST /api/v1/projects/{id}/tasks/{tid}/evidence/auto-check
  — single task auto-detection (n8n or manual)
POST /api/v1/projects/{id}/evidence/auto-check-all
  — all active tasks for project (n8n hourly)

### n8n Workflow (PENDING CREATION)
"Octiss — Evidence Intelligence (Hourly)"
Trigger → GET projects → Loop → POST auto-check-all → Log

---

## 27. Task Description Rewrite (17 June 2026)

### GF Complete — BF Pending
- 246 GF tasks: all descriptions rewritten ✅
- BF tasks: pending (create BF project first)
- Script: scripts/generate_task_descriptions.py (gitignored,
  credentials hardcoded for local one-time use)

### Approved Description Format
# [Task Name]
**Phase:** X | **Type:** KEEP/TRACK | **Implementation**

## WHAT THIS TASK INVOLVES
- phase-specific, task-specific, real-world context
- business risk if missed

## WHAT YOU NEED TO DO
- **Contact** [role] to [action]
- **Confirm** [specific thing]
- **Capture** [what + where]
- **Escalate** to [role] if [trigger]
- **Evidence** required: [specific items]

## ACCEPTANCE CRITERIA
- **[Deliverable]** [measurable condition]
- **Evidence** documented and filed

## EVIDENCE REQUIRED
- [Specific item 1-5]

### Rules
- Role placeholders only in DB description column
- Real names injected at render time from team_members
- Partner not SI Firm
- Max 400 words per task
- Task-specific content — never generic boilerplate

### BF Run — Use Batch API + Prompt Caching
- client.messages.batches.create() — 50% cheaper, no hanging
- cache_control on system prompt — 80% input token saving
- No sequential API calls — submit all at once, get results back

---

## 28. Task Detail Page Redesign (17 June 2026)

### Components Added (commit e26957a)
TaskHeaderMeta — header pills row:
  - Planned dates / Actual dates (shown only if set)
  - Variance: Overdue Xd (red) / Start +Xd late (amber) /
    On Track (green)
  - Assignee pill (purple) / Send-to pill (green)

WhoToContact — contact panel below WYNTD:
  - Partner Consultant(s) for task module (handles 2+)
  - Customer BPO for task module
  - Customer PM (escalation Stage 1)
  - Customer SteerCo (escalation Stage 2)
  - Each entry has mailto: link (PM-reviewed, no auto-send)
  - Uses normModule for matching (same as getDefaultSendTo)
  - Receives registryMembers (full team, not filtered)

PMEscalationPath — 4-stage collapsible:
  Stage 1: Partner Consultant (delivery issues)
  Stage 2: Customer BPO (24hr no response)
  Stage 3: Customer PM (48hr unresolved)
  Stage 4: Customer SteerCo (critical/go-live risk only)

EvidenceSection — live evidence checklist:
  - Fetches from GET /evidence on task open
  - Auto-initializes on first load
  - Progress bar (0-100%)
  - Auto-source labels: calendar/document/email/onedrive
  - Manual tick saves via PATCH /evidence/{id}
  - All-complete banner: "Ready to mark Complete"
  - Uses axios api client (not raw fetch) for auth

SapActivateRef — collapsed reference pill:
  Shows phase / workstream / implementation_type on expand

### Deferred
Step 7 — right panel consolidation (duplicate buttons remain):
  Generate Official Document appears in Agent Assist AND
  Document Workflows. Draft Review Email same.
  Target: one panel with DOCUMENTS / COMMUNICATIONS /
  MEETINGS (conditional on task name) / AGENT / SEND DOC TO

### Agent Changes
- "Open Prepare Agent" relabelled to "Ask Phase Agent"
- No auto-fire on task open
- Agent auto-brief useEffect: removed
- All agent calls are PM-initiated only

---

## CR Intelligence Layer (Designed 18 Jun 2026 — Not Yet Built)

### Component 1 — SOW Baseline
No new table required. Baseline data already exists:
- projects table: SOW uploaded via POST /api/v1/projects/{id}/sow,
  Opus extraction runs at upload (module list, WRICEF counts, dates)
- project_commercial: WRICEF budget baseline, integration count,
  contract value — PM enters on Commercial screen
- project_wricef: live WRICEF count for diff against baseline
- Existing WRICEF_OVER_BUDGET alert in Alert Agent IS the scope
  drift signal — connect to CR pre-fill, not just notification

### Component 2 — SCOPE_DRIFT Detection
Extend Alert Agent + n8n hourly workflow with SCOPE_DRIFT check:
- WRICEF count drift: project_wricef rows vs project_commercial budget
- Session note out-of-scope: Meeting Intelligence NLP extraction
- Document review new integration: Document Review Agent scan
- Task in module not in SOW: Task Assignment Engine module check
On detection: fire SCOPE_DRIFT alert → pre-fill CR form with evidence

### Component 3 — CR Auto Pre-Fill (20 of 28 fields)
Change Impact Agent fills on + New CR or SCOPE_DRIFT alert open:
Auto-filled (20): title, cr_type, priority, raised_by, date_raised,
  impact_area, description, impact, impact_on_scope,
  impact_on_schedule, impact_on_budget, impact_on_effort,
  impact_on_resources, dependencies_affected, risks_introduced,
  ccb_required, ccb_members, ccb_meeting_date (today+7),
  crf_generated, status (Draft)
PM-only (8): benefits, ccb_decision, ccb_decision_date, ccb_notes,
  crf_sent_at, crf_signed_at, ccb_meeting_date (override),
  final description edits

---

## RAID Register — Architecture Notes (18 Jun 2026)

### Actual Schema (corrected from earlier docs)
- Active risk table: risks (NOT project_raid_items)
- Active frontend page: RAIDRegister.jsx (Raid.jsx = 1-line stub)
- Risk save was broken: probability/impact CHECK only allowed
  low/medium/high but form sends 5-point values — fixed m10bc

### New Columns on risks table (migration m10bc)
- risk_category: Scope/Schedule/Budget/Process/People/
  Data/Functionality/Technical
- risk_owner: text nullable — day-to-day risk manager
  (distinct from response_owner = escalation decision maker)
- risk_status: Open/In Progress/Escalated/Mitigated/Closed
- probability expanded: Rare/Unlikely/Possible/Likely/Certain
- impact expanded: Insignificant/Minor/Moderate/Major/Severe

### New Tables (Phase 2 — shipped 18 Jun 2026)
project_raid_issues:
  id, project_id, issue_id (ISS-001...), issue_type,
  linked_risk_id, linked_assumption_id, raised_by,
  raised_at, title, description, business_owner,
  response_plan, responsible, due_date, next_check,
  status (Open/In Progress/Resolved/Closed), remarks,
  created_at, updated_at — RLS enabled

project_raid_decisions:
  id, project_id, decision_id (DEC-001...), assumption,
  description, impact_areas, importance, impact_if_false,
  business_owner, assumption_status (True/At Risk/Issue),
  decision_made, decision_by, decision_timeline,
  decision_status (NA/In Progress/Implemented),
  additional_details, created_at, updated_at — RLS enabled

New backend router: raid_registers.py (10 routes)
New frontend module: raidExtras.js

---

## AI-Driven RAID Intelligence Layer (Designed 18 Jun 2026 — Not Yet Built)

### Engine 1 — Evidence Scanner (n8n hourly extension)
New RAID Evaluator added to existing Octiss hourly workflow.
Reads intelligence_config from project before every check.

Auto-creates Issues when:
- Task overdue > task_overdue_issue_days (default 3)
- Task overdue > task_overdue_critical_days → escalate (default 7)
- FSD stuck "Sent to Consultant" > fsd_no_response_days (default 7)
- CR Approved but no new tasks > cr_task_update_warning_days (default 5)
- Meeting action item unresolved past due date
- WRICEF count exceeds SOW budget without CR raised

Auto-creates/updates Risks when:
- Task overdue in critical path module
- 2+ tasks overdue in same workstream
- Go-live < 90 days + Realize phase < 70% complete
- Health score drops for health_decline_snapshots consecutive runs
- PI ≥ 16 risk open > risk_pi16_escalation_days without response plan

Auto-suggests Decisions when:
- WRICEF object in scope with no owner assigned
- Two conflicting session notes reference same object
- Phase end date passed, next phase tasks not started

### Engine 2 — Downstream Impact Mapper
Trigger: any task status change to overdue OR hourly sweep
Logic:
1. Find task delayed > downstream_cascade_days (default 2)
2. Find all tasks in same phase with
   planned_start > delayed task planned_end
3. Mark those tasks status="At Risk"
4. Auto-create or update Risk:
   - Title: "Downstream impact — N tasks at risk due to [Task] delay"
   - Condition: "[Task] is N days overdue"
   - Dependence: "N downstream tasks depend on [Task] completion"
   - Impact: "Phase X completion delayed — go-live impact TBD"
   - risk_category: Schedule
   - PI Score: calculated from delay severity
5. If any impacted task is a Phase Gate task:
   - risk impact = Severe (5)
   - WORKCOM escalation badge fires immediately

### Engine 3 — RAID Intelligence Feed Panel
Location: Right panel on RAID Register screen (new component)
Feed format per signal:
  🔴/🟡 Severity | Source | Signal description
  [Log as Issue] [Log as Risk / Raise CR] [Dismiss]
Rules:
- Dismissed signals suppressed for 48hrs (not re-shown)
- Critical/Severe signals: Acknowledge only (cannot dismiss)
- Clicking action button pre-fills the relevant form
- Feed refreshes on every page load + after each save

DB additions needed:
- auto_generated BOOLEAN on risks + project_raid_issues
- evidence_source TEXT on risks + project_raid_issues
- evidence_item_id UUID on risks + project_raid_issues

---

## Configurable Escalation Thresholds (Designed 18 Jun 2026 — Not Yet Built)
### Decision — PM Has Final Say (18 Jun 2026)
This is a deliberate product decision, not just a config option:

- The PM knows their project, customer, and team better than 
  any default value
- System defaults are starting points only — PM can change 
  any threshold at any time during the project
- The system NEVER overrides PM-set thresholds, even if the 
  default would have fired earlier
- Per-project: each project has its own intelligence_config — 
  thresholds set on Project Alpha do not affect other projects
- Rationale: a 2-day overdue threshold may be right for a 
  critical Realize phase task but wrong for a low-priority 
  Prepare task. The PM decides what warrants action.
- UI principle: show the default clearly so PM understands 
  what they're changing. Never hide or auto-reset thresholds.

### Storage
New column on projects table:
  intelligence_config JSONB DEFAULT '{
    "task_overdue_issue_days": 3,
    "task_overdue_critical_days": 7,
    "fsd_no_response_days": 7,
    "meeting_action_overdue_days": 5,
    "cr_task_update_warning_days": 5,
    "risk_pi16_escalation_days": 7,
    "downstream_cascade_days": 2,
    "health_decline_snapshots": 2
  }'

### UI Location
Project Settings → new section: "Intelligence & Escalation Thresholds"
Number input per threshold with description and default shown.
"Reset to Defaults" button at section bottom.

### Rule
ALL n8n RAID Evaluator checks, Downstream Impact Mapper,
and Alert Agent escalation timing MUST read intelligence_config
from the project record. Never use hardcoded day values.

---

## Screen Intelligence Designs (19-20 Jun 2026)

### Q-Gate Evidence Intelligence
- 8 categories auto-evaluated vs live data:
  Roles & Responsibilities → team_members count (≥5=green)
  Steering Committee → team_members WHERE role ILIKE steering
  Risk Management → risks count (≥1=green)
  Issue Management → project_raid_issues count
  Scope & Change Handling → project_change_requests count
  WBS & Schedule → project_tasks count (≥50=green, ≥10=yellow)
  Budget Monitoring → project_commercial contract_value not null
  Progress Reporting → project_status_reports count
- Evidence badge per item: 🟢 Auto-verified / 🟡 Needs review /
  🔵 Upload doc / ⚪ Manual
- Post-E2E: QG-G2 — AI Review should auto-suggest status
  (set Green for auto-verified, PM confirms not manually ticks)
- POST E2E: QG-G1 — tighten ILIKE patterns to reduce
  General bucket from 55 items

### Documents Register Architecture
- Active table: project_document_register
- NEVER use project_documents — that is the RAG/Knowledge table
  (knowledge.py ingests into project_document_chunks)
- 34 required docs seeded per project (greenfield_rise):
  Prepare(8): Charter/Governance/RACI/Schedule/Kickoff/
    Comms/Risk Mgmt/PM Plan
  Explore(8): BPML/SDD/Workshop Agenda/MOM/FSD/
    Data Migration Strategy/Integration Design/Test Strategy
  Realize(8): Config Workbook/Unit Test/SIT Plan/SIT Results/
    UAT Plan/Cutover Strategy/Training Materials/CRF
  Deploy(6): UAT Sign-off/Go-Live Checklist/Cutover Plan/
    Hypercare Plan/Production Support/Go-Live Approval
  Run(4): Hypercare Report/Closure Report/Lessons Learned/KT Doc
- Phase 2 (post-E2E): Upload + OneDrive + Agent 8 + auto-hooks

### Data Migration Intelligence
- Active table: project_data_migration (extended)
- Source of truth: existing run columns kept + new per-env cols
- AI Suggest: deterministic module→objects mapping
  FICO: Customer Master/Vendor Master/GL Account Master/
    GL Balances/Cost Center/Profit Center/Asset/Bank
  MM: Material Master/Open POs/Purchase Info/Source Lists/Stock
  SD: Customer Master/Open Sales Orders/Pricing/CMIR
  HCM: Employee Master/Org Structure/Pay Scale
- Readiness scoring: Mock1(+20)/Mock2(+25)/Dress(+30)/
  QAS(+15)/Trend(+10)/Threshold breach(-15)/Go-live penalty
- RAID auto-link: sub-threshold → project_raid_issues Technical
- Field labels needed (DM-G4): Pass Rate% / Records Loaded /
  Error Count — auto-calc Records and Errors from Pass Rate

### Commercial Intelligence Architecture
- Shared engine: app/services/commercial_intel.py
- Health calculation: WRICEF variance + CR protection +
  burn rate → GREEN/AMBER/RED (no manual input ever)
- WRICEF protection source: project_wricef.is_in_sow_count +
  cr_approved flags (per-object, not synthetic count)
- Revenue exposure: contract_value ÷ wricef_in_sow ×
  unprotected_count
- Payment Milestones: JSONB on project_commercial
- Burn entries: JSONB on project_commercial
- Health snapshots: JSONB on project_commercial (12-week trend)
- CR financial impact: project_change_requests.financial_impact
- Status Report: _get_commercial_summary in
  status_report_service.py
- Gap COM-G2: health_snapshots populated but frontend
  Commercial Trend section not rendering

### Training Intelligence Architecture
- Active table: project_training (extended, not new)
- Train-the-Trainer model:
  Wave 1 (training_tier='bpo', trained_by='partner', wave=1)
    Partner Consultant trains BPOs/Power Users in Realize
  Wave 2 (training_tier='end_user', trained_by='bpo', wave=2)
    BPOs train End Users in Deploy
    prerequisite_group_id → Wave 1 group for same module
    bpo_signed_off=true required before Wave 2 can progress
- Readiness formula:
  Materials created: 20pts
  Sessions scheduled vs planned: 20pts
  Sessions completed: 30pts
  Attendance rate ≥80%: 20pts
  Sign-off count: 10pts
- Two gates:
  Gate 1 (Realize exit): all Wave 1 groups ≥80%
  Gate 2 (Deploy exit): all Wave 2 groups ≥80%
- RAID auto-link: go-live <60d AND overall <50% →
  People risk auto-created (risks table)
- Training Actions panel replaces chat:
  Groups at risk / Sessions overdue /
  Users not trained / Days to go-live
  Request Materials (Outlook mailto) /
  Schedule Sessions (modal) /
  View in Status Report
- "Generate Training Materials" removed — wrong owner.
  Consultant/Trainer generates materials, not PM.

### Agent Panel UX Rule (Global Product Principle)
Established: 20 Jun 2026

Rule: ALL "Ask the agent" chat panels across ALL screens
MUST be replaced with context-aware Actions panels.

Rationale: Agents are invisible workers, not chatbots.
They fire automatically and surface results.
PM never needs to type a question to an agent.

Actions panel shows:
- What the agent has already detected/done
- Specific actionable next steps
- Buttons that pre-fill forms or draft emails
- Nothing is sent automatically — PM reviews all outputs

Exception: Voice PMO Copilot (explicitly conversational)

Status:
✅ Implemented: Commercial, Training
🔲 Remaining: Data Migration, Documents, WRICEF, any others

### Notification Alert Grouping (Post-E2E)
Current: one card per alert (20 overdue tasks = 20 cards)
Required: one collapsed card per alert TYPE:
  "20 tasks overdue in Prepare phase [Expand] [Bulk Action]"
Individual cards only for: CRITICAL alerts + unique alerts
Training alerts: deduplicate (NOT-G1)
Same-type grouping: collapse with count (NOT-G2)

---

*End of Document - Octiss SUD v2.8 - 18 June 2026 (WRICEF Intelligence Layer + Document Filing Service)*
*Supersedes: Octiss_SUD_v2.4.md*
*Read alongside: Octiss_Master_Handoff_v2.0.md*
