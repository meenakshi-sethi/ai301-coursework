# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: the plan's stated cause (eval: usually its opening "Diagnosis"/"Cause"
line; live: the student's draft plan.md). The ground truth to check it against is the
repro-evidence block (eval) or the student's posted repro comment (live) — specifically
any control runs, step-by-step output, or named behavior.

What good looks like: the stated cause is a specific mechanism the repro evidence
actually demonstrates (a control run that isolates it, a step that shows the behavior
only happens under the claimed condition). It fails when a control run or an observed
step in the repro evidence contradicts or is silent on the claimed cause — including
when the plan is simply repeating a theory popular in the thread that the repro evidence
itself does not support.

## Scope

Where it lives: the plan's explicit in-scope / not-in-scope statement and its list of
files or areas to touch.

What good looks like: one bounded change — a specific file or a small named set of
files, addressing exactly the symptom the issue reports. It fails when the plan also
proposes unrelated refactors, a migration, a new feature, a cross-cutting redesign, or
"while I'm in there" work the issue never asked for, even when the core fix inside it is
correct.

## Executability

Where it lives: the plan's named files/areas and its approach or order-of-work section.

What good looks like: someone who has never seen the issue could start working
immediately — concrete file(s), a concrete approach, and (when order matters) a stated
order. It fails when real decisions are deferred to build time: "somewhere in the
codebase," "whichever approach is easier," "not sure if this is the TUI layer or the
input layer."

## Test plan

Where it lives: the plan's test-plan section, read against the repro evidence's steps
and artifacts (what was run, what was observed).

What good looks like: the test plan names a specific, observable outcome tied to
re-running (or adapting) the repro steps — e.g. "re-run the repro script; expect exit 0
instead of the current panic." It fails when the outcome is a feeling ("should feel
faster") or when the plan names a generic process with no outcome specific to this fix
("run the full test suite").

## Honesty

Where it lives: the plan's risks/unknowns section, and (after a build) its Deviations
section.

What good looks like: unknowns and deferred work are named explicitly and the plan says
why ("I'm deferring the Windows-specific variant because I have no way to test it") —
this is honest scoping-down, not a failure. It fails when the plan states a deferral or
an assumption as settled fact, or when a build that ended up different from the plan has
no corresponding note explaining what changed and why.

## Comms

Where it lives: the plan comment, read against the thread highlights (eval) or the live
issue thread (live) for any explicit maintainer direction already given, and against the
repo-facts block's stated contribution/AI-use policy.

What good looks like: the comment engages with maintainer direction already in the
thread rather than silently ignoring it (proposing a different approach than one a
maintainer already asked for, without saying why, fails this). Separately: every package
in this course is AI-assisted work by default — if the repo's policy requires disclosing
AI assistance, the comment must contain an explicit disclosure statement; a comment that
simply never mentions AI is a fail, not a pass by default. If the policy states nothing
about AI, silence is not a violation.
