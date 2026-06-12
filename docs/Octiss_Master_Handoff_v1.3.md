# Octiss — Master Handoff Document
**Version:** v1.3 | **Date:** 11 June 2026
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

4. WhatsApp is NOT part of Octiss.
   All external communications via Outlook only.

5. UI/UX design is deferred.
   Build all functionality first. E2E verify everything.
   UI/UX polish is the final step before beta.
```

---

## 1. What Is Octiss

Octiss is an AI-powered SAP PMO Intelligence Platform. It guides SAP Project Managers through SAP Activate — Prepare → Explore → Realize → Deploy → Run — with 12 specialist AI agents embedded directly into the workflow.

---

## 2. Live URLs

| Item | URL / Path |
|---|---|
| Frontend | https://octiss-production.vercel.app |
| Backend | https://sap-pmo-agent-production.up.railway.app |
| Frontend Repo | mohsinsardar-ai/em-intelligence-labs-dashboard |
| Backend Repo | mohsinsardar-ai/sap-pmo-agent |
| Database | Supabase Singapore — qrxfprybbpqugptakeke |
| SUD v2.0 | docs/Octiss_SUD_v2.0.md (frontend repo) |
| Codex Prompt Queue | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\Octiss_Codex_Prompt_Queue_v1.0.txt |
| Production Credentials | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\production_credentials.txt |

---

## 3. Current Build State (11 June 2026 — End of Day)

### Metrics
- Backend tests: **1320 passing** (+85 added today)
- Backend latest commit: `92ad415`
- Frontend latest commit: `775e1ee`
- Both repos pushed clean to origin/main

### What Is Live and Working

| Feature | Status |
|---|---|
| Project creation Path B (manual wizard) | ✅ Auto-generates 193 GF / 201 BF tasks |
| Project creation Path A (SOW upload) | ✅ Claude Opus extracts fields |
| Phase View with deliverable grouping | ✅ |
| Task Screen (KEEP + TRACK) | ✅ |
| Agent Assist panel | ✅ System prompt hidden, speaks to PM directly |
| Agent Task Flow seeding from Excel | ✅ |
| Issue 10 — task descriptions | ✅ All 486 tasks have meaningful descriptions |
| Generate Document CTA | ✅ TRACK fallback template added |
| Document file rendering (.docx/.xlsx) | ✅ |
| Send document via Outlook | ✅ |
| OneDrive document storage | ✅ Phase/status folder structure |
| n8n document delivery logging | ✅ |
| Team Member Registry | ✅ |
| Task-to-Template DB mapping | ✅ All 5 GF phases seeded |
| Auto-RAID from overdue TRACK | ✅ |
| CR Generation Flow | ✅ WRICEF auto-trigger + Outlook send |
| WRICEF Inventory | ✅ Auto-created, scenarios A/B/C |
| FSD per WRICEF item | ✅ Type-specific templates |
| Q-Gate Interactive Screen | ✅ Real template items loading |
| Q-Gate Sign-off document | ✅ Evidence-based |
| Phase gate sign-off via Outlook | ✅ Phase unlock on confirmation |
| Attendance Management | ✅ Linked to Q-Gate + MOM |
| MOM auto-generation as .docx | ✅ Send to attendees |
| BF Conversion Module | ✅ SBX/DEV/QAS/PRD |
| Project Plan Export (.xlsx + .xml) | ✅ |
| Calendar Mapping (Graph API) | ✅ |
| Delete Task | ✅ Removed — Exclude Task only |
| communication_drafts table | ✅ Created |
| 87 delivery templates | ✅ Complete |
| 5 beta tester accounts | ✅ Registered — not yet sent |

---

## 4. E2E Verification — STATUS

**Started but NOT complete.** 31 issues logged.

### Screens Tested
| Screen | Status |
|---|---|
| Screen 1 — Login | ⚠️ Partial |
| Screen 2 — Command Center | ⚠️ Partial |
| Screen 3 — Project Creation Path B | ⚠️ Partial |
| Screen 6/7 — Task Screen | ⚠️ Partial |
| Screens 4,5,8-16 | ⏳ Not yet tested |

### Critical Issues — ALL FIXED
| Issue | Fix |
|---|---|
| E2E-19: No tasks after project creation | CF-1 ✅ |
| E2E-26: Generate Document not found | CF-2 ✅ |
| E2E-27: communication_drafts missing | CF-2 ✅ |
| E2E-28: System prompt visible | CF-3 ✅ |
| E2E-29: Agent wrong tone | CF-3 ✅ |
| E2E-09: Module not dropdown | CF-4 ✅ |
| E2E-20/21: Delete Task visible | CF-4 ✅ |

### Remaining High Priority Issues
| Issue | Description |
|---|---|
| E2E-12 | Review/Launch screen needs redesign |
| E2E-13 | SOW tab duplicates wizard data |
| E2E-17 | Ready tab naming confusing |
| E2E-18 | Two overlapping setup wizards |
| E2E-11 | CSV import broken |

---

## 5. Test Files on Desktop

| File | Purpose |
|---|---|
| `Project_Alpha_SOW_v1.0.txt` | Upload for Path A SOW extraction test |
| `Project_Alpha_Team_Registry_v1.0.csv` | Upload for CSV team import test |

Team includes 2 ABAP (Robert Taylor + Nina Kowalski) and 2 Basis (Lisa Zhang + Hassan Al-Rashid) consultants — tests multi-consultant module assignment in agent recommendations.

---

## 6. Pending Items (Priority Order)

| # | Item | Type | Status |
|---|---|---|---|
| 1 | Complete E2E verification — all 16 screens | Verification | In progress |
| 2 | Test CF fixes on production | Verification | Next session |
| 3 | Fix E2E-12 Review/Launch redesign | Bug/UX | Pending |
| 4 | Fix E2E-13 SOW tab duplication | Bug | Pending |
| 5 | Fix E2E-17/18 Two wizard flows | Bug/UX | Pending |
| 6 | Fix E2E-11 CSV import | Bug | Pending |
| 7 | Beta tester User Manual | Content | On hold |
| 8 | Beta tester Welcome Pack + email | Content | On hold |
| 9 | Feedback form / survey | Content | On hold |
| 10 | Group J flowchart PPTX (31st) | Template | Pending |
| 11 | UI/UX design pass | Design | Deferred |

---

## 7. The 12 Specialist Agents

| # | Agent | Model | Role |
|---|---|---|---|
| 1 | SAP Activate Governance | Opus | Phase gates, quality gates |
| 2 | Project Plan Agent | Opus | SOW extraction, task generation |
| 3 | Prepare Agent | Sonnet | Prepare phase guidance |
| 4 | Explore Agent | Opus | FTS, BPML, gap analysis, WRICEF |
| 5 | Realize Agent | Sonnet | Configuration, testing |
| 6 | Deploy Agent | Opus | Cutover, go-live |
| 7 | Run/Hypercare Agent | Sonnet | Hypercare, closure |
| 8 | Stakeholder Communication | Sonnet | Sign-offs, CRs, Outlook |
| 9 | Meeting Intelligence | Sonnet | Workshops, MOM, attendance |
| 10 | RAID/Risk Agent | Sonnet | Risks, issues, escalation |
| 11 | Brownfield Conversion | Opus | BF conversion, SUM execution |
| 12 | Daily PM Briefing | Sonnet | 8AM briefing via n8n |

---

## 8. Locked Decisions (v1.3 Additions)

| Decision | Detail |
|---|---|
| Folder path structure | PM provides ONE root folder. Octiss manages subfolders by phase + document status (In Progress / Review / Signed Off) |
| Delete Task | Removed entirely. Exclude Task is the only valid action. |
| Agent tone | Agents speak directly TO the PM in second person. Never third-person. |
| System prompt | Never visible to PM. Filtered from agent panel display. |
| Mock test files | Project_Alpha_SOW_v1.0.txt + Project_Alpha_Team_Registry_v1.0.csv on Desktop |

---

## 9. Key File Locations

| File | Path |
|---|---|
| Backend repo | D:\Mohsin Personal\OneDrive\Automation\SAP PMO Agent\sap-pmo-agent |
| Frontend repo | D:\Mohsin Personal\OneDrive\Automation\EM Intelligence Labs Dashboard |
| Templates | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\OCTISS OFFICIAL TEMPLATES\ |
| Q-Gate Template | sap-pmo-agent\data\Q_Gate_Checklist_Octiss_Template.xlsx |
| Agent Task Flows GF | sap-pmo-agent\data\Octiss_Agent_Task_Flows_v1.0.xlsx |
| Agent Task Flows BF | sap-pmo-agent\data\Octiss_Agent_Task_Flows_BF_v1.0.xlsx |
| Mock SOW | Desktop\Project_Alpha_SOW_v1.0.txt |
| Mock Team CSV | Desktop\Project_Alpha_Team_Registry_v1.0.csv |
| Continuation Prompt | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\Octiss_Continuation_Prompt.txt |
| This Handoff v1.3 | docs\Octiss_Master_Handoff_v1.3.md |

---

## 10. Production Environment

| Service | URL / ID | Status |
|---|---|---|
| Backend (Railway) | sap-pmo-agent-production-3f52.up.railway.app | HEALTHY |
| Frontend (Vercel) | octiss-production.vercel.app | LIVE |
| Database (Supabase) | qrxfprybbpqugptakeke — Singapore Pro | ACTIVE |

---

## 11. Infrastructure Notes

- Windows: Notification mode — TMC IT arranging new 512GB drive
- OneDrive: Paused — will resume when new drive installed
- Files safe locally on D: drive
- C: drive: ~15GB free

---

*End of Document — Octiss Master Handoff v1.3 — 11 June 2026*
*Supersedes: Octiss_Master_Handoff_v1.2.md*
*Read alongside: Octiss_SUD_v2.0.md*
