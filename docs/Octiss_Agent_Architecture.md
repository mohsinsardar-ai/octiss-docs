# Octiss — Agent Architecture
**Version:** v1.0 | **Date:** 18 June 2026
**Owner:** Mohsin Sardar — EM Intelligence Labs
**Purpose:** Single source of truth for all AI agents in Octiss.
Every Claude and Codex session must read this before building,
modifying, or wiring any agent.
**Rule:** Never create a new agent without checking this document
first. If an existing agent can be extended, extend it.
Never duplicate agent responsibility.

---

## Agent Inventory — 17 Agents

### GROUP 1 — PHASE AGENTS (5)
These agents provide task-level guidance for each SAP Activate
phase. Triggered when PM opens a task and clicks Ask Phase Agent.
They read full project context before responding.

#### Agent 1 — Prepare Agent
**Trigger:** PM opens Prepare phase task → clicks Ask Phase Agent
**Purpose:** Guide PM through Prepare phase tasks. Explain what
needs to happen, who to involve, what evidence to collect, and
what risks to watch for.
**Input:** Task name, workstream, module, team roster, project
dates, task type (KEEP/TRACK)
**Output:** Conversational guidance — next PM move, specific
actions, escalation triggers
**Does NOT:** Generate documents, send emails, create records.
Guidance only.

#### Agent 2 — Explore Agent
**Trigger:** PM opens Explore phase task → clicks Ask Phase Agent
**Purpose:** Guide PM through Explore phase — Fit-Gap workshops,
FSD initiation, process confirmation sessions. Knows that Explore
is where WRICEF requirements are confirmed and FSDs are initiated.
**Input:** Same as Prepare Agent
**Output:** Conversational guidance specific to Explore phase
**Does NOT:** Generate FSDs (that is FSD Agent). Guidance only.

#### Agent 3 — Realize Agent
**Trigger:** PM opens Realize phase task → clicks Ask Phase Agent
**Purpose:** Guide PM through Realize — configuration, development,
testing coordination. Knows Realize is where FSDs are being
developed by Consultants and UAT is being prepared.
**Input:** Same as Prepare Agent
**Output:** Conversational guidance specific to Realize
**Does NOT:** Review FSDs (that is Document Review Agent).

#### Agent 4 — Deploy Agent
**Trigger:** PM opens Deploy phase task → clicks Ask Phase Agent
**Purpose:** Guide PM through Deploy — cutover, data migration
execution, go-live readiness checks.
**Input:** Same as Prepare Agent
**Output:** Conversational guidance specific to Deploy

#### Agent 5 — Run Agent
**Trigger:** PM opens Run phase task → clicks Ask Phase Agent
**Purpose:** Guide PM through Run — hypercare, issue resolution,
knowledge transfer, project close.
**Input:** Same as Prepare Agent
**Output:** Conversational guidance specific to Run

---

### GROUP 2 — DOCUMENT AGENTS (3)

#### Agent 6 — FSD Agent
**Trigger:** PM clicks "Create FSD Template" on a WRICEF item
**Purpose:** Generate a pre-filled FSD template (.docx) for a
specific WRICEF item. The template is NOT the final FSD — it is
a structured starting point for the Consultant to complete.
**Input:**
- WRICEF item details (ref, title, type, module, scenario)
- Project context (name, go-live, contract value)
- Linked session notes from wricef_session_notes table
  (requirements agreed in Fit-Gap/SDD sessions)
- Team member for this module (Consultant name + email)
**Output:** .docx template with:
- Header: WRICEF ref, title, type, module, project
- Pre-filled requirements section from session notes
- Empty sections marked [CONSULTANT TO COMPLETE]
- Open items from sessions marked [TBD — DECISION PENDING]
- Acceptance criteria section for Customer BPO sign-off
**Does NOT:** Complete the FSD. Review quality. Send to
Consultant (that is Stakeholder Agent).
**Depends on:** Meeting Intelligence Agent having extracted
and stored session notes first. If no session notes exist,
generates blank template with warning.

#### Agent 7 — Change Impact Agent
**Trigger:** PM clicks "AI Suggest Impact" on a Change Request
**Purpose:** Pre-fill the 8-field impact analysis for a CR
(impact on scope/schedule/budget/effort/resources,
dependencies affected, risks introduced, benefits).
**Input:** CR title, description, CR type, project context
**Output:** JSON with 8 impact fields. Always suggestion-only.
**Does NOT:** Approve CRs. Send CRF. PM actions only.

