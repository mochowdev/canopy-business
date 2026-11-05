<!-- Review note: rules for writing a case (IDs, evidence grades, three scenarios, show the arithmetic) and the case index. -->
# Cases

## Contents

<!-- contents:start -->
- [Rules for every case](#rules-for-every-case)
- [Status of evidence](#status-of-evidence)
- [Index](#index)
- [Adding a case](#adding-a-case)
- [What these cases are not](#what-these-cases-are-not)
<!-- contents:end -->

Worked scenarios that estimate how much time and money the platform could save, using public data where it exists and clearly labeled assumptions where it does not.

Each case answers four questions in plain language:

1. **What is the situation?** Who is affected and what happens today.
2. **What do the real numbers say?** Public data, with sources.
3. **What would the platform change?** Our assumptions, in ranges, with the reasoning.
4. **So what is it worth, and why?** The arithmetic, shown step by step.

## Rules for every case

- **Every number has an ID.** Sourced facts are `S-xx` and our own assumptions are `A-xx`. Both live in [`registry.md`](registry.md). A case may not use a number that is not in the registry.
- **Evidence is graded.**

  | Grade | Meaning |
  |---|---|
  | **A** | Opened and read in the primary source |
  | **B** | Reported by a credible secondary source; primary not yet opened |
  | **C** | Our assumption; no source |

- **Three scenarios per case:** conservative, base and ambitious. We never present a single number.
- **Show the arithmetic.** Anyone should be able to redo it with a calculator.
- **Say what is left out.** Platform cost, error cost and implementation effort are explicit gaps until we fill them.
- **Say what would prove us wrong.** Each case lists the facts that would change its conclusion.

## Status of evidence

As of the first draft, **no external figure has been verified against its primary source.** The research environment could not reach the primary sites (KFF, BLS, AMA). All sourced numbers are grade B. Before any case is shown outside the team, each `S-xx` row must be upgraded to grade A or replaced.

## Index

| Case | Scenario | Evidence | Status |
|---|---|---|---|
| [001](001-medicare-advantage-reviewer-time.md) | Reviewer time across all Medicare Advantage prior auth (industry ceiling) | S-01, S-02, S-07, A-01 to A-04 | Draft |
| [002](002-physician-practice-burden.md) | Physician office time lost to prior auth and missing-document loops | S-05, S-06, A-05 to A-07 | Draft |
| [003](003-lumbar-mri-service-line.md) | One service line (lumbar spine MRI, CPT 72148) with plan-level data | Needs data pull | Blocked on data |

## Adding a case

Copy [`_TEMPLATE.md`](_TEMPLATE.md), give it the next number, add any new sources or assumptions to the registry first, then fill in the case.

## What these cases are not

They are not forecasts and not a business plan. They size an opportunity so we can decide where to look harder. Data reflects the most recent published reporting year (2024 for Medicare Advantage), not live operations.
