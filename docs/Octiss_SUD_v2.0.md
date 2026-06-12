# Octiss — Solution Understanding Document (SUD)
**Version:** v2.0 | **Date:** 11 June 2026 | **Status:** LIVE
**Owner:** Mohsin Sardar — EM Intelligence Labs
**Classification:** Confidential — Internal Use Only

> **PURPOSE:** Single source of truth for what Octiss should be and why.
> Every Claude session and every Codex prompt must read this document first.

---

## Changelog

| Version | Date | Changes |
|---|---|---|
| v1.0–v1.8 | 01–09 Jun 2026 | See previous SUD versions |
| v1.9 | 11 Jun 2026 | 12 Codex prompts. Tests 1235→1311. Both repos pushed. |
| v2.0 | 11 Jun 2026 | 26 total Codex prompts today. Tests 1311→1320. Full feature build complete. E2E started. Critical fixes applied. Group B features built. Folder path decision locked. Delete Task removed. Agent tone locked. |

---

## 1. Product Vision

Octiss is an AI-powered SAP PMO Intelligence Platform. It guides SAP Project Managers through every phase of an SAP implementation — task by task, phase by phase — using 12 specialist AI agents embedded directly into the project workflow.

**One-Liner:** Intelligence for Every Project. Clarity for Every Decision.

---

## 2. Methodology Coverage

| Code | Methodology | Status |
|---|---|---|
| GF | New Implementation (Rise with SAP) | ✅ Active |
| BF | System Conversion (Grow with SAP) | ✅ Active |
| SF | Selective Data Transition | ⏸ Deferred |

---

## 3. Project Creation Flow — LOCKED

### Path A (PRIMARY — SOW Upload)
```
PM clicks New Project → Upload SOW/Contract
  ↓
Claude Opus extracts:
  Project Name, Methodology, Go-Live Date,
  Modules, WRICEF Count, Integrations,
  Data Migration scope, Timeline
  ↓
Confirmation screen — PM reviews and corrects
  ↓
Project Plan Agent generates full task plan
  GF: 193 active tasks
  BF: 201 active tasks
  ↓
Redirect to Phase View — Prepare phase
```

### Path B (FALLBACK — Manual Q&A)
```
PM clicks "Set Up Manually"
  ↓
5-step wizard: Basics → Modules → Storage → Team → Review
  ↓
Project Plan Agent auto-triggered after wizard
  GF: 193 active tasks seeded
  BF: 201 active tasks seeded
  ↓
Redirect to Phase View — Prepare phase
```

### Mock Test Files (Desktop)
- `Project_Alpha_SOW_v1.0.txt` — GF, 14 modules, 18 WRICEF, 3 integrations
- `Project_Alpha_Team_Registry_v1.0.csv` — 17 team members
  - 2 ABAP: Robert Taylor (Senior) + Nina Kowalski
  - 2 Basis: Lisa Zhang (Lead) + Hassan Al-Rashid

---

## 4. Folder Path Structure — LOCKED (v2.0)

```
PM provides ONE root folder path only:
  Example: /Project Alpha

Octiss auto-creates and manages ALL subfolders:
  /Project Alpha/Prepare/In Progress/
  /Project Alpha/Prepare/Review/
  /Project Alpha/Prepare/Signed Off/
  /Project Alpha/Explore/In Progress/
  /Project Alpha/Explore/Review/
  /Project Alpha/Explore/Signed Off/
  ... (same for Realize, Deploy, Run)

Document status drives subfolder:
  Generated/Draft    → /Phase/In Progress/
  Sent for Review    → /Phase/Review/
  Signed Off         → /Phase/Signed Off/

Rule: PM never configures subfolders.
      Octiss manages all folder logic automatically.
```

---

## 5. Agent Behaviour — LOCKED (v2.0)

