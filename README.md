<!-- Review note: business overview, scoring rules, MVP scope and the scoring-platform workstream (section 9). Placeholders throughout; S worked example should read 73.4375. -->
# Important links
https://claude.ai/artifact/EQcNFAVjLf1fHTb4yat4Do

# Prior Authorization Copilot: Business Overview

> **Status:** Draft v0.1. This is a business repo. It holds the business rules and project overview, and no code.
> All weights, thresholds and cut-offs below are **starting placeholders** until calibrated against reviewer decisions.

---

## 1. The problem

Before a doctor can order many tests, procedures or drugs, the patient's insurer has to approve them. This is called **prior authorization**. It is slow and frustrating for everyone:

- **Doctors' offices** spend hours on paperwork and phone calls.
- **Patients** wait days without knowing what is happening or why.
- **Insurer staff** review a mountain of requests. Many are routine, but a few are risky, and it is hard to tell which is which.
- **Analysts** cannot easily see where requests get stuck or where money goes.

Federal rules set decision deadlines: **72 hours** for expedited requests and **7 calendar days** for standard ones.

## 2. The idea

An AI-assisted system that does two jobs:

1. **Sorts requests by how well the paperwork supports them.** It reads clinical documentation, checks it against coverage rules, scores it, and routes the request to the right level of review.
2. **Talks to people.** A chatbot lets patients, physicians and the insurer's analytics team ask questions in plain language and get answers from the same underlying data.

## 3. Users

| Person | Typical questions | What they get | Data they can see |
|---|---|---|---|
| **Patient / member** | "Where is my request? What do I need to do?" | Plain-language status and next steps | Their own case only |
| **Physician / provider** | "Why is this flagged? What is missing?" | The unmet criteria and the document that would fix it | Their attributed patients and requests |
| **Insights team** | "Where are requests getting stuck?" | Summary numbers and trends | Aggregates only, with small-count suppression (hide counts under 11) |

Login is role-based. Access control is enforced in the data and tool layer on the server, never by instructions to the AI.

## 4. Principles (non-negotiable)

1. **The system never denies.** It can recommend approval or hand the case to a human. Adverse determinations belong to licensed clinicians.
2. **Missing evidence never helps.** Absent or stale information can only keep a case at the same tier or lower it.
3. **Safety rules cannot be outvoted.** A high score never overrides a hard gate (section 6).
4. **Every number is explainable.** The chatbot reports results from the scoring system. It does not invent or recompute them.
5. **Privacy by design.** Identifying details are removed before text reaches any AI model, and the audit log stores no raw patient information.
6. **Everything is replayable.** Any past decision can be re-derived exactly as the system saw it at the time.
7. **No protected attributes** (race, sex, age band, etc.) are used as scoring inputs. Outcomes are audited by group.

## 5. How scoring works

### Pipeline

```
request + notes + claims
  -> remove identifying details
  -> extract clinical facts (each with a verbatim quote)
  -> build facts as of the decision time
  -> hard gates
  -> five factor scores (0-100), adjusted for evidence confidence
  -> composite score S, confidence C, uncertainty band
  -> rating -> routing tier A-D
  -> audit entry (replayable)
```

### Factors and weights (sum to 1)

| Factor | Weight | What it measures |
|---|---|---|
| Clinical necessity | 0.35 | Criteria met, diagnosis-to-service fit, severity, red flags |
| Conservative care | 0.20 | Duration vs policy minimum, adherence, failed treatments |
| Documentation completeness | 0.20 | Required elements present, recency of key results, consistency |
| Guideline alignment | 0.15 | Appropriateness rating, policy version match |
| Utilization context | 0.10 | Duplicate or recent similar service, ordering outliers, site of service |

Sub-score weights within each factor:

| Factor | Sub-scores |
|---|---|
| Clinical necessity | criteria met 0.45, diagnosis fit 0.25, severity 0.20, red flags 0.10 |
| Conservative care | duration 0.50, adherence 0.25, failed modalities 0.25 |
| Documentation | required elements 0.60, result recency 0.25, consistency 0.15 |
| Guideline alignment | appropriateness 0.70, policy version match 0.30 |
| Utilization | duplicate service 0.50, ordering outlier 0.30, site of service 0.20 |

### Evidence confidence

Every fact carries a confidence built from three parts:

- **Source trust**

  | Source | Trust |
  |---|---|
  | Adjudicated claim | 0.95 |
  | Lab or structured EHR result | 0.90 |
  | Signed clinician note | 0.80 |
  | Referral or intake form | 0.65 |
  | Member self-report | 0.50 |
  | AI-extracted fact | source trust x extraction confidence x 0.9 |

- **Recency decay:** `trust x 0.5^(age / half_life)`. Half-lives: labs 30 days, imaging 90, chronic diagnoses 365, default 365.
- **Corroboration:** several reports of one fact are combined by a confidence-weighted average. Agreement raises confidence, and disagreement lowers it.

**Shrinkage:** `factor score = C x raw + (1 - C) x 40`. Thin evidence pulls the score toward 40, which sits below the approval line. Composite `S` is the weighted sum of factor scores, and `C` is the weighted sum of factor confidences.

### Uncertainty band and tiers

- The uncertainty band widens as confidence falls: `sigma = 0.08 + (1 - C)` on the log-odds scale, with z = 1.28 for an 80% band. The lower bound is `S_low`.
- **Rating** `R = 0.65 x S_low + 0.20 x (50 + 50 x momentum) + 0.15 x 100 x C`. Momentum is a -1 to 1 blend of document arrival rate and gap closure.

Because the tier is based on the **lower bound**, thin data cannot reach the top tier.

## 6. Business rules

### Hard gates

Gates are checked before the score and cap the tier regardless of `S`.

| Gate | Condition | Effect |
|---|---|---|
| G1 Exclusion | A policy exclusion is documented (e.g. prior identical study, contraindication) | Tier D, clinician required |
| G2 Mandatory element | A policy-required element is absent | Tier D, documentation request |
| G3 Urgency | Request is flagged expedited | 72-hour clock, never routed below tier B |
| G4 Ungrounded evidence | A decisive fact has no verbatim supporting quote | The fact is discarded |
| G5 Conflict | Note and claim contradict on a decisive fact | Capped at tier C |

### Routing tiers

| Tier | Meaning | Rule |
|---|---|---|
| **A** | Auto-approve candidate | R >= 75, C >= 0.70, no gate tripped |
| **B** | Fast-track nurse review | 60 <= R < 75 |
| **C** | Standard clinician review | 45 <= R < 60, or gate G5 |
| **D** | Documentation request or physician required | R < 45, or gate G1 or G2 |

The tier is a routing decision. It is not an outcome.

### Initial coverage policies (MVP)

| Policy | Service |
|---|---|
| Lumbar spine MRI | CPT 72148 |
| Knee arthroscopy with meniscectomy | CPT 29881 |
| GLP-1 for chronic weight management | Drug class code |

## 7. MVP

**Goal:** prove the system can fairly sort requests and explain why.

**In scope**
- Synthetic (fake) requests only. No real patient data.
- The three policies above.
- Reads clinical notes, scores each request, assigns tier A-D with reasons.
- A physician-facing chatbot that answers "why this tier?" and "what is missing?".
- Role-based login (patient and insights roles present but minimal).
- Audit log and a page showing how scoring works.

**Out of scope**
- Real patient data or insurer integrations.
- Document upload or actions from chat.
- Full patient and analytics experiences.
- Cost forecasting and member risk modelling.

**Success measures**
- **False-approval rate:** it never routes something to tier A that reviewers would not approve. This is the key safety number.
- **Agreement:** tiers broadly match reviewer decisions on test cases.
- **Explainability:** a physician can understand why any request landed where it did.

## 8. Phasing

1. Scoring engine and evidence platform on synthetic data.
2. Read-only physician chatbot that explains scores and cites policy.
3. Patient status chatbot with strict scoping and escalation.
4. Insights assistant over suppressed aggregates.
5. Actions, such as document upload and review requests.

---

## 9. Workstream: Scoring and Evidence Platform

**Owner:** backend developer (FastAPI, Postgres, Python). No AI background needed.

This is the deterministic core that everything else depends on. It is plain arithmetic, tables and APIs, with exact values that can be asserted in tests.