#### Agent 8 — Document Review Agent
**Trigger:** PM uploads a document via "Upload for Review"
(Task Checklist tab or WRICEF FSD review)
**Purpose:** Review uploaded document for quality and
completeness against task/WRICEF acceptance criteria.
**Input:**
- Uploaded document (PDF/DOCX/XLSX)
- Task or WRICEF acceptance criteria
- For WRICEF FSDs: linked session notes from
  wricef_session_notes (agent knows what requirements
  should be covered)
**Output:** JSON scorecard:
- Overall score (0-100), completeness, accuracy,
  format compliance, SAP Activate alignment scores
- Summary sentence
- Strengths list
- Gaps list (specific missing items)
- Recommendation: Pass or Needs Revision
**Does NOT:** Send revision requests (Stakeholder Agent).
Approve documents (PM decision).

**WRICEF Evidence-Based Review (auto-triggered on upload):**
When reviewing FSD/SDD/BPML documents:
1. Reads submitted document
2. Reads wricef_session_notes for that WRICEF item
3. For each session requirement — checks if covered:
   - Covered ✅
   - Missing ❌ (with session date and exact requirement)
   - Partial ⚠️ (needs clarification)
4. Scorecard includes session-referenced gaps:
   "Requirement from [session_type] [date]:
   '[requirement]' — NOT found in submitted document"
5. Overall: requirements_coverage % score

**New output fields:**
- requirements_coverage (% of session requirements covered)
- missing_requirements (list with session references)
- partial_requirements (list needing clarification)
- sessions_checked (count of sessions reviewed)

**Backend endpoint:** POST /api/v1/tasks/{id}/review-uploaded-document
  or POST /api/v1/wricef/{id}/review-fsd

---

### GROUP 3 — COMMUNICATION AGENTS (3)

#### Agent 9 — Escalation Agent
**Trigger:** n8n hourly → process-due endpoint detects overdue task
**Purpose:** Draft escalation emails at each of 5 ladder stages.
All emails are mailto drafts — never auto-sent. PM always reviews.
**Ladder:**
- Stage 1: Initial request to assigned Consultant
- Stage 2: Reminder 1 (24hrs later)
- Stage 3: Reminder 2 (48hrs later)
- Stage 4: Escalation 1 — Supervisor CC (72hrs)
- Stage 5: Escalation 2 — Supervisor as primary (96hrs)
**Input:** Task details, assignee email, supervisor email,
PM email, project name, task due date
**Output:** mailto: string with To/CC/Subject/Body
**Does NOT:** Auto-send. Change task status.
**Also used for:** WRICEF FSD overdue escalation

#### Agent 10 — Status Report Agent
**Trigger:** n8n Monday 8AM weekly workflow
**Purpose:** Generate weekly project status report narrative.
Reads project health data and writes executive summary,
RAG status, key achievements, upcoming milestones, risks,
and decisions needed.
**Input:** Project tasks, RAID register, change requests,
WRICEF status, go-live days remaining, phase progress
**Output:** Formatted status report saved to
project_status_reports. PM reviews and sends.
**Does NOT:** Send the report. PM reviews first.

#### Agent 11 — Stakeholder Communication Agent
**Trigger:** Multiple triggers:
1. PM clicks "Draft Review Email" on a task
2. FSD template created → draft send email to Consultant
3. FSD review complete → draft revision/approval email
4. CR approved/rejected → draft notification to Customer PM
5. Phase gate passed → draft sign-off notification
**Purpose:** Draft context-aware stakeholder emails. Knows
who to address based on action type:
- Task review → assigned Consultant
- FSD send → module Consultant
- FSD revision → Consultant with gaps listed
- FSD approval → Customer BPO
- CR notification → Customer PM
- Phase gate → Customer SteerCo
**Input:** Action type, stakeholder details from team_members,
document/task/CR context
**Output:** mailto: draft with correct To/CC/Subject/Body
All drafts PM-reviewed — never auto-sent.
**Does NOT:** Auto-send. Make decisions.

---

### GROUP 4 — INTELLIGENCE AGENTS (3)

