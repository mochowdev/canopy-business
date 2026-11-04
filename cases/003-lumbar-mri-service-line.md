# Case 003: Lumbar spine MRI (CPT 72148), one service line

**Status:** Blocked on data
**Last updated:** 2026-11-04
**Scope:** One service line, sized with plan-level volumes once we have them.

## 1. The situation (plain language)

Lumbar spine MRI is our first policy, and a good test case: it is common, the coverage criteria are well defined (about six weeks of conservative care, or a red flag, plus radiculopathy or a neurological deficit), and it is often ordered before the criteria are met. That makes it a place where both fast approval and early "what is missing" feedback could matter.

Cases 001 and 002 size the whole market. This case narrows to one service so every input can eventually come from real plan data.

## 2. What we need before this case can be written

No numbers are filled in yet, on purpose. Using invented volumes here would make the case look more real than it is.

| Input | Needed for | Candidate source | Status |
|---|---|---|---|
| Annual volume of CPT 72148 for a defined population | Number of requests | CMS Medicare Physician and Other Practitioners public use file, for Medicare volumes; the plan's own claims for commercial and MA | Not pulled (the research environment could not reach data.cms.gov) |
| Share of 72148 requests that require prior authorization | Requests in scope | Plan policy and operations | Not available |
| Approval, pend and denial rates for 72148 | Sets the ceiling on the fast lane | Plan data | Not available |
| Reviewer minutes per 72148 case | Replaces A-03 | Time study | Not available |
| Share of 72148 requests pended for missing documentation | Replaces A-05 | Plan case data | Not available |
| Reimbursement for 72148 | Sizes the cost of an inappropriate scan, which is a separate benefit | CMS physician fee schedule | Not pulled |

## 3. The calculation layout (ready to fill)

```
requests in scope        = annual volume x share requiring prior auth
fast-lane requests       = requests in scope x share qualifying for tiers A/B
reviewer minutes saved   = fast-lane requests x minutes saved per case
reviewer dollars saved   = minutes saved x loaded cost per minute        (as in Case 001)
pends avoided            = requests in scope x pend rate for missing docs x share resolved at submission
office hours saved       = pends avoided x minutes per loop / 60          (as in Case 002)
```

A second benefit to evaluate separately: **appropriateness**. If better routing and earlier feedback reduce scans ordered before conservative care is tried, the avoided scan cost is also real. It needs reimbursement data and a believable rate of inappropriate orders, and it deserves its own case.

## 4. What would make this case convincing

- All volume inputs come from one named plan or population, not from market-wide averages.
- Reviewer time comes from a measured sample, not an assumption.
- Results are shown next to the shadow-mode agreement and false-approval rates, because savings mean nothing if safety is not demonstrated.

## 5. Next steps

1. Allow the research environment to reach the data hosts (see the note below) or pull the CMS file manually.
2. Ask operations for the plan-level inputs in the table above.
3. Fill in the arithmetic and move this case to Draft.

**Network note:** the research environment could not reach `www.kff.org`, `www.bls.gov`, `www.ama-assn.org` or `data.cms.gov`. These hosts would need to be allowed for the primary sources to be read and the registry upgraded to grade A.
