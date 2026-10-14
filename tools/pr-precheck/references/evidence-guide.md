# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives: in an eval bundle, the plan context block's prose — its
"Scope, one bounded change" sentence, its "Not in scope" sentence, its
"Files:" list, and any deviation or known-limit note — read against the
candidate PR's unified diff (every `+++`/`---` file header and the hunks
under it) and the candidate PR's description. Live, the same three things
sit in your own `plan.md` (with its deviation notes section), the output of
`git diff main...HEAD` run from your working copy, and your draft PR
description.

What good looks like: every file the diff touches is one the plan named, or
sits under a deviation note that says what changed and why; every behavior
the plan promised is present in the diff, or its absence is covered by the
same kind of note. The description's claims about what it changed match the
diff exactly — "implements the plan exactly" is only true if nothing in the
diff falls outside the plan's named scope. Silent drift shows up in both
directions: a diff that does *more* than the plan (an unannounced config
flag, a rewrite of a file never mentioned, a "while I'm in there" cleanup)
and a diff that does *less* than the plan promised with no note explaining
the gap (a docs update the plan called for that the diff never adds, while
the description still claims the docs now cover it). An honestly recorded
deviation is not drift; a gap that exists only in the diff, with no note
anywhere in the plan, is.

## Test evidence (harness category: not-tested)

Where it lives: in an eval bundle, the candidate PR's "Test evidence"
section, read against the plan context block's "Test plan:" sentence
(every repro step and failure mode it names) and against the diff's own
changed code path. Live, the same thing is your captured before/after
output (terminal transcript, screenshot, test-run log) held next to your
`plan.md`'s test plan and your actual branch diff.

What good looks like: a named observable state before the fix, the same
observable after it (an error that stops appearing, an exit code that
changes, an output that now matches), tied to the specific repro the plan
named — not a general "tests pass" or "works on my machine" with nothing
named. When the plan's "Test plan:" sentence itself explicitly names more
than one distinct failure mode or repro (two bugs, two code paths, a fix
case and a control case), each one it names needs to be addressed
*somewhere* in the package: a before/after transcript, or a specifically
named regression test in the diff with its passing outcome stated (even
tersely — "the control is unchanged", "passes" next to a suite run that
includes it). What fails this is silence, not terseness: a named failure
mode with no transcript, no named test, and no stated outcome anywhere in
the diff, description, or test evidence, or evidence that substitutes a
different, already-working code path in its place (re-running a control
that already passed instead of the fix's own failure mode, or a new test
that exercises an unrelated behavior). Count only what the "Test plan:"
sentence itself enumerates as a repro to re-run — a fix applied across
several functions or files under one "same shape" scope statement is not
several failure modes unless the test plan names more than one repro to
check. The repo's own checks or test suite being run and their outcome
stated (not merely implied) is part of this too.

## Diff quality (harness category: unreviewable)

Where it lives: the candidate PR's unified diff, hunk by hunk, and its
commit list. Live, the same thing is `git diff main...HEAD` and `git log
main..HEAD --oneline` on your branch.

What good looks like: every hunk is there because the fix (or its
necessary tests, docs, or changelog entry) needs it, and nothing else.
Debris tells are specific and do not get a pass for being small or
"just churn": a debug `print`/`eprintln`, a commented-out line or an entire
commented-out first attempt left beside the real one, a dead function kept
behind a `#[allow(dead_code)]`/`# noqa`/similar marker, an import or
re-indentation hunk that changes no behavior, or a hunk in a file the fix
never needed to touch. A commit list of scratch labels ("wip", "fix", "fmt
+ cleanup") is itself a tell that the branch was never cleaned up before
posting, even when the fix inside it is correct and evidenced.

## Standards and comms (harness category: standards-wall)

Where it lives: the repo-facts block's stated PR-template sections or
checklist and its stated contribution/AI-use policy, read against the
candidate PR's description (and, where the template asks for a file, the
diff itself — e.g. a required changelog/whatsnew entry). Live, the same
facts live in the repo's actual `.github/PULL_REQUEST_TEMPLATE.md` and
`CONTRIBUTING.md`/`AI_POLICY.md`, read against your draft description and
branch diff. (Whether the description's *fidelity* claims match the diff is
plan fidelity, above; this family is only about the repo's own stated
procedural asks.)

What good looks like: every required template section or checklist item
has real, specific content about this PR, not a blank line or an
unedited boilerplate placeholder, and a required artifact the template asks
for (a changelog entry, a whatsnew entry, a `closes #NNNN` line) is actually
present in the diff or description, not just promised. Separately: every
package this tool grades is AI-assisted work by default, so when the repo's
stated policy requires disclosing AI assistance, the description must
contain an explicit disclosure statement in the author's own words — a
description that simply never brings AI up is a fail here, not a pass by
default. If the repo states no template and no AI policy at all, there is
nothing to fail this family on.
