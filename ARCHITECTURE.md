# Architecture

## Overview

ClaimShield AI is a single-page React application backed by serverless Deno functions and a managed entity database. The frontend is statically deployed; all AI and data operations run through the backend function layer.

```
Users (Admin / Investigator / Claimant)
        │
        ▼
┌─────────────────────────────────────────┐
│              Frontend (React 18)         │
│  Admin pages │ Investigator │ Claimant  │
│  portal      │ workbench    │ portal    │
└──────────────────────┬──────────────────┘
                       │ REST / SDK
                       ▼
┌─────────────────────────────────────────┐
│         Backend (Serverless Deno)        │
│  Auth API │ Claims API │ Interview API  │
│           │            │ Audit API      │
└──────┬────────────┬───────────┬─────────┘
       │            │           │
       ▼            ▼           ▼
  Entity DB    LLM API      File Storage
  (Base44)   (GPT-4o-mini)
```

---

## Frontend

Built with React 18, Vite, Tailwind CSS, shadcn/ui, and Framer Motion.

**State management** uses TanStack Query v5 for server state (caching, background refetch, optimistic updates) and React Hook Form + Zod for form state and validation.

**Routing** uses React Router v6 with role-based redirect logic — admins land on `/dashboard`, claimants land on `/my-claims`.

**Key component groups:**

- `FraudScoreResult` — renders the 0–100 score gauge, risk badge, top reasons, and recommended action
- `FraudAgentChat` — streaming AI chat copilot scoped to a single claim
- `InvestigationDrawer` — slide-out panel for investigator actions and status transitions
- `LiveCameraCapture` — browser camera API wrapper with timestamp injection
- `AnnotatedTranscript` — interview transcript viewer with per-response deception flags highlighted inline
- `ReviewActionButtons` — triage action bar (Approve / Route to Adjuster / Escalate / Request Info)

---

## Backend functions

Two primary serverless functions handle the AI workload:

**`claimsSubmit`** — Receives a new claim payload, persists it, then triggers initial fraud pre-scoring before returning to the client.

**`scoreClaim`** — Full fraud scoring pipeline. Runs pattern-rule checks (policy tenure, prior claims, incident timing), NLP analysis of the incident narrative, and network/contextual signal checks. Produces a fraud score, risk level, top reasons, and a recommended action. Writes results to `AI_Assessments` and `Fraud_Signals`.

All other CRUD operations (claims list, status updates, audit log writes, interview management) go through the entity database SDK directly from the frontend with server-side auth enforcement.

---

## AI layer

The fraud scoring and interview analysis features use an LLM (GPT-4o-mini by default) via the backend function layer. The frontend never calls the LLM API directly — all prompts and responses are mediated by the serverless functions, which also handle input sanitisation, output parsing, and audit logging.

**Fraud scoring inputs:** claim type, amount, incident description, policy tenure, prior claims count, photo credibility signals, supporting document summary.

**Interview NLP inputs:** per-question response text, response duration, proctoring violation log. Output includes a credibility score, per-response flags (evasion, inconsistency, hedging), and an overall risk summary.

---

## Auth

Role-based auth with two roles: `admin` (full dashboard access) and `user` (claimant portal only). Auth state is managed via a React context provider that wraps the entire app. Protected routes redirect unauthenticated users to the login page.

---

## Data flow — new claim submission

```
Claimant fills 3-step wizard
        │
        ▼
ClaimForm validates with Zod
        │
        ▼
claimsSubmit function called
        │
        ├── Persist claim record
        ├── Run AI pre-score
        └── Return claim ID + initial score
                │
                ▼
        Redirect to /claims/:id
                │
                ▼
        Full scoreClaim triggered
                │
                ├── Write AI_Assessment
                ├── Write Fraud_Signals
                └── Update claim status
```

---

## Data flow — proctored interview

```
Investigator creates interview from /claimant-interviews
        │
        ▼
ClaimantInterview record created (status: Pending)
Secure link generated → emailed to claimant
        │
        ▼
Claimant opens /claimant-interview/:id
        │
        ├── Proctoring starts (camera, screen share, event listeners)
        ├── AI generates personalised questions from claim context
        └── Claimant answers via voice-to-text
                │
                ▼
        Session submitted
                │
                ├── Violations log persisted
                ├── NLP deception analysis run
                └── ai_analysis + integrity_score written back
                        │
                        ▼
                Investigator reviews annotated transcript
```
