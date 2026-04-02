# ClaimShield AI

> AI-powered insurance fraud detection and claims management platform.

Live demo: [claimsai.work](https://claimsai.work)

---

## What it does

ClaimShield AI helps insurance investigators detect fraud faster by combining real-time AI scoring, proctored claimant interviews, live photo evidence verification, and a full compliance audit trail — all in one unified system.

---

## Key features

**AI fraud detection** — Every claim gets a fraud score (0–100) computed from pattern rules, NLP document analysis, and network signals. The score maps to a risk level (Low / Medium / High / Critical) and a triage recommendation (Approve, Route to Adjuster, Escalate to SIU, Request More Info).

**Proctored claimant interviews** — Investigators send secure, time-limited interview links. The session enforces full proctoring: camera feed, screen sharing, tab-switch detection, copy/paste blocking. Claimant answers are transcribed via voice-to-text and analysed for deception signals (credibility score, per-response flags, risk indicators).

**Live photo evidence** — Claimants capture timestamped photos directly in-app. AI analyses damage severity and accident validity in real time. A credibility bonus is applied to live in-app photos vs. uploads.

**Investigation workflow** — Kanban pipeline (Intake → AI Review → Adjuster → SIU → Resolved) with an AI fraud agent chat copilot on every claim detail page.

**Compliance audit log** — Unified event timeline across submissions, AI scores, and reviewer decisions. Filterable, CSV-exportable, with stats on fraud detection rate and avg. resolution time.

**Claimant portal** — Self-service portal for policyholders to file claims via a 3-step wizard, upload evidence, and track status.

---

## Tech stack

| Layer | Tech |
|---|---|
| Frontend | React 18, Tailwind CSS, shadcn/ui, Framer Motion, Recharts |
| Routing | React Router v6 |
| State / data | TanStack Query v5 |
| Forms | React Hook Form + Zod |
| Backend | Serverless Deno functions (Base44) |
| AI | LLM API (GPT-4o-mini) via backend functions |
| Auth | Role-based auth — admin / claimant |
| Storage | File upload for photos and documents |

---

## Pages

| Route | Description |
|---|---|
| `/` | Landing / login |
| `/dashboard` | Admin overview — stats and recent activity |
| `/claims` | Claims list with fraud score filters |
| `/claims/new` | 3-step claim intake with AI pre-scoring |
| `/claims/:id` | Claim detail — AI assessment, review actions, agent chat |
| `/workflow` | Kanban investigation pipeline |
| `/interview` | Upload & analyse interview transcripts |
| `/claimant-interviews` | Manage & send proctored interview links |
| `/investigator` | Investigator triage workbench |
| `/audit` | Compliance audit log |
| `/my-claims` | Claimant self-service portal |
| `/claimant-interview/:id` | Proctored interview session (claimant-facing) |

---

## User roles

| Role | Access |
|---|---|
| Admin / Investigator | Full dashboard, claims intake, AI analysis, SIU escalation, audit log, interview management |
| Claimant | Self-service portal — file claims, upload photos, track status |

---

## Project structure

```
src/
├── components/         # Shared UI components
│   ├── ui/             # shadcn/ui primitives
│   ├── FraudScoreResult.jsx
│   ├── FraudAgentChat.jsx
│   ├── InvestigationDrawer.jsx
│   ├── LiveCameraCapture.jsx
│   ├── AnnotatedTranscript.jsx
│   └── ...
├── pages/              # Route-level page components
├── lib/                # Auth context, query client, utilities
├── hooks/              # Custom React hooks
└── api/                # API client setup
base44/
├── entities/           # Data model schemas (JSON)
└── functions/          # Serverless backend functions
```

---

## Getting started (local)

```bash
# 1. Clone the repo
git clone <your-repo-url>
cd claimshield-ai

# 2. Install dependencies
npm install

# 3. Set environment variables
cp .env.example .env.local
# Fill in VITE_BASE44_APP_ID and VITE_BASE44_APP_BASE_URL

# 4. Run dev server
npm run dev
```

---

## Data model

See [`DATA_MODEL.md`](./DATA_MODEL.md) for entity schemas.

See [`ARCHITECTURE.md`](./ARCHITECTURE.md) for system design.
