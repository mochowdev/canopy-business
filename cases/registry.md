<!-- Review note: every sourced fact (S-xx) and assumption (A-xx) used by the cases. External figures are grade B, unverified against primary sources. -->
# Evidence and Assumption Registry

Every number used in a case is listed here first. Last reviewed: 2026-11-04.

Grades: **A** = primary source opened and read. **B** = credible secondary report, primary not opened. **C** = our assumption.

## Sourced facts

| ID | Fact | Value | Year | Grade | Source | Notes |
|---|---|---|---|---|---|---|
| S-01 | Medicare Advantage prior authorization determinations | "Nearly 53 million" (use 53.0M) | 2024 | B | KFF analysis, published 2026-01-28. Secondary: [AHA summary](https://www.aha.org/news/headline/2026-01-28-analysis-ma-insurers-made-53-million-prior-authorization-determinations-2024), [Fierce Healthcare](https://www.fiercehealthcare.com/payers/kff-nearly-53m-prior-auth-requests-submitted-medicare-advantage-2024), [KFF embed](https://www.kff.org/medicare/medicare-advantage-insurers-made-nearly-53-million-prior-authorization-determinations-in-2024/embed/) | Industry-wide, all MA insurers, not one company. |
| S-02 | Share fully or partially denied | 7.7% (about 4.1 million) | 2024 | B | Same as S-01 | Implied total 4.1M / 7.7% = 53.2M, consistent with S-01. So fully approved is at most 92.3%. |
| S-03 | Denials that were appealed | 11.5% | 2024 | B | Same as S-01 | 11.7% in 2023. |
| S-04 | Appeals fully or partially overturned | 80.7% | 2024 | B | Same as S-01 | 81.7% across 2019 to 2023. |
| S-05 | Physician prior-auth workload | About 40 requests per week and about 13 hours per week (physician plus staff) | Survey reported May 2026 | B | AMA survey as reported by the [California Medical Association](https://cmadocs.org/newsroom/news/view/ArticleId/51200/AMA-survey-highlights-ongoing-prior-auth-burdens-as-CMA-continues-reform-efforts); earlier AMA surveys: 43 per week and 12 hours (2023), 45 and 14 hours (2022) | Hour counts differ between survey years. Confirm who is included (physician only or staff too). |
| S-06 | Time and cost per prior-auth transaction | Manual 20 min vs electronic 8 min; manual cost $13.40 (2020 data), $10.92 (2019 data) | 2019-2020 | B | CAQH Index via [CAQH CORE slides](https://www.caqh.org/sites/default/files/core/CAQH%20CORE%20CCF-PAN%20PA%20Pilot%20Case%20Study%20Webinar%20Part%202%20Slides%20.pdf) and [TechTarget](https://www.techtarget.com/revcyclemanagement/news/366602037/Prior-Authorization-Costs-Rise-as-Electronic-Adoption-Remains-Low) | Dated. Find the latest CAQH Index edition. |
| S-07 | Registered nurse median hourly wage | About $45.00 ($93,600 per year) | May 2024 | B, conflicting | BLS OEWS, SOC 29-1141 ([primary page](https://www.bls.gov/oes/current/oes291141.htm), not opened). Seen on third-party pages; some show $42.79 to $45.00 | Verify on bls.gov before use. |

Cross-check: S-05 implies 13 hours x 60 / 40 requests = **19.5 minutes per request**, close to the 20-minute manual figure in S-06.

## Our assumptions (grade C)

None of these has a source. Each has a reason, and each is a candidate to replace with real operational data.

### Insurer side (Case 001)

| ID | Assumption | Conservative | Base | Ambitious | Reasoning | What would replace it |
|---|---|---|---|---|---|---|
| A-01 | Share of determinations that need a human clinical review today (h) | 40% | 50% | 60% | Many plans already auto-approve some services, so not every request is read by a nurse. | Plan operations data on auto-adjudication rates |
| A-02 | Share of human-reviewed requests our top tiers could fast-track (r) | 10% | 20% | 30% | Capped by the approval rate (92.3% or less). Most requests are approved, but only well-documented ones qualify. | Shadow-mode results (agreement and false-approval rate) |
| A-03 | Reviewer minutes saved per fast-tracked request (m) | 5 | 10 | 15 | Pre-organized criteria and evidence quotes replace reading raw notes. | Time-and-motion study with reviewers |
| A-04 | Loaded cost multiplier on wage (benefits, overhead) | 1.3 | 1.3 | 1.3 | Common planning range is 1.25 to 1.4. | Finance |

Loaded cost per minute = $45.00 per hour (S-07) x 1.3 / 60 = **$0.975**.

### Provider side (Case 002)

| ID | Assumption | Conservative | Base | Ambitious | Reasoning | What would replace it |
|---|---|---|---|---|---|---|
| A-05 | Share of requests that hit a missing-information loop (f) | 10% | 20% | 30% | Incomplete documentation is a common reason for pends and resubmission. | Pend and resubmission rates from plan data |
| A-06 | Office minutes saved per avoided loop | 5 | 10 | 15 | One phone call or resubmission costs several minutes of staff time. | Practice time study |
| A-07 | Loaded cost of office staff time per minute | $0.50 | $0.50 | $0.50 | About $30 per hour loaded. Placeholder. | BLS wage data for medical administrative staff |

## Changelog

- 2026-11-04: Initial registry. All external figures are grade B because primary sources could not be reached from the research environment.