#### Agent 12 — RAID Agent
**Trigger:** PM clicks "AI Suggest" on RAID register
**Purpose:** Suggest up to 3 ranked risks based on project
context (phase, overdue tasks, WRICEF count, CR count,
team gaps, go-live proximity). Deterministic P/I/PI scoring.
**Input:** Project phase, task completion %, WRICEF status,
CR status, team coverage gaps, go-live days
**Output:** Up to 3 risk suggestions with probability,
impact, PI score, escalation level, suggested mitigation.
Always suggestion-only — PM saves to register.
**Does NOT:** Auto-add risks.

#### Agent 13 — Alert Agent
**Trigger:** n8n hourly → evaluate endpoint
**Purpose:** Evaluate all active projects and generate
proactive alerts when thresholds are breached:
- Task overdue by more than X days
- WRICEF FSD overdue
- Phase milestone at risk
- Scope creep detected (WRICEF over budget)
- No status report filed this week
- CR pending approval past SLA
**Input:** All project data
**Output:** project_alerts records with alert_type,
severity, title, message, smart action buttons
**Does NOT:** Take action. PM receives and acts.

#### Agent 14 — Health Score Agent
**Trigger:** n8n hourly (same as Alert Agent)
**Purpose:** Calculate customer health score and detect
scope creep. Combines task completion rate, WRICEF FSD
completion rate, CR count vs baseline, team engagement,
go-live risk.
**Input:** Full project snapshot
**Output:** Health snapshot saved to project_health_snapshots:
overall_score, schedule_health, scope_health, team_health,
delivery_health, scope_creep_detected, narrative summary
**Does NOT:** Alert directly (Alert Agent does that).

---

### GROUP 5 — MEETING AGENT (1)

#### Agent 15 — Meeting Intelligence Agent
**Trigger:** Multiple triggers:
1. Digital Twin completes session recording (auto)
2. PM uploads session transcript/notes manually
3. PM clicks "Generate MOM" from Calendar event

**Purpose:** The most important agent for evidence capture
and WRICEF intelligence. Operates at two levels:

**Level 1 — Full Session MOM (task level):**
- Processes transcript/recording from session
- Generates structured MOM covering all topics discussed
- Extracts: decisions, action items, attendees confirmed,
  open items, next steps
- Sends MOM to PM in Octiss Notifications for review
- On PM approval — Stakeholder Agent distributes to attendees
- Filed in OneDrive: Project Management/MOMs/
- Evidence stored against the linked task

**Level 2 — WRICEF Requirements Extraction (WRICEF level):**
- From same session transcript, extracts WRICEF-specific
  requirements for each WRICEF item covered in session
- Extracts: decisions, requirements agreed, gaps identified,
  open items, acceptance criteria discussed
- Stored in wricef_session_notes table per WRICEF item
- Read by FSD Agent when generating FSD template
- Read by Document Review Agent when reviewing FSD/SDD/BPML

**Input:**
- Audio transcript (from Voice Agent / Whisper API)
  OR PM-uploaded notes/transcript
- Session type (Fit-Gap / SDD / BPML / UAT / SteerCo)
- Linked task ID
- Linked WRICEF item IDs (which WRICEF items were covered)
- Attendee list from project_meetings

**Output:**
- Structured MOM document (.docx)
- wricef_session_notes records (one per WRICEF item covered)
- MOM pending PM approval (stored in project_meetings)
- Notification to PM: "MOM ready for review"

**Does NOT:** Send MOM directly (PM approval required first).
Auto-approve anything. Create FSDs (FSD Agent does that).

**Critical dependency:** FSD Agent and Document Review Agent
both depend on this agent having run first for a WRICEF item.
Without session notes, FSD template is blank and document
review cannot check requirements coverage.

### Meeting Intelligence — Digital Twin Architecture

#### Digital Twin / Voice Copilot
**What it is:** An AI bot that joins every Octiss-scheduled
session automatically as a silent participant.
Records audio, transcribes, feeds Meeting Intelligence Agent.

**Supported platforms — Phase 1 (build now):**
- Microsoft Teams (via MS Graph API — already connected)
- Google Meet (via Google Meet API — Google OAuth connected)

**Supported platforms — Phase 2 (stub now, build later):**
- Zoom (via Zoom Meeting SDK)

**Transcription engine:** OpenAI Whisper large-v3
- Accuracy: best-in-class multilingual
- Languages: English, Urdu, Arabic (+ 98 others)
- Speaker diarization: knows who said what
- Cost: ~$0.006/minute of audio

