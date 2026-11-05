<!-- Review note: physician office time lost to prior auth. Value accrues to providers, not the insurer's cost line. Draft. -->
# Case 002: Physician office time lost to prior authorization

**Status:** Draft (all external figures are grade B, see [registry](registry.md))
**Last updated:** 2026-11-04
**Scope:** One physician and one practice. **This value accrues to providers, not to the insurer's own cost line.** It matters for adoption and provider relations, not for the insurer's savings.

## 1. The situation (plain language)

A typical physician's office completes dozens of prior-authorization requests every week. Surveys put it around 40 requests and 13 hours of physician and staff time per physician per week. A good share of that time goes to back-and-forth: a request is pended for a missing document, the office finds out days later, and someone calls or resubmits.

If the platform tells the office at submission exactly what is missing, some of those loops disappear.

## 2. What the real numbers say

| ID | Fact | Value | Grade |
|---|---|---|---|
| S-05 | Requests completed per physician per week | about 40 | B |
| S-05 | Hours per physician per week (physician plus staff) | about 13 | B |
| S-06 | Minutes per manual request (CAQH) | about 20 | B |

Cross-check: 13 hours x 60 minutes / 40 requests = **19.5 minutes per request**, which agrees with the CAQH figure. Two independent sources pointing the same way raises our confidence in the baseline, though both are still secondary.

## 3. What the platform would change

At submission, the physician-facing view lists unmet and unclear criteria and the document that would resolve each. Fewer requests bounce back as pends.

| ID | Assumption | Conservative | Base | Ambitious |
|---|---|---|---|---|
| A-05 | Share of requests with a missing-information loop (f) | 10% | 20% | 30% |
| A-06 | Office minutes saved per avoided loop | 5 | 10 | 15 |
| A-07 | Loaded staff cost per minute | $0.50 | $0.50 | $0.50 |

## 4. The arithmetic

```
loops avoided per week = 40 requests x f
minutes saved per week = loops avoided x minutes per loop
minutes saved per year = per week x 52
dollars per year       = minutes per year x $0.50
share of burden        = minutes per week / (13 hours x 60 = 780 minutes)
```

**Conservative** (f 10%, 5 min): 4 loops x 5 = 20 min per week = 1,040 min per year = **17.3 hours**, **$520**
**Base** (f 20%, 10 min): 8 loops x 10 = 80 min per week = 4,160 min per year = **69.3 hours**, **$2,080**
**Ambitious** (f 30%, 15 min): 12 loops x 15 = 180 min per week = 9,360 min per year = **156 hours**, **$4,680**

## 5. Results (per physician per year)

| | Conservative | Base | Ambitious |
|---|---|---|---|
| Loops avoided per week | 4 | 8 | 12 |
| Office hours saved | 17.3 | 69.3 | 156 |
| Dollars saved | $520 | $2,080 | $4,680 |
| Share of the 13-hour weekly burden | 2.6% | 10.3% | 23.1% |

Example: a 50-physician group at the base case saves about **$104,000 per year** in office time.

## 6. Why these results

- The dollar figures are small per physician. The strength of this case is **adoption**, not savings: providers who find the process faster and clearer are more likely to submit complete, well-documented requests, which feeds the insurer's own fast lane in Case 001.
- The biggest unknown is A-05. We do not know how many pends are caused by missing documents versus clinical disagreement.
- The case ignores physician time, which is more expensive than staff time and which surveys include in the 13 hours.

## 7. What is left out

- Physician (not staff) time, valued at a higher rate.
- Patient benefit from faster care. Surveys report that most physicians say delays affect access to care, but we do not turn that into dollars here.
- Cost of integrating with practice software.
- Whether practices would actually use the physician-facing view.

## 8. What would prove this wrong

- If most pends are clinical disagreements, not missing documents, A-05 is much lower.
- If offices already know what a plan wants (through experience or portals), the "what is missing" feedback adds little.
- If the 13-hour figure includes mostly physician time, the staff-time saving is smaller than shown.

## 9. Next data to pull

| Data | Why | Where |
|---|---|---|
| Pend reasons by category (missing document, clinical, administrative) | Replaces A-05 | Plan case-management data |
| Resubmission rate and time between pend and resubmission | Sizes the delay cost | Plan data |
| Primary AMA report, opened and read | Upgrades S-05 to grade A, and confirms whether staff time is included | AMA |
| Staff wage for medical administrative workers | Replaces A-07 | BLS OEWS |