### Agent Tone Rules
```
RULE 1 — Agents speak DIRECTLY to PM in second person
  WRONG: "RAID/Risk Agent prepared a PMgr-review draft"
  RIGHT: "Here is your action plan for this task"

RULE 2 — System prompts NEVER visible to PM
  Filter: Only role=assistant and role=user shown
  Never show: role=system, role=context messages

RULE 3 — Follow-up responses skip context repetition
  First response: may include brief context summary
  Follow-up: answer question directly, no restating context

RULE 4 — Agents act FIRST, PM decides
  Agent fires automatically on task open
  PM never types questions to start an agent
  PM asks follow-up questions only after initial response
```

---

## 6. Task Management Rules — LOCKED (v2.0)

```
Delete Task: REMOVED ENTIRELY
  Tasks must NEVER be permanently deleted
  Only action available: Exclude Task
  Excluded tasks are hidden but preserved in DB
  Rationale: task library integrity must be maintained
```

---

## 7. Document Flow — LOCKED

```
Three-Tier Field System:
  Tier 1 (black):        Pre-filled by Octiss automatically
  Tier 2 (navy []):      Consultant fills substantive content
  Tier 3 (grey italic):  Guidance text — deleted before submitting

Document Status Flow:
  Generated → In Progress folder (OneDrive)
  Sent for Review → Review folder (OneDrive)
  Signed Off → Signed Off folder (OneDrive)

File movement is automatic on status change.
```

---

## 8. WRICEF Change Management — LOCKED

| Scenario | Trigger | Action |
|---|---|---|
| A — Drop | PM marks dropped | No CR. Log reason. |
| B — Add within count | Net count unchanged | Agent asks PM: notify or silent? |
| C — Exceed count | Active > SOW approved | Auto CR mandatory |

---

## 9. Core UX Layout — LOCKED

### Default — Two Panels
```
LEFT SIDEBAR (~20%) | CENTRE PANEL (~80%)
```

### Triggered — Three Panels
```
LEFT SIDEBAR (~20%) | CENTRE (~45%) | RIGHT PANEL (~35%)
Right panel: slides in on agent response or document generation
Dismissed with [X] — centre expands back
```

### Right Panel Trigger Events
- Agent generates response
- Document draft ready
- Q-Gate sign-off document generated
- MOM generated
- CR draft ready
- FSD generated

---

## 10. Features Built — Complete List (v2.0)

### Core Infrastructure
- ✅ 484 tasks seeded (GF 247, BF 237)
- ✅ 12 specialist agents live
- ✅ 87 delivery templates complete
- ✅ MS365 fully connected
- ✅ n8n 8+ workflows live

### Project Management
- ✅ Project creation Path A (SOW upload + Claude Opus extraction)
- ✅ Project creation Path B (manual wizard + auto task generation)
- ✅ Phase View with deliverable grouping
- ✅ Task Screen (KEEP + TRACK)
- ✅ Task descriptions — all 486 meaningful and specific
- ✅ Agent task flow seeding from Excel files
- ✅ Delete Task removed — Exclude Task only

### Document Management
- ✅ Task-to-Template DB mapping (all 5 GF phases)
- ✅ Document generation endpoint
- ✅ Document file rendering (.docx + .xlsx)
- ✅ TRACK task fallback template
- ✅ Send document via Outlook (Microsoft Graph)
- ✅ OneDrive storage with phase/status folder structure
- ✅ n8n document delivery logging + audit trail
- ✅ MOM auto-generation as .docx
- ✅ FSD per WRICEF item (type-specific: W/R/I/C/E/F)

### Team Management
- ✅ Team Member Registry (add/edit/deactivate)
- ✅ Attendance Management (linked to Q-Gate + MOM)

### RAID and Risk
- ✅ Auto-RAID from overdue TRACK tasks
- ✅ CR Generation Flow (WRICEF auto-trigger)
- ✅ WRICEF Inventory (auto-created, scenarios A/B/C)

### Quality Gates
- ✅ Q-Gate Interactive Screen (three-panel)
- ✅ Q-Gate seeder from real template file
- ✅ Q-Gate sign-off document generation (evidence-based)
- ✅ Phase gate customer sign-off via Outlook
- ✅ Phase unlock on customer confirmation

### Exports and Integration
- ✅ Project Plan Export (.xlsx + MS Project .xml)
- ✅ Calendar Mapping (Graph API auto-sync)