**Recording lifecycle:**
Session scheduled in Octiss Calendar
    → Digital Twin auto-invited to every meeting
    → Session starts → recording begins (Teams or Google Meet)
    → Session ends → Whisper large-v3 transcribes audio
    → Transcript saved to DB permanently
    → Audio saved to Supabase Storage (temp bucket, 48hr TTL)
    → Meeting Intelligence Agent processes transcript:
        → MOM generated
        → WRICEF requirements extracted per item covered
    → PM notified in Octiss Notifications
    → PM reviews transcript and MOM → approves or corrects
    → On PM approval:
        → Stakeholder Agent distributes MOM to attendees
        → Audio deleted from Supabase Storage immediately
        → Transcript kept permanently as evidence
    → Maximum audio retention: 7 days regardless of status

**Fallback — PM manual upload (always available):**
If Digital Twin was not present OR recording failed:
→ PM uploads transcript (.txt/.docx) or audio file manually
→ Same processing pipeline applies
→ PM reviews → approves → Stakeholder Agent distributes

#### MOM Review Flow (PM)
After Meeting Intelligence Agent processes session:

**Location 1 — Notifications:**
PM receives alert: "MOM ready for review — [Session Name]"
→ Click → opens MOM review panel
→ PM can edit, approve, or request re-extraction

**Location 2 — Calendar Event:**
Open the session in Calendar → MOM tab
→ Shows transcript, extracted MOM, WRICEF requirements covered
→ PM reviews and approves from here

**Location 3 — Linked Task Evidence:**
The task linked to the session shows in Evidence section:
☑ Session MOM generated and filed (auto-ticked on approval)
☑ MOM distributed to attendees (auto-ticked on send)

#### Document Quality Review — Evidence-Based Check
When Consultant submits FSD/SDD/BPML and PM uploads it:

**Auto-triggered on upload (no PM click needed):**
Document Review Agent (Agent 8) runs automatically:
1. Reads submitted document
2. Reads ALL linked session transcripts in wricef_session_notes
3. For each requirement from sessions — checks if covered:
   Covered / Missing (with session date and requirement) /
   Partial (needs clarification)
4. Returns scorecard with session-referenced gaps:
   "Requirement from [session_type] on [date]:
   '[requirement]' — NOT found in submitted document"

**PM decision:**
- Approve → Stakeholder Agent notifies Customer BPO
- Request Revision → Stakeholder Agent drafts revision email
  to Consultant listing specific gaps with session references

---

### GROUP 6 — SETUP AGENT (1)

#### Agent 16 — Project Plan Agent
**Trigger:** SOW upload → extraction complete → PM confirms scope
**Purpose:** Generate the full task plan (246 GF or 201 BF tasks)
based on SOW scope, modules, and implementation type.
Runs once at project creation.
**Input:** Extracted SOW data (modules, WRICEF count, go-live
date, implementation type)
**Output:** project_tasks records for all phases
**Does NOT:** Assign tasks (Task Assignment Engine does).
Generate WRICEF items (those come from SOW extraction).

---

### GROUP 7 — VOICE AGENT (1)

#### Agent 17 — Voice PMO Copilot
**Two distinct functions:**

**Function A — Digital Twin (passive, always-on):**
Joins every Octiss-scheduled session automatically.
Records audio silently. Does not participate in discussion.
Feeds transcript to Meeting Intelligence Agent (Agent 15).
PM does not need to activate — it is automatic for all sessions.

**Platforms (Phase 1):**
- Microsoft Teams bot (via MS Graph API)
- Google Meet bot (via Google Meet API / Google OAuth)

**Platforms (Phase 2 — stub now):**
- Zoom bot (via Zoom Meeting SDK)

**Function B — Interactive Voice Copilot (on-demand):**
PM activates via microphone button in Octiss UI.
PM speaks commands/questions in English, Urdu, or Arabic.
Voice Agent routes to appropriate agent or action.
Responds in voice.

Examples:
- "What tasks are overdue?" → Alert Agent query
- "Draft escalation for task 1.2.3" → Escalation Agent
- "What is the status of WRICEF-001 FSD?" → DB lookup
- "Schedule a Fit-Gap session for FICO next Tuesday" → Calendar

**Does NOT:** Auto-send anything. Bypass PM approval.
Make decisions without PM confirmation.

**Transcription:** OpenAI Whisper large-v3
**Languages:** English, Urdu, Arabic (+ 98 others)
**Speaker diarization:** identifies who said what in transcript

---

