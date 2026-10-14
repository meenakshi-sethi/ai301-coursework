# Procedure: how this tool grades a PR package

## Read order

1. Read the issue first: what symptom or request is actually reported, in
   the reporter's own words. This is the problem the rest of the package is
   answerable to.
2. Read the plan context block in full before the diff: its stated cause,
   its in-scope/not-in-scope statement, the files it names, its test plan
   (every distinct repro or failure mode it names), and any deviation or
   known-limit notes. This is the ground truth every side-by-side check
   below reads the diff against, so it has to be loaded first or the diff
   gets graded against nothing.
3. Read the candidate PR's full diff and commit list next, before the
   description. List every changed file and, for each file, the hunks in
   it, in your own notes. Reading the diff before the description means a
   claim in the description gets checked against what you already saw, not
   the other way around.
4. Read the candidate PR's title and description.
5. Read the test-evidence section.
6. Read the repo-facts block (eval) or the repo's PR template and
   CONTRIBUTING.md/AI_POLICY.md (live), noting every required template
   section or checklist item and the repo's stated AI-disclosure policy
   verbatim. Read the thread highlights for any explicit maintainer
   direction already given.

## Evidence gathering

Pull exactly this before grading each check; do not substitute a general
impression of the PR for the named pairing.

- **diff-matches-plan-scope**: the plan's named files plus its in-scope and
  not-in-scope sentences plus its deviation/known-limit notes, next to the
  full list of files and behaviors the diff actually touches (from step 3's
  notes).
- **description-matches-diff**: every fidelity sentence the description
  makes about what it changed or didn't ("exactly as planned", "no changes
  beyond", "now documents X"), next to the diff hunks that would have to
  exist for each sentence to be true.
- **test-evidence-decisive**: the test-evidence section's stated before and
  after (or its absence), next to the plan's test plan.
- **test-evidence-on-target**: the specific behavior each piece of test
  evidence exercises, next to (a) the code path the diff changed and (b)
  the full list of distinct failure modes or repro steps the plan's test
  plan names — count how many of those the evidence actually re-runs.
- **diff-reviewable**: every hunk in the diff plus the commit message list,
  scanned for debug prints, commented-out code, dead functions, formatting-
  only hunks, and commit messages that read as scratch labels.
- **template-compliant**: the repo-facts block's required template
  sections/checklist items, next to the description and the diff, item by
  item.
- **ai-disclosure-compliant**: the repo-facts block's stated AI/contribution
  policy sentence, next to the description's text for a disclosure
  statement.

## Check execution

1. Grade checks in the rubric's table order: diff-matches-plan-scope,
   description-matches-diff, test-evidence-decisive, test-evidence-on-target,
   diff-reviewable, template-compliant, ai-disclosure-compliant.
2. Grade each check pass, fail, or unclear using only the evidence gathered
   for it above; do not re-read the whole package per check.
3. If the fact a check needs is genuinely absent from the package (not
   merely terse) — no test-evidence section at all, no repo-facts policy
   line, no plan scope statement — grade that check unclear, not pass.
4. diff-matches-plan-scope and description-matches-diff are both graded
   against the diff directly, never against how confidently or at what
   length the description asserts fidelity; a terse, accurate description
   passes both, and a long, confident one that the diff contradicts fails
   both.
5. test-evidence-on-target fails only when a failure mode or repro that the
   plan's "Test plan:" sentence itself explicitly names is addressed
   nowhere in the package — no transcript, no specifically named
   regression test in the diff, no stated outcome at all — or when the
   evidence substitutes a different, already-working code path for it. A
   named item whose outcome is only asserted ("unchanged", "passes") next
   to a named regression test or a suite run that includes it still
   passes; this check is not a bar on how much transcript detail each named
   item gets, only on whether each one is addressed at all. Count only what
   the "Test plan:" sentence enumerates as something to re-run — a fix
   that touches several functions or files under "same one-shape fix" in
   the scope/files section is not, by itself, more than one failure mode
   unless the test plan names more than one repro to check.
6. ai-disclosure-compliant passes automatically when the repo-facts block
   states no AI policy; it is not graded against the voice guide, which is
   a live-mode-only, personal seam and carries no gold labels here.
7. A deviation note recorded in the plan (and, ideally, restated in the
   description) turns what would otherwise be a scope or test-coverage gap
   into an honestly scoped shortfall, not a fail: check the plan's own text
   for the note before failing diff-matches-plan-scope or
   test-evidence-on-target on an apparent gap.

## Verdict assembly

1. Apply the rubric's verdict rule: accept if and only if all seven
   required checks pass; a grade of unclear counts as fail.
2. The check that produced the first required fail, in the table's order
   above, is the one quoted as the deciding check in the output summary. If
   more than one required check fails, quote all of them, in table order.
3. Produce the same verdict from the same set of check grades every time;
   verdict assembly never re-weighs evidence, it only combines the grades
   already reached in Check execution.
