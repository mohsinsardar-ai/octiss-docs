# Octiss — Master Handoff Document
**Version:** v1.2 | **Date:** 11 June 2026
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

5. WhatsApp is NOT part of Octiss.
   All external communications via Outlook only.
   Do not implement or reference WhatsApp anywhere.

6. UI/UX design is deferred.
   Build all functionality first.
   E2E verify everything.
   UI/UX polish is the final step before beta.
```

---

## 1. What Is Octiss

Octiss is an AI-powered SAP PMO Intelligence Platform. It guides SAP Project Managers through the SAP Activate methodology — Prepare → Explore → Realize → Deploy → Run — with 12 specialist AI agents that surface as contextual CTAs on task screens.

**NOT a dashboard. NOT a tool suite. A guided SAP Activate journey.**

---

## 2. Live URLs

| Item | URL / Path |
|---|---|
| Frontend | https://octiss-production.vercel.app |
| Backend | https://sap-pmo-agent-production.up.railway.app |
| Frontend Repo | mohsinsardar-ai/em-intelligence-labs-dashboard |
| Backend Repo | mohsinsardar-ai/sap-pmo-agent |
| Database | Supabase Singapore — qrxfprybbpqugptakeke |
| SUD v1.9 | docs/Octiss_SUD_v1.9.md (frontend repo) |
| Production Credentials | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\production_credentials.txt |
| Codex Prompt Queue | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\Octiss_Codex_Prompt_Queue_v1.0.txt |

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

## 4. Current Build State (11 June 2026)

### What Is Live and Working

- Backend: 365 routes, **1311 tests passing**
- Frontend: Deployed Vercel, build passing
- Both repos pushed and clean on origin/main
- Layer 1 cleanup complete — 6,500+ lines removed
- Issues 1–9: all fixed and committed
- Setup 5/5 READY — all setup steps complete in production
- MS365 fully connected — OneDrive, Outlook, Calendar verified
- 484 tasks seeded (GF 247, BF 237)
- 12 specialist agents built and live
- 87 delivery templates built and documented
- 5 beta tester accounts registered (tester1-5@octiss.com) — credentials NOT yet sent
- Q-Gate template in repo: data/Q_Gate_Checklist_Octiss_Template.xlsx

### Completed 11 June 2026 — Codex Session (12 Prompts)

| Prompt | Feature | Backend Commit | Frontend Commit |
|---|---|---|---|
| 01 | Issue 9 — Agent formatting | f4ec2d5 | 50d69e3 |
| 02 | Phase View deliverable grouping | e8098092 | 0d9c2d2f |
| 03 | RoadmapAgent rewire + WhatsApp retired | 5396acb5 | — |
| 04 | Team Member Registry | 5eeff791 | — |
| 05 | Task-to-Template DB mapping | 10c96256 | — |
| 06 | Auto-RAID from overdue TRACK | 692cd4f | — |
| 07 | CR Generation Flow E2E | 4aa41af2 | 95e535fb |
| 08 | Q-Gate Interactive Screen | 72ab853b | 5680ef8a |
| 09 | Attendance Management | 138b3641 | b14e42f4 |
| 10 | BF Conversion Module | 84a68cc1 | a23618ea |
| 11 | Project Plan Export (.xlsx + .xml) | 901ad0e | b8d006b |
| 12 | Calendar Mapping (Graph API) | 87868171 | 8200e02f |

### Pending Items (Priority Order)

| # | Item | Type | Status |
|---|---|---|---|
| 1 | Issue 10 — Generic task descriptions | Bug | After Issue 9 confirmed working |
| 2 | E2E verification — all 16 screens | Verification | Required before beta |
| 3 | Document Generation and Send to Team | Feature | Not built — spec in SUD v1.9 Section 9 |
| 4 | Q-Gate seeder verification | Verification | Template in repo — needs live test |
| 5 | Phase gate unlock E2E | Verification | Built — needs live test |
| 6 | Schedule Session → Outlook send E2E | Verification | Not confirmed live |
| 7 | Auto-RAID E2E live test | Verification | Built — needs live test |
| 8 | CR generation flow live test | Verification | Built — needs live test |
| 9 | Beta tester prep (User Manual, Welcome Packs) | Content | On hold |
| 10 | Group J flowchart PPTX (31st template) | Template | 30 of 31 done |
| 11 | UI/UX design pass | Design | Deferred — last step before beta |

---

## 5. The 12 Specialist Agents

| # | Agent | Model | Primary Role |
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

## 6. Key Design Decisions Locked

| Decision | Detail |
|---|---|
| Project creation flow | SOW upload FIRST → extraction → PM confirms → plan generated |
| CTA state machine | Sequential — only next required action shown |
| Agent behaviour | Proactive — agents read context and deliver output first |
| WhatsApp | Removed entirely. Outlook only. |
| Phase Gate | Evidence-based auto-populated draft. Phase unlocks on customer sign-off. |
| UI layout | Left sidebar 20%, centre 80%, right panel slides in on output |
| UI/UX build timing | Deferred to post-functionality-complete |
| Q-Gate source | data/Q_Gate_Checklist_Octiss_Template.xlsx — 4 phases seeded to DB |
| Document flow | Agents pre-populate every template — consultants fill on-ground knowledge only |
| WRICEF CR trigger | Exceeds SOW count → auto CR. Within count → substitution notice. Drop → no CR. |

---

## 7. Milestone History

| Milestone | What Was Built |
|---|---|
| M1–M8D | RAG foundation, ingestion, MS Graph, PMO intelligence, SAP Knowledge Base |
| M9 | 11 specialist agents live |
| M10 | Enterprise hardening, billing, voice copilot, rebrand to Octiss |
| M10b | SAP Activate Workflow Engine — 484 tasks, 12 agents mapped, sidebar, Command Center, task screen |
| Layer 1 Cleanup | 6,500+ lines removed. Issues 1–8 fixed. |
| 09 Jun 2026 | 87 templates complete. SUD v1.8 published. WRICEF flow locked. |
| 11 Jun 2026 | 12 Codex prompts executed. Tests 1235→1311. All major features built. |

---

## 8. Key File Locations

| File | Path |
|---|---|
| Backend repo | D:\Mohsin Personal\OneDrive\Automation\SAP PMO Agent\sap-pmo-agent |
| Frontend repo | D:\Mohsin Personal\OneDrive\Automation\EM Intelligence Labs Dashboard |
| Q-Gate Template | sap-pmo-agent\data\Q_Gate_Checklist_Octiss_Template.xlsx |
| Templates | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\OCTISS OFFICIAL TEMPLATES\ |
| Production Credentials | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\production_credentials.txt |
| Codex Prompt Queue | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\Octiss_Codex_Prompt_Queue_v1.0.txt |
| Continuation Prompt | D:\Mohsin Personal\OneDrive\EM Intelligence Lab\Octiss Production\Octiss_Continuation_Prompt.txt |
| SUD v1.9 | D:\Mohsin Personal\OneDrive\Automation\EM Intelligence Labs Dashboard\docs\Octiss_SUD_v1.9.md |
| This Handoff v1.2 | D:\Mohsin Personal\OneDrive\Automation\EM Intelligence Labs Dashboard\docs\Octiss_Master_Handoff_v1.2.md |

**Disk warning: C: drive ~15GB free — keep Codex sessions lean, no Playwright**

---

## 9. Production Environment

| Service | URL / ID | Status |
|---|---|---|
| Backend (Railway) | sap-pmo-agent-production-3f52.up.railway.app | HEALTHY |
| Frontend (Vercel) | octiss-production.vercel.app | LIVE |
| Database (Supabase) | qrxfprybbpqugptakeke — Singapore Pro | ACTIVE |

---

## 10. Infrastructure Notes

- Windows on this machine is in **Notification mode** (not activated)
- OneDrive sync is paused — contact TMC IT to activate Windows
- Files are safe locally on D: drive
- Both git repos are clean and pushed to origin/main

---

*End of Document — Octiss Master Handoff v1.2 — 11 June 2026*
*Supersedes: Octiss_Master_Handoff_v1.1.md*
*Read alongside: Octiss_SUD_v1.9.md*
