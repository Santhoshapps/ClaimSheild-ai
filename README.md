# ClaimShield AI — Insurance Fraud Detection Platform

A full-stack AI-powered insurance fraud detection and claims management platform built for modern insurance operations.

---

## Overview

ClaimShield AI combines real-time fraud scoring, live photo evidence verification, NLP-powered claimant interview analysis, and a full compliance audit trail into one unified system.

---

## Features

### 🔍 AI Fraud Detection
- Multi-layer fraud scoring across pattern rules, NLP document intelligence, and network/contextual signals
- Fraud scores (0–100) with risk levels: Low, Medium, High, Critical
- Recommended triage actions: Approve, Route to Adjuster, Escalate to SIU, Request More Info

### 📸 Live Photo Evidence
- Claimants capture timestamped, verified photos directly in-app
- Real-time AI analysis of damage severity and accident validity
- Credibility bonus applied to claims with live in-app photos

### 🎙️ Proctored Claimant Interviews
- Investigators send secure, time-limited interview links to claimants
- AI generates personalized, claim-specific questions based on fraud signals
- Full proctoring: camera feed, screen sharing, tab-switch detection, copy/paste blocking
- Voice-to-text transcription for claimant answers
- NLP deception analysis: credibility score, per-response flags, risk indicators

### 📋 Investigation Workflow
- Kanban-style pipeline: Intake → AI Review → Adjuster → SIU → Resolved
- Drag-and-drop style triage view per investigator
- AI Fraud Agent Chat — conversational copilot for each claim

### 📁 Claimant Portal
- Self-service portal for policyholders to file claims, upload evidence, and track status
- 3-step claim wizard with AI fraud pre-scoring before submission
- Policy overview and claims history

### 📊 Compliance Audit Log
- Unified event timeline: submissions, AI scores, reviewer decisions
- Filterable by action type, actor, claim ID, and date range
- CSV export for compliance reporting
- Stats: fraud detection rate, avg resolution time, total fraud prevented

---

## User Roles

| Role | Access |
|------|--------|
| **Admin / Investigator** | Full dashboard, claims intake, AI analysis, SIU escalation, audit log, interview management |
| **Claimant** | Claimant portal (file claims, upload photos, track status) |

---

## Tech Stack

- **Frontend:** React 18, Tailwind CSS, shadcn/ui, Framer Motion, Recharts
- **Backend:** Base44 (serverless Deno functions, entity database)
- **AI:** Base44 LLM integration (GPT-4o-mini by default)
- **Auth:** Base44 AuthProvider (role-based: admin / user)
- **Storage:** Base44 file upload (photos, documents)

---

## Pages

| Route | Description |
|-------|-------------|
| `/` | Landing page — login portal for claimants and admins |
| `/dashboard` | Admin overview — claim stats and recent activity |
| `/claims` | Claims list with fraud scores and status filters |
| `/claims/new` | 3-step new claim intake with AI pre-scoring |
| `/claims/:id` | Claim detail — AI assessment, review actions, agent chat |
| `/workflow` | Kanban investigation pipeline |
| `/interview` | Upload & analyze interview transcripts |
| `/claimant-interviews` | Manage & send proctored interview links |
| `/investigator` | Investigator dashboard with triage workbench |
| `/audit` | Compliance audit log |
| `/my-claims` | Claimant self-service portal |
| `/claimant-interview/:id` | Proctored interview session (claimant-facing) |

---

## Entities

- **Claims** — Core claim records
- **Claimants** — Policyholder profiles
- **Policies** — Insurance policy records
- **AI_Assessments** — Fraud scores and risk analysis per claim
- **Fraud_Signals** — Individual fraud indicators linked to claims
- **Investigations** — Investigator assignments and outcomes
- **ClaimantInterview** — Interview sessions, questions, responses, AI analysis
- **Review_Actions** — Reviewer decisions and audit trail entries

---

## Custom Domain

This app is deployed at **[claimsai.work](https://claimsai.work)**.

Interview links sent to claimants follow the format:
```
https://claimsai.work/claimant-interview/{interview-id}
```

---

## Getting Started

1. **Admin users** log in and are redirected to `/dashboard`
2. **Claimants** log in and are redirected to `/my-claims`
3. New claims can be filed from `/claims/new` or through the claimant portal
4. Run AI analysis on any claim from the claim detail page
5. Send proctored interviews from `/claimant-interviews