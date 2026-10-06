# Procedure: how this skill grades a plan package

## Read order

1. Read the issue context first, to know what the reported problem or request actually is.
2. Read the repro-evidence block (eval) or the student's posted repro comment (live) next. Note the exact behavior it pins down and any control-run results — this is the ground truth the plan's diagnosis must match. This must be read before the plan, since the plan is graded against it, not the other way around.
3. Read the candidate plan in full. Note: its stated cause, its in-scope/not-in-scope statement, named files/areas, approach/order of work, test plan, and risks/unknowns.
4. Read the candidate plan comment. Note what it says to the thread.
5. Read the thread highlights (eval) or the live issue thread (live), and the repo-facts block's stated contribution/AI-use policy. Note any explicit maintainer direction already present, and any AI-disclosure requirement.

## Evidence gathering

For each check, pull exactly this before grading it:

- **diagnosis-grounded**: the plan's stated cause (one sentence), plus the specific repro-evidence lines or control-run results that confirm or contradict it.
- **scope-bounded**: the plan's in-scope and not-in-scope statements, plus the list of files/areas it names.
- **executable**: the plan's named files/areas plus its approach/order-of-work section.
- **test-plan-decisive**: the plan's test-plan section plus the repro evidence's steps/artifacts it re-runs or checks against.
- **comms-thread-aware**: the plan comment's text, plus any explicit maintainer direction quoted in the thread highlights, plus the repo-facts block's AI-policy line.

## Check execution

1. Grade checks in the table's order: diagnosis-grounded, scope-bounded, executable, test-plan-decisive, comms-thread-aware.
2. Grade each check pass, fail, or unclear using only the evidence gathered for it above; do not re-read the whole package per check.
3. If the evidence a check names is genuinely absent from the package (not merely terse), grade that check unclear.
4. diagnosis-grounded is always graded against the repro evidence directly. If the thread has settled on a cause but the repro evidence contradicts or does not support it, the check fails regardless of how confidently the plan or the thread states it.
5. comms-thread-aware fails on disclosure alone if the repo's policy requires it and the comment contains no disclosure statement, even if every other check passes and the plan is otherwise excellent.

## Verdict assembly

1. Apply the rubric's verdict rule: accept if and only if every required check passes; a grade of unclear counts as fail.
2. The check that produced the first required fail, in the table's order, is the one quoted as the deciding check in the output. If more than one required check fails, quote all of them.
3. Preferred checks, if the rubric has any, never gate the verdict — they are reported but do not change accept/reject.
4. Produce the same verdict from the same set of check grades every time; verdict assembly never re-weighs evidence, only combines the grades already reached.
