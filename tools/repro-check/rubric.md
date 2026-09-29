# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | the repro report's environment record, read against the issue's stated version/OS | names the tool version, OS, and install method actually used; if it differs from the issue's, the difference is stated explicitly; if the report has no environment record at all, fail | required |
| steps-followable | the repro report's steps/commands, read for completeness | commands run are exact; input content is either shown verbatim or described precisely enough that a stranger could reconstruct an equivalent trigger (e.g. "a dependencies list plus an unrecognized category: key" counts); no missing setup step, no reference to private/unshared files or environments | required |
| behavior-matches-issue | the artifact (output/log/error shown) read against the issue's described behavior and trigger | if the report claims to reproduce the issue, the artifact must show the same behavior (same error type, same trigger condition/input syntax) — a different error from a modified or mistyped trigger fails even if narrated as confirming the bug; if the report honestly states it did NOT reproduce the behavior, this check passes (honest-claims grades whether that claim is backed) | required |
| honest-claims | the report's stated conclusions read against what its own artifacts actually show | every claim is backed by a shown artifact; a report that says "could not reproduce" and names what it tried and what differed still passes; a claim of certainty, root cause, or "verified"/"guaranteed" with no artifact to back it fails | required |
| conventions-respected | the repo's stated contribution/AI-use policy, read against the claim and repro comment text | every package in this course is AI-assisted work by default; if the repo's policy requires disclosing AI assistance, ANY comment with no explicit disclosure statement fails this check, even if the comment never mentions AI — silence is not proof of no AI use; if the policy states nothing about AI, silence passes; separately, the claim comment must not promise a fix, a timeline, or certainty it has not earned | required |

## Verdict rule

Accept (ready) if and only if every required check passes. Unclear counts as fail.
Preferred checks never change the verdict — they only rank or inform, never gate.
