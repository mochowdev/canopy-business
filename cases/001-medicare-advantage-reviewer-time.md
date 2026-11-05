<!-- Review note: Medicare Advantage industry-ceiling savings. Draft; sensitive to A-01 and A-03, and not specific to one company. -->
# Case 001: Reviewer time across Medicare Advantage prior authorization

**Status:** Draft (all external figures are grade B, see [registry](registry.md))
**Last updated:** 2026-11-04
**Scope:** Industry-wide ceiling for all Medicare Advantage insurers. This is **not** one company's number.

## 1. The situation (plain language)

Medicare Advantage plans decided about 53 million prior-authorization requests in 2024, and the large majority were approved. Today a request that is clearly well documented can still wait in the same queue as a risky one, and a nurse still reads it. If the platform can safely sort the clearly supported requests into a fast lane, reviewers spend less time on cases that were always going to be approved and more on the hard ones.

This case sizes how much reviewer time that could free up across the whole market, so we know whether the opportunity is worth pursuing.

## 2. What the real numbers say

| ID | Fact | Value | Grade |
|---|---|---|---|
| S-01 | MA prior-auth determinations, 2024 | about 53.0 million | B |
| S-02 | Fully or partially denied | 7.7% (about 4.1 million) | B |
| S-07 | RN median wage | about $45.00 per hour | B, conflicting |

The denial rate sets a ceiling. At most 92.3% of requests are fully approved, so the fast lane can never legitimately cover more than that share.

## 3. What the platform would change

The tier A and B routing sends well-documented requests to a quick sign-off, with criteria, evidence quotes and confidence already assembled. The nurse confirms instead of reconstructing the case.

| ID | Assumption | Conservative | Base | Ambitious |
|---|---|---|---|---|
| A-01 | Share needing human clinical review today (h) | 40% | 50% | 60% |
| A-02 | Share of those fast-tracked (r) | 10% | 20% | 30% |
| A-03 | Minutes saved per fast-tracked request (m) | 5 | 10 | 15 |
| A-04 | Loaded cost multiplier | 1.3 | 1.3 | 1.3 |

Cost per minute = $45.00 / 60 x 1.3 = **$0.975** (S-07, A-04).

## 4. The arithmetic

```
fast-tracked requests = determinations x h x r
minutes saved         = fast-tracked requests x m
hours saved           = minutes saved / 60
full-time equivalents = hours saved / 2,080
dollars saved         = minutes saved x $0.975
```

**Conservative** (h 40%, r 10%, m 5):
- 53.0M x 0.40 = 21.2M requiring human review
- 21.2M x 0.10 = 2.12M fast-tracked
- 2.12M x 5 = 10.6M minutes = 176,667 hours = 85 FTE
- 10.6M x $0.975 = **$10.3M**

**Base** (h 50%, r 20%, m 10):
- 53.0M x 0.50 = 26.5M
- 26.5M x 0.20 = 5.3M fast-tracked
- 5.3M x 10 = 53.0M minutes = 883,333 hours = 425 FTE
- 53.0M x $0.975 = **$51.7M**

**Ambitious** (h 60%, r 30%, m 15):
- 53.0M x 0.60 = 31.8M
- 31.8M x 0.30 = 9.54M fast-tracked
- 9.54M x 15 = 143.1M minutes = 2,385,000 hours = 1,147 FTE
- 143.1M x $0.975 = **$139.5M**

## 5. Results

| | Conservative | Base | Ambitious |
|---|---|---|---|
| Fast-tracked requests per year | 2.12M | 5.3M | 9.54M |
| Share of all determinations | 4.0% | 10.0% | 18.0% |
| Reviewer hours saved | 176,667 | 883,333 | 2,385,000 |
| Full-time equivalents | 85 | 425 | 1,147 |
| Dollars saved per year | $10.3M | $51.7M | $139.5M |
| Value per fast-tracked request | $4.88 | $9.75 | $14.63 |

Check on the ceiling: even the ambitious case fast-tracks 18.0% of all determinations, well under the 92.3% approval share.

## 6. Why these results

- **Everything is linear.** Doubling any one input doubles the answer. The spread between conservative and ambitious (about 13x) comes from multiplying three uncertain inputs together.
- **The weakest numbers are A-01 and A-03.** We do not know how much review is already automated, and we have no measured time saved per case. Both can be measured inside a plan in weeks.
- **Value per request is the portable number.** At about $4.88 to $14.63 per fast-tracked request, the question for any deployment becomes: how many requests does it need to fast-track to pay for itself? At the base case, **each $1M of annual platform cost needs about 102,600 fast-tracked requests** to break even ($1,000,000 / $9.75).

## 7. What is left out

- Platform build, hosting, model, oversight and maintenance cost.
- The cost of errors. One wrongly fast-tracked case could cost more than hundreds of saved minutes, which is why false-approval rate is the headline safety metric.
- Implementation and change management, including reviewer training.
- Time saved at later stages (appeals, rework, patient calls).
- Any benefit from fewer missed deadlines.

## 8. What would prove this wrong

- If most routine requests are already auto-approved, A-01 is far lower than 40% and the savings shrink toward zero.
- If a pilot shows reviewers still re-read every note before signing off, A-03 falls near zero.
- If shadow mode shows the top tier agrees with reviewers well under, say, 95% of the time, r must be cut to protect safety.
- If the true RN loaded cost is meaningfully different from $0.975 per minute, every dollar figure scales with it.

## 9. Side observation: appeals (not included in the numbers above)

From S-02 to S-04: 4.1M denials x 11.5% appealed = about 471,500 appeals, and 80.7% of those were overturned, about 380,500 cases. A high overturn rate suggests many initial denials were questionable or documentation was incomplete at the first pass. Our platform does not issue denials, but earlier "what is missing" feedback might reduce this rework. This is a **hypothesis to test**, not a saving we count. We have no cost-per-appeal figure yet.

## 10. Next data to pull

| Data | Why | Where |
|---|---|---|
| UnitedHealthcare's own MA determinations, denial and appeal figures | Turns an industry ceiling into a company-sized number | KFF's insurer-level table in the primary report |
| Auto-adjudication rate for imaging requests | Replaces A-01 | Internal operations |
| Reviewer minutes per case by tier of complexity | Replaces A-03 | Time study |
| Verified RN wage and loaded cost | Replaces S-07 and A-04 | BLS OEWS page and Finance |
| Primary KFF report, opened and read | Upgrades S-01 to S-04 to grade A | KFF |
