# Data Model

ClaimShield AI uses eight core entities.

---

## Claims

The central record for every insurance claim submission.

| Field | Type | Notes |
|---|---|---|
| `claimant_name` | string | Required |
| `claim_type` | enum | Auto Collision, Property Damage, Medical, Liability, Workers Comp, Theft, Natural Disaster |
| `claim_amount` | number | Required |
| `incident_date` | date | Required |
| `description` | string | Incident narrative |
| `policy_tenure` | number | Years the policy has been active |
| `prior_claims_count` | number | Historical claims on the account |
| `supporting_notes` | string | Document summaries or adjuster notes |
| `status` | enum | Pending Analysis → Under Review → Approved / Routed to Adjuster / Escalated to SIU / More Info Requested |
| `photos` | array | Each photo has `url`, `is_live` (captured in-app vs uploaded), and `analysis` (AI result) |

---

## AI_Assessments

One assessment record per claim, written by the fraud-scoring function.

| Field | Type | Notes |
|---|---|---|
| `claim_id` | string | FK → Claims |
| `fraud_score` | number | 0–100 |
| `risk_level` | enum | Low, Medium, High, Critical |
| `explanation` | string | Human-readable rationale |
| `top_reasons` | string[] | Top contributing risk factors |
| `recommended_action` | enum | approve_normal_processing, send_to_adjuster, escalate_fraud_review, request_more_information |
| `confidence` | number | Model confidence % |

---

## Claimants

Policyholder profiles linked to claims.

| Field | Type | Notes |
|---|---|---|
| `full_name` | string | |
| `email` | string | |
| `phone` | string | |
| `policy_number` | string | FK → Policies |
| `date_of_birth` | date | |
| `address` | string | |

---

## Policies

Insurance policy records.

| Field | Type | Notes |
|---|---|---|
| `policy_number` | string | Unique identifier |
| `policy_type` | string | |
| `coverage_amount` | number | |
| `start_date` | date | |
| `end_date` | date | |
| `claimant_id` | string | FK → Claimants |

---

## ClaimantInterview

A proctored interview session linked to a claim. Stores questions, responses, integrity violations, and AI deception analysis.

| Field | Type | Notes |
|---|---|---|
| `claim_id` | string | FK → Claims |
| `claimant_name` | string | |
| `claimant_email` | string | |
| `claim_type` | string | |
| `incident_date` | string | |
| `status` | enum | Pending, In Progress, Completed, Expired |
| `questions` | array | Each item: `id`, `question`, `category` |
| `responses` | array | Each item: `question_id`, `question`, `answer`, `duration_seconds` |
| `violations` | array | Proctoring events: `type`, `message`, `timestamp` (tab switches, copy/paste, etc.) |
| `ai_analysis` | object | `credibility_score`, `risk_level`, `summary`, `top_risk_indicators`, `per_response_flags` |
| `integrity_score` | number | 0–100 composite score |
| `expires_at` | string | Link expiry timestamp |
| `investigator_notes` | string | |

---

## Fraud_Signals

Individual fraud indicators linked to a claim, used to explain the AI score.

| Field | Type | Notes |
|---|---|---|
| `claim_id` | string | FK → Claims |
| `signal_type` | string | e.g. "late_reporting", "inconsistent_timeline" |
| `severity` | enum | Low, Medium, High |
| `description` | string | Human-readable signal detail |

---

## Investigations

Tracks investigator assignment and outcome for a claim.

| Field | Type | Notes |
|---|---|---|
| `claim_id` | string | FK → Claims |
| `investigator_id` | string | Assigned user |
| `stage` | enum | Intake, AI Review, Adjuster, SIU, Resolved |
| `outcome` | string | Final disposition |
| `notes` | string | |

---

## Review_Actions

Audit trail of every human decision made on a claim.

| Field | Type | Notes |
|---|---|---|
| `claim_id` | string | FK → Claims |
| `actor` | string | User who took the action |
| `action` | string | Decision taken |
| `timestamp` | string | |
| `notes` | string | Optional comment |