#### Agent 17 — Implementation status (as built, 20-21 Jun 2026) — TWO components

**Component A — Interactive Q&A (pre-existing):**
- File: `app/routers/voice.py`
- Endpoints: `POST /api/v1/voice/query`, `GET /api/v1/voice/history`, `GET /api/v1/voice/usage`
- Purpose: PM asks a question by voice/text → answer + citations
- Usage metering: 60 queries/month (Project tier)

**Component B — Meeting MOM Lifecycle (built this session):**
- File: `app/routers/meetings.py` (lines ~2284-2478)
- Endpoints: 7 × `/api/v1/meetings/voice/*`
- Purpose: Upload recording → Whisper transcription → MOM generation → PM review → approve
- Table: `project_meetings` (extended via m10bq)
- Coverage: `meeting_wricef_coverage` (table)
- Audio TTL: 48hr via `audio_retained_until` column
- Cleanup: `POST /api/v1/meetings/voice/cleanup-audio` (n8n scheduled, `X-Cleanup-Key` auth)
- voice_status lifecycle: pending → audio_uploaded → transcribed → mom_generated → approved

---

## GROUP 8 — TRACKING AGENTS (added 19-20 Jun 2026)

These two agents are wired in the backend (chat slugs) but were not
numbered in the original 17-agent inventory. Documented here; see the
Agent Numbering Discrepancy note below for reconciliation.

#### Commercial / Contract Agent
**Slug:** commercial_contract_agent
**Backend:** app/services/commercial_intel.py
**Trigger:** Every GET /commercial + hourly n8n check
**Purpose:** Monitor commercial health, detect unprotected WRICEF,
calculate revenue exposure, flag scope creep.
**Inputs:** project_commercial, project_wricef
(is_in_sow_count/cr_approved), project_change_requests,
project tasks (consumed days).
**Outputs:** commercial_health (GREEN/AMBER/RED auto),
wricef_protected/unprotected counts, unprotected_revenue,
budget_burn_pct, health_snapshots (12-week trend).
**Does NOT:** Send emails, auto-raise CRs, require PM input.
**Actions panel outputs:** Raise CR prompt, invoice reminder draft,
budget risk communication draft.