**Onboarding:** the first week is an aggressive five-day sprint, built to learn the domain by contributing. See the [Onboarding Sprint Board](https://claude.ai/artifact/D6ckrUruidjFdf3y871NgU) (ten tickets, a daily pace table and a domain primer).

### Scope

1. **Evidence pipeline.** Facts from different sources are normalized, deduplicated and combined (trust, recency decay, corroboration, conflict detection).
2. **Scoring engine.** Gates, factors, shrinkage, uncertainty band, rating and tier.
3. **Versioned configuration.** Policies, weights and thresholds are versioned records. Every assessment pins the version it used, and old versions are never altered.
4. **Snapshots and replay.** Append-only assessment history, point-in-time reconstruction, and diffs between assessments.
5. **Backtesting and calibration harness.** Run any model version over historical cases and compare results.

### Data model (initial)

| Table | Purpose |
|---|---|
| `requests` | The incoming prior-auth request |
| `facts` | One row per fact: value, source type, observed date, confidence, verbatim quote |
| `config_versions` | Immutable policy, weight and threshold sets |
| `assessment_snapshots` | Append-only: S, C, tier, gates tripped, JSON factor breakdown, config version, `as_of` timestamp |

### Planned endpoints

| Endpoint | Purpose |
|---|---|
| `POST /api/requests/{id}/assess` | Run the engine and store a snapshot |
| `GET /api/requests/{id}/assessments` | Assessment history |
| `GET /api/requests/{id}/explain` | Factor breakdown and drivers |
| `GET /api/model/methodology` | Weights and thresholds served verbatim |

### Sprint plan (two weeks each)

| Sprint | Theme | Work |
|---|---|---|
| **1** | Foundations | Schema and migrations (M). Pure scoring module with golden tests (M). Weights and thresholds in one config block with a methodology endpoint (S). |
| **2** | Evidence pipeline | Source-trust table and recency decay (S). Combining multiple reports of one fact (M). Conflict detection feeding gate G5 (M). Idempotent fact-ingestion API (M). |
| **3** | Versioning and replay | Immutable versioned config (L). Point-in-time `as_of` assessment (M). Append-only snapshots enforced by DB constraints (S). Snapshot diff endpoint (M). |
| **4** | Scale and operations | Async batch assessment with idempotent retries (L). Re-assess everything when a new model version ships (M). Structured logging, metrics, audit hash chain (M). |
| **5** | Backtesting | Replay harness for a model version over historical cases (L). Metric reports: tier distribution, false-A rate, reviewer agreement (M). Version-vs-version comparison (M). |
| **6** | Calibration and hardening | Calibrate score against reviewer outcomes and choose thresholds by error cost (L). Property-based tests (M). Fairness report by group (M). Load testing (S). |

Sprints 1-3 deliver a working, auditable engine. Sprints 4-6 cover scale, replay at volume and calibration.

### Acceptance criteria

- Golden tests with exact expected scores for around 10 hand-built cases, including the worked example (S approx. 73.5, C approx. 0.76).
- **Monotonic:** adding documented evidence never lowers `S`.
- **Safe on missing data:** removing a fact can only keep a tier the same or lower it.
- **Replayable:** the same `as_of` and config version always give identical output.
- **Gates bind:** a tripped gate caps the tier no matter how high `S` is.
- **Immutable:** snapshots cannot be updated or deleted, enforced at the database level.

### Handoff boundaries

| Owner | Responsibility |
|---|---|
| Backend developer | Everything in this section, including the `facts` table contract and the assessment API |
| AI team | Extraction that writes `facts`, and the chatbot that reads assessments |
| Shared (agree in sprint 1) | The `facts` schema, so both sides can build against fixtures |

The scoring engine does not care whether a fact came from a lab feed, a form or an AI model. It can be built and tested entirely with hand-written facts.

### Risks

- Weights and thresholds are placeholders. Calibration in sprint 6 may force schema changes.
- Backtesting needs realistic synthetic reviewer decisions. Someone must define how they are generated, ideally by sprint 3.
- `as_of` must be a parameter everywhere, never the current time, or replay breaks.

## 10. Known weaknesses

- Tier cut-offs (75 / 60 / 45), all weights, the 0.9 AI-extraction discount and the shrinkage prior of 40 are uncalibrated guesses.
- The utilization factor risks penalizing providers unfairly and may need heavy review or removal.
- Counting unclear criteria as zero is conservative and may over-refer cases.
- A weighted sum can let strong sub-scores hide a failing one inside a factor.
- Real deployment needs legal and privacy review (HIPAA and related rules).

## 11. Cases

Worked scenarios that estimate time and money saved, built from public data and labeled assumptions, are in [`cases/`](cases/README.md). Start with the evidence registry and Case 001.

## 12. Origin

The scoring approach (weighted factors, source trust, recency decay, shrinkage toward a prior, tier rating, point-in-time replay) is adapted from the Fighter Market Intelligence valuation model.
