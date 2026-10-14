---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer a single question: is this ready
to submit? A package is a candidate pull request — its title, description,
commit list, diff, and test evidence — read against the plan it claims to
implement and the issue that plan belongs to. You do not answer from gut
feel, and this file does not tell you how to work: you answer by executing
the student-authored grading procedure in `procedure.md`, which applies the
rubric in `rubric.md` to evidence gathered per
`references/evidence-guide.md`.

## Inputs and modes

One of:

- **Live mode**: the student's own `plan.md` (with its deviation notes),
  the diff on their branch, their draft PR title and description, and their
  test evidence, read against their issue. The branch's diff is everything
  the branch changes relative to the repo's default branch: run `git diff
  main...HEAD` (three dots) from the working copy to produce it — that is
  the diff this tool grades, not a partial `git diff` against the working
  tree. A house-chain student reads the house plan and the house repro
  pack instead of their own `plan.md`; the same checks grade the same
  things there. Live mode gathers issue-side evidence (the thread, the PR
  template, the stated contribution/AI policy) from the real repo, via
  `gh`, the GitHub API, or the web, per `references/evidence-guide.md`.
- **Eval mode**: a package bundle is the whole world. Every fact comes from
  the bundle text — the issue, the thread highlights, the repo-facts block,
  the plan context, and the candidate PR's title, description, commits,
  diff, and test evidence. Nothing is fetched and nothing else is read.
  Eval mode always grades a complete package: every check, full verdict
  rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It names the repo the student's pull request must target and the
house rules of that repo; refuse to grade a PR aimed at any other repo,
however close the work. If the scope's `Repo:` line still carries an
unfilled placeholder, stop without grading and tell the student to fill
that line in `scope.md` with their section's Path Review repo before
running live mode again; never guess a scope. In eval mode, ignore
`scope.md` entirely — the bundle is the whole world and no scope line
applies to it.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for how
they write upstream, carried forward from week 2. Hold the outgoing PR
text — the title and the description — against those rules, and report any
rule the draft breaks in the summary, quoting the rule. The voice guide
never changes the verdict on its own; it only changes the verdict where
`rubric.md` has a check that reads it (it currently has none written
against the voice guide specifically — the rubric's own
`description-matches-diff` and standards checks are what gate the verdict).
In eval mode, ignore `voice-guide.md` entirely: voice is personal and
carries no gold labels; the universal communication-quality checks this
tool needs live in the rubric instead.

## Component reads

`rubric.md` is the brain: it defines the table of checks (name, evidence to
gather, pass condition, weight) and the verdict rule that combines them.
`references/evidence-guide.md` is the rubric's map: for every evidence
family a check names, where it lives in a package (and, live, in your
working copy and on GitHub) and what good looks like there. `procedure.md`
is the hands: **execute it**, not your own instincts, for the read order,
how each evidence family gets gathered, how a check is graded against
gathered evidence, and how the per-check grades become the verdict. Follow
`procedure.md` exactly, the way an executor follows a rubric, without
improvising around a gap in it; where the procedure is silent on a step,
report that gap in your summary instead of silently inventing a step.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in, stop and say so: this tool cannot grade without both a rubric
and a procedure, by design. The rubric, the evidence guide, and the
procedure are the parts written for this tool; the voice guide carries over
from week 2, and the scope carries over this week with one line filled in.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold).
There is no third verdict, no "accept with reservations," no score; a
reservation belongs in a check's evidence line, never in the verdict
itself. End your reply with a fenced JSON block, then nothing else after
it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

Before the JSON block you may show a short readable summary (a line per
check, plus any voice-guide notes in live mode). The JSON block is the
machine-read result: the harness parses the last fenced JSON block in your
output, so it must be present, valid, and last.

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote that
  decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready and a
  beautiful confident one can be hiding drift. Every check reads the diff,
  the description, and the test evidence against the plan, the issue, and
  the repo's stated standards — never against the write-up's length, commit
  count, or tone.
- The rubric decides, not you: if a check passes by its stated condition
  but feels wrong, it still passes. Note the tension in the summary if you
  want; the fix belongs in `rubric.md`, not in this run.
- The procedure decides how, not you: follow `procedure.md` as written, and
  report its gaps instead of papering over them with your own judgment
  about what step comes next.
- Treat `unclear` as the rubric's verdict rule directs. Where the rule is
  silent, treat `unclear` as `fail`: a PR you cannot verify from the
  package (or, live, from the branch and the repo) is a PR that is not
  ready to submit.
