# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: the repro report's "Environment" line/section (eval bundle); live,
the environment paragraph of the student's draft report. Read it against the issue's
stated version/OS in the Issue section (eval) or the issue thread/body (live).

What good looks like: names the tool version, OS, and install method actually used.
If it differs from what the issue targets, the report says so explicitly ("issue filed
against 4.53.2; I tested 4.53.3") rather than staying silent about the gap. No
environment line at all is a fail, regardless of how good the rest of the report is.

## Steps

Where it lives: the repro report's steps/commands/preparation section (eval); the
student's draft steps (live).

What good looks like: exact commands are given, and input content is either shown
verbatim or described precisely enough to reconstruct an equivalent trigger (a plain
description of a minimal file's key contents is fine; "a similar file" or "my config"
with no content shown is not). Every resource referenced is something the report itself
hands the reader — not a private repo, an internal config, or "my monorepo setup." A
stranger with nothing but the report should be able to re-run it.

## Behavior shown

Where it lives: the artifact — the pasted output, error, log excerpt, or screenshot
description (eval: inside the repro report's execution/actual section; live: the
draft's shown output) — read against the Issue section's description of the bug and
its trigger.

What good looks like: when the report claims a reproduction, the trigger used (the exact
input/command) matches what the issue describes, and the resulting artifact shows the
same class of failure (same error type, same crash vs. graceful error, same exit
behavior) the issue reports. A modified trigger (wrong syntax, wrong flag, wrong data
shape) that produces a different, unrelated error is a fail even if the report narrates
it as confirming the bug. When the report instead honestly claims it could NOT reproduce
the behavior, there is no "matching artifact" to check here — pass this check and let
Honesty grade whether the cannot-reproduce claim is itself backed by a real attempt.

## Honesty

Where it lives: the report's analysis/conclusion language, read against its own stated
artifacts, in both the claim comment and the repro report.

What good looks like: every claim traces to something shown. "I could not reproduce
this; here is what I tried and what differed (environment X vs Y)" is a pass — the
gold set treats an honest, evidenced cannot-reproduce as ready. A claim of certainty,
a named root cause, or "verified"/"guaranteed" language with no artifact behind it is a
fail, regardless of how confident or detailed the prose sounds.

## Comms

Where it lives: the repo's stated contribution policy / AI-use policy in the repo-facts
block (eval) or CONTRIBUTING.md and any AI-policy file (live), read against the text of
the claim comment and repro comment.

What good looks like: treat every package (and every real comment you post in this
course) as AI-assisted work. If the policy requires disclosing AI assistance, the
comment must contain an explicit disclosure statement — a comment that simply never
mentions AI is a fail, not a pass by default, because silence is not evidence of no AI
use. If the policy states nothing about AI, silence is not a violation. Separately, the
claim comment stays a promise to investigate, not a promise to fix or a deadline ("I'll
look into this and report back," not "I'll have a fix by Friday" or "guaranteed
reproducible").