### BF Specific
- ✅ BF Conversion Module (SBX/DEV/QAS/PRD, Pre/SUM/Post)

### Agent Intelligence
- ✅ System prompt hidden from PM
- ✅ Agent speaks directly to PM (second person)
- ✅ Context not repeated on follow-up responses
- ✅ Agent task flow seeded from official Excel mapping files

---

## 11. Features Pending

### E2E Fixes Required
| Issue | Description | Severity |
|---|---|---|
| E2E-12 | Review/Launch screen needs redesign | High |
| E2E-13 | SOW tab duplicates wizard data | High |
| E2E-17 | Ready tab naming confusing | High |
| E2E-18 | Two overlapping setup wizards | High |
| E2E-11 | CSV import broken | Medium |
| E2E-16 | Automation status shows raw "not_run" | Low |
| E2E-15 | Map Tasks guidance text missing | Low |

### E2E Screens Not Yet Tested
Screens 4, 5, 8, 9, 10, 11, 12, 13, 14, 15, 16

### Beta Prep
- Beta tester User Manual
- Beta tester Welcome Pack + email
- Feedback form / survey

### Deferred to Post-Beta
- Document inline editing
- PMP compilation
- Agent verification layer
- Sprint planning integration
- Transport Request Tracker
- Data Migration Run Tracker live
- Hypercare SLA monitoring
- UI/UX design pass (intentionally last)

### Post-M11 (After Revenue)
- Multilingual support
- SF (Bluefield) methodology
- Digital Twin
- Client-facing portal
- Consultant portal

---

## 12. Template Library — COMPLETE

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

## 13. MS365 Configuration

| Item | Value |
|---|---|
| Azure App | Octiss Production |
| Client ID | 3a027ac4-a5f3-452c-b283-42a29c8be118 |
| Permissions | User.Read, Files.ReadWrite, Files.Read.All, Sites.Read.All, Mail.Send, Calendars.ReadWrite, offline_access |
| Status | All granted + admin consent applied |

---

## 14. Production Environment

| Service | URL | Status |
|---|---|---|
| Backend | sap-pmo-agent-production-3f52.up.railway.app | HEALTHY |
| Frontend | octiss-production.vercel.app | LIVE |
| Database | qrxfprybbpqugptakeke — Singapore Pro | ACTIVE |

---

## 15. What Is Excluded from Octiss

- Discover phase
- WhatsApp — removed entirely
- SAP Cloud ALM setup tasks
- ECT/Tricentis
- SAP employee-only tasks
- SF (Bluefield) — deferred
- Multilingual — post-M11
- Digital Twin — post-revenue

---

## 16. Decisions Log (v2.0 Additions)

| Date | Decision |
|---|---|
| 11 Jun 2026 | Folder path: PM provides one root folder. Octiss manages all subfolders by phase + document status automatically. |
| 11 Jun 2026 | Delete Task removed from all surfaces. Exclude Task is the only valid action. Task library integrity must be preserved. |
| 11 Jun 2026 | Agent tone locked: speak directly to PM in second person. System prompts never shown to user. No context repetition on follow-up. |
| 11 Jun 2026 | Mock test files created: Project_Alpha_SOW_v1.0.txt + Project_Alpha_Team_Registry_v1.0.csv on Desktop. Use for E2E testing. |
| 11 Jun 2026 | 2 ABAP consultants (Robert Taylor + Nina Kowalski) and 2 Basis consultants (Lisa Zhang + Hassan Al-Rashid) in mock team — tests multi-consultant module assignment in agent recommendations. |
| 11 Jun 2026 | 26 Codex prompts executed today. Tests 1235→1320 (+85). All major product features now built. E2E verification started — 31 issues logged, 7 critical issues fixed same day. |

---

*End of Document — Octiss SUD v2.0 — 11 June 2026*
*Supersedes: Octiss_SUD_v1.9.md*
*Read alongside: Octiss_Master_Handoff_v1.3.md*
*Next update: v2.1 after E2E verification complete and remaining issues fixed*
