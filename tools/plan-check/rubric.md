# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | the plan's stated cause, read against the repro-evidence block's shown behavior and any control runs | the stated cause is consistent with what the repro evidence actually shows; graded against the repro evidence directly, never against what the thread merely believes — a plan that adopts a popular but evidence-contradicted theory from the thread still fails this | required |
| scope-bounded | the plan's in-scope/not-in-scope statement and named files/areas | names one bounded change (specific file(s) or area(s)) that addresses the issue's reported symptom; fails if the plan bundles unrelated refactors, migrations, new features, or a redesign alongside the fix | required |
| executable | the plan's named files/areas and its approach/order-of-work | a stranger could start without asking the author anything: concrete files and a concrete approach are named; fails if real decisions are deferred to build time ("somewhere," "whichever is easier," "not sure which layer") | required |
| test-plan-decisive | the plan's test-plan section, read against the repro evidence's steps/artifacts | names an observable outcome tied to the repro steps (e.g. re-running the repro and naming the specific different result expected); fails on a vague outcome ("should feel fast") or a non-specific test ("run the full test suite" with no outcome named for this fix) | required |
| comms-thread-aware | the plan comment, read against the thread highlights' explicit maintainer direction and the repo-facts block's stated AI-use policy | the comment engages with (does not ignore) any explicit maintainer direction already in the thread; if the repo's policy requires disclosing AI assistance, the comment contains an explicit disclosure statement — a comment that never mentions AI fails this when disclosure is required, since every package here is AI-assisted work by default | required |

## Verdict rule

Accept (ready) if and only if every required check passes. Unclear counts as fail.
Preferred checks never change the verdict — they only rank or inform, never gate.