#### Training & Adoption Agent
**Slug:** training_adoption_agent
**Backend:** app/services/training_intel.py
**Trigger:** Every GET /training + on group save
**Purpose:** Track training readiness, enforce Train-the-Trainer wave
model, score groups, flag at-risk user populations.
**Inputs:** project_training (all groups), project.go_live_date,
team_members (BPO names), project_document_register (training materials).
**Outputs:** readiness_score per group (0-100), overall readiness,
Gate 1 + Gate 2 status, agent_assessment text, RAID risk if <50%
near go-live.
**Does NOT:** Send emails, auto-schedule sessions, generate training
materials (Consultant's job).
**Actions panel outputs:** Request Materials mailto draft,
Schedule Sessions modal, Status Report link.

### AGENT NUMBERING DISCREPANCY (flagged 19-20 Jun 2026)
The architecture doc numbering does not match backend agent slug
numbering. Agents without documented numbers in this file:
- Commercial/Contract Agent (slug: commercial_contract_agent)
- Training/Adoption Agent (slug: training_adoption_agent)
- Data Migration Agent (slug: data_migration_agent,
  referenced as Agent 15 in some session notes — note Agent 15 in
  this file is the Meeting Intelligence Agent)
Action needed: Reconcile all agent numbers and ensure every backend
slug has a matching doc entry.

---

## NEW THIS SESSION (20-21 Jun 2026) — Document & Audio Infrastructure

### Document Intelligence — auto-register hooks
- `register_generated_document()` in `app/services/document_seed.py`
- Called by: FSD (`wricef_inventory.py`), Status Report (`reports_intel.py`),
  SteerCo (`project_health.py`), CRF (`change_requests.py`)
- Purpose: auto-registers generated docs to `project_document_register`
- Pattern: best-effort upsert by (project_id, document_name) — never breaks generation

### Document Upload (new)
- File: `app/routers/new_agents.py`
- Endpoints: `POST /api/v1/projects/{project_id}/document-register/{doc_id}/upload`
  and `POST /api/v1/projects/{project_id}/document-register/{doc_id}/verify-filing`
- Storage: Supabase bucket `project-documents` (private, 100MB)
- B-7 verification: lists the storage path, stamps `onedrive_verified` + `onedrive_verified_at`
- Note: reuses the `onedrive_verified` columns for Supabase storage (flagged: may need a
  dedicated `storage_verified` if OneDrive + Supabase ever coexist on the same doc)

### Audio Cleanup (new)
- File: `app/routers/meetings.py`
- Endpoints: `POST` + `DELETE /api/v1/meetings/voice/cleanup-audio`
- Auth: `X-Cleanup-Key` header (dedicated, NOT `X-N8N-Key` — least-privilege for a destructive delete)
- Trigger: n8n "Octiss — Audio Cleanup (6hr)" workflow
- Logic: deletes from the `meeting-audio` bucket where
  `audio_retained_until < NOW()` AND `audio_deleted_at IS NULL` AND `voice_status='approved'`;
  then stamps `audio_deleted_at` and nulls `audio_path`. Idempotent.

---

## Agent Interaction Map
```

SOW Upload → Project Plan Agent (task generation)
Task Opens → Phase Agent (guidance on demand) → Document Review Agent (if PM uploads doc) → Escalation Agent (if overdue, auto via n8n)
WRICEF Item → Meeting Intelligence Agent (after Fit-Gap session) → stores wricef_session_notes → FSD Agent (reads session notes → creates template) → Stakeholder Agent (sends template to Consultant) → Document Review Agent (reviews returned FSD) → Stakeholder Agent (sends revision/approval email) → Escalation Agent (if FSD overdue)
Change Request → Change Impact Agent (impact pre-fill) → Stakeholder Agent (CRF send / approval notification)
Meetings → Meeting Intelligence Agent (MOM + requirements) → Stakeholder Agent (MOM distribution)
Proactive (hourly n8n) → Alert Agent → Health Score Agent → Escalation Agent (process-due)
Weekly (Monday n8n) → Status Report Agent
All stakeholder communications → Stakeholder Agent → All output is mailto draft — never auto-sent

```

---

## What Does NOT Have Its Own Agent

These are handled by deterministic code (no LLM):

| Function | Handler |
|---|---|
| Task assignment | Task Assignment Engine (rule-based) |
| Escalation ladder initialization | Escalation Service (rule-based) |
| Q-Gate auto-initialization | Q-Gate Service (rule-based) |
| Template matching (Generate Document) | Keyword rules (85 rules) |
| Evidence auto-tick | Evidence Service (rule-based) |
| WRICEF SOW extraction | Claude Opus (one-time, not an agent) |

---

## Rules for Adding New Agents

Before creating any new agent:
1. Check this document — does an existing agent cover this?
2. If an existing agent can be EXTENDED, extend it
3. If truly new functionality is needed, add here first
4. Update this document immediately when agent scope changes
5. Commit updated file to both repos

---

## Locked Decisions (added 20 Jun 2026)

- Agent panels: ALL "Ask the agent" chat panels replaced with
  context-aware Actions panels. Agents work invisibly.
  Exception: Voice PMO Copilot only.
- Training materials: Generated by Consultant, not PM.
  PM's role is to REQUEST materials via Outlook draft.
- Commercial health: Auto-calculated only, never manual.
  No manual GREEN/AMBER/RED selector anywhere.
- Table discipline: Always check existing tables first.
  Never create new table if existing can be extended.
  Key table mapping (canonical, never deviate):
  - Risks → risks table (not project_raid_items)
  - Issues → project_raid_issues
  - Decisions → project_raid_decisions
  - Documents → project_document_register
    (NEVER project_documents = RAG table)
  - Training → project_training
  - Commercial → project_commercial

---

## DB Tables Used by Agents

| Agent | Tables Read | Tables Written |
|---|---|---|
| Phase Agents | project_tasks, team_members, projects | agent_context |
| FSD Agent | project_wricef, wricef_session_notes, team_members | project_wricef (fsd_path) |
| Change Impact Agent | project_change_requests | project_change_requests |
| Document Review Agent | project_tasks, project_wricef | (returns JSON only) |
| Escalation Agent | task_escalations, team_members | communication_drafts |
| Status Report Agent | all project tables | project_status_reports |
| Stakeholder Agent | team_members, project_tasks, project_wricef | communication_drafts |
| RAID Agent | projects, project_tasks, project_wricef | (returns JSON only) |
| Alert Agent | all project tables | project_alerts |
| Health Score Agent | all project tables | project_health_snapshots |
| Meeting Intelligence Agent | project_meetings, project_wricef | wricef_session_notes, communication_drafts |
| Project Plan Agent | projects (SOW data) | project_tasks |
| Voice Agent | all (routes to other agents) | (delegates) |

---

## Pending Agent Enhancements (Backlog)

| Item | Agent | Priority |
|---|---|---|
| Read wricef_session_notes before FSD generation | FSD Agent | HIGH |
| Extract requirements from MOM → wricef_session_notes | Meeting Intelligence Agent | HIGH |
| Link meetings to WRICEF items (wricef_id on project_meetings) | Meeting Intelligence Agent | HIGH |
| FSD overdue escalation | Escalation Agent | MEDIUM |
| FSD revision email with gaps listed | Stakeholder Agent | MEDIUM |
| FSD approval notification to Customer BPO | Stakeholder Agent | MEDIUM |
| Phase gate auto-assessment | Alert Agent extension | LOW |

---

## New DB Columns Required (Meeting Intelligence + Voice)

```sql
-- Add to project_meetings table:
ALTER TABLE project_meetings
  ADD COLUMN IF NOT EXISTS transcript TEXT,
  ADD COLUMN IF NOT EXISTS transcription_status TEXT
    DEFAULT 'pending',
  ADD COLUMN IF NOT EXISTS recording_path TEXT,
  ADD COLUMN IF NOT EXISTS recording_expires_at TIMESTAMPTZ,
  ADD COLUMN IF NOT EXISTS mom_content TEXT,
  ADD COLUMN IF NOT EXISTS mom_status TEXT DEFAULT 'pending',
  ADD COLUMN IF NOT EXISTS mom_approved_at TIMESTAMPTZ,
  ADD COLUMN IF NOT EXISTS mom_distributed_at TIMESTAMPTZ,
  ADD COLUMN IF NOT EXISTS digital_twin_joined BOOLEAN
    DEFAULT false,
  ADD COLUMN IF NOT EXISTS platform TEXT DEFAULT 'teams',
  ADD COLUMN IF NOT EXISTS wricef_ids_covered JSONB
    DEFAULT '[]';

-- New table: links meetings to WRICEF items covered
CREATE TABLE IF NOT EXISTS meeting_wricef_coverage (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  meeting_id UUID REFERENCES project_meetings(id)
    ON DELETE CASCADE,
  wricef_id UUID REFERENCES project_wricef(id)
    ON DELETE CASCADE,
  session_notes_id UUID REFERENCES wricef_session_notes(id)
    ON DELETE SET NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

## New Backend Services Required

| Service | Purpose | Priority |
|---|---|---|
| app/services/voice/whisper_service.py | OpenAI Whisper large-v3 transcription | HIGH — Phase 1 |
| app/services/voice/teams_bot_service.py | Teams bot join + record | HIGH — Phase 1 |
| app/services/voice/meet_bot_service.py | Google Meet bot join + record | HIGH — Phase 1 |
| app/services/voice/zoom_bot_service.py | Zoom bot (stub) | Phase 2 |
| app/services/meeting_review_service.py | MOM review + approval flow | HIGH — Phase 1 |

## Pending Build Backlog — Meeting Intelligence + Voice

| Item | Service | Priority |
|---|---|---|
| OpenAI Whisper large-v3 integration | whisper_service.py | HIGH |
| Microsoft Teams bot auto-join | teams_bot_service.py | HIGH |
| Google Meet bot auto-join | meet_bot_service.py | HIGH |
| Supabase Storage audio bucket (48hr TTL) | Infrastructure | HIGH |
| MOM review panel in Notifications | Frontend | HIGH |
| MOM tab in Calendar event | Frontend | HIGH |
| project_meetings transcript columns | DB migration | HIGH |
| meeting_wricef_coverage table | DB migration | HIGH |
| Auto-trigger document review on FSD upload | Document Review Agent | HIGH |
| Session-referenced gaps in review scorecard | Document Review Agent | HIGH |
| Zoom bot stub | zoom_bot_service.py | Phase 2 |

---

*End of Document — Octiss Agent Architecture v1.1 — 18 June 2026*
*Changes in v1.1: Meeting Intelligence two-level architecture,
Digital Twin (Teams + Google Meet Phase 1, Zoom Phase 2),
OpenAI Whisper large-v3, evidence-based document review,
MOM approval flow, recording lifecycle with 48hr TTL*
*Read before building any agent feature*
*Update immediately when agent scope changes*
