# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73

**Verdict output**

```
Check-by-check summary

Today: 2026-09-22 | Repo last push: 2026-09-16 (6 days ago) | Maintainer: Aburke225 (COLLABORATOR, in all 5 recent commits) | CONTRIBUTING.md: not found (no AI ban)

---
Issue #73 — README and .env.example disagree about which LLM API key to set

| Check              | Grade | Evidence                                                                             |
|---------------------|-------|---------------------------------------------------------------------------------------|
| repo-active        | pass  | Last push 2026-09-16, 6 days ago — within 90 days                                     |
| scope-settled      | pass  | One bounded docs fix (sync two config files); 0 comments, no debate, no prior PRs     |
| maintainer-alive   | pass  | Aburke225 (COLLABORATOR) committed 2026-09-16, within 90 days                         |
| unclaimed          | pass  | No assignee, no linked open PRs, 0 comments                                           |
| ai-policy-allowed  | pass  | No CONTRIBUTING.md; silence = allowed                                                 |

Verdict: ACCEPT

---
Ranking (fit profile: Python/HTML/CSS, finance background, wants layer understanding, avoid deep unfamiliar territory)

1. #73 (docs) — Pure README + .env.example edit, zero code required. Perfect first foothold for
   mapping the repo without needing to understand any layer internals. Fits squarely in the "docs"
   lane a Python/CSS person can own confidently.
2. #63 (test fixture) — Pure Python, just extend a text fixture or tighten an assertion in one test
   file. No system architecture knowledge needed; builds test-reading fluency.
3. #68 (ZeroDivisionError) — Python defensive-coding fix + test cleanup. Slightly more involved
   (understanding a BM25 index call), but still a single guard condition and a one-line marker removal.
4. #61 (SQLAlchemy) — One-liner fix but requires knowing SQLAlchemy's text() API and touching the
   API/DB layer. Good learning opportunity, but the fit profile explicitly flags "issues that assume
   deep familiarity with how layers interact" as something to avoid — rank last.

---
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
  "checks": [
    {"name": "repo-active", "grade": "pass", "evidence": "Repo last pushed 2026-09-16, 6 days before grading date 2026-09-22 — well within 90 days"},
    {"name": "scope-settled", "grade": "pass", "evidence": "Issue describes one bounded docs fix (sync README and .env.example); 0 comments, no debate, no prior PRs"},
    {"name": "maintainer-alive", "grade": "pass", "evidence": "Aburke225 (COLLABORATOR) is author of last 5 commits, most recent 2026-09-16 (within 90 days)"},
    {"name": "unclaimed", "grade": "pass", "evidence": "No assignee, no open linked PRs, 0 comments"},
    {"name": "ai-policy-allowed", "grade": "pass", "evidence": "No CONTRIBUTING.md found; silence counts as allowed"}
  ],
  "verdict": "accept"
}
```

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Smoke test, `--limit 3` (issue-01, issue-02, issue-03): 3/3 agree (partial run, no bar).
2. Full run, no `--save-run`: 18/20 agree — `categories: claimed 4/4 clear-accept 7/8
   dead-repo 3/3 policy 1/1 scope 3/4` — PASS. Two disagreements: issue-15 (graded
   accept, gold reject) and issue-19 (graded reject, gold accept).
3. Targeted re-test, `--only issue-15,issue-19` after revising the `scope-settled`
   check: 2/2 agree.
4. Full confirming run, `--save-run eval-run.txt`: **19/20 agree** —
   `categories: claimed 4/4 clear-accept 8/8 dead-repo 3/3 policy 1/1 scope 3/4` — PASS
   (bar: 18/20). This is the run committed in `eval-run.txt`.

**Issue analysis**

`issue-15` (zulip/zulip#19589). My rubric's first version graded this **accept**, but
the gold label is **reject**. The original ask reads as a clean, bounded feature request
(separate the `command` and `text` fields in a Slack-compatible outgoing webhook), and
there is no "should we do this" debate — everyone in the thread agrees it's reasonable.
What my rubric missed at first is a different signal entirely: the comment thread shows
roughly ten different contributors claiming the issue via `@zulipbot claim` since 2021,
each auto-unassigned after 14 days of inactivity, and two separate pull requests
(#20840, #23123) opened and later closed without merging. That repeated
claimed-then-abandoned pattern is itself evidence the issue is harder to finish than it
looks — a signal the evidence guide names directly ("long-open issues with several
abandoned unmerged PR attempts signal real difficulty") but that my first
`scope-settled` check didn't test for; it only checked for open design debate. I added
an explicit clause for prior abandoned PR attempts, re-ran with `--only issue-15`, and
the rubric now correctly grades this issue reject.

**Check rationale**

Quoting `scope-settled` from `rubric.md` as uploaded:

> issue describes one bounded problem (a bug report's implicit goal, e.g. "stop it from
> freezing," counts as expected behavior even if several alternative fix approaches are
> suggested); no open "should we do this" debate; no more than 1 prior closed/abandoned
> PR attempt at this same fix

I wrote it this way because a bounded-*sounding* issue can be a bad first pick for two
different reasons, not one: either nobody has agreed on what to build (open debate), or
people have already tried and failed to build it (abandoned PR attempts). My first draft
only tested for the first reason, which is why it missed issue-15. I also added the
"implicit goal" clause after a separate false-reject (issue-19, a maintainer-filed bug
report that never spelled out "expected behavior" in so many words, and which listed
several alternative fix approaches that my earlier wording mistook for an unscoped,
multi-part umbrella issue).

**Trade-offs**

The "no more than 1 prior closed/abandoned PR" threshold is a number I picked, not one
the evidence guide gave me directly, so it can be wrong in both directions — an issue
with exactly one honest attempt that simply ran out of contributor time isn't
necessarily as doomed as one with five. I re-ran `--only issue-15,issue-19` right after
this edit specifically to confirm it fixed issue-15 without breaking issue-19 (a
maintainer-filed bug with no history of failed attempts), and both came back correct.
I also know this check still has a blind spot: issue-20 (excalidraw's "add company logo
to the toolbar" request) still passes `scope-settled`, even though it hides a real
product decision, because it has zero comments and no abandoned PRs to flag — my check
only tests for the two scope-failure patterns I could name from evidence, not every
pattern that exists. I chose not to add a third clause for that case, since a single
data point risked overfitting a rule that might reject legitimate bot-filed issues.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to interests and time available.** #73 is a pure documentation/config fix
   (aligning `README` and `.env.example` on which LLM API key variable name to set) —
   no application code to touch. Coming from a finance/data-analysis background with no
   hands-on production software experience, this gives me a low-risk first contribution
   where I can focus entirely on learning the actual contribution workflow (fork,
   branch, PR, review) instead of also fighting unfamiliar code.
2. **What the verdict identified vs. what I weighed myself.** The rubric confirmed the
   objective safety signals: the repo pushed code 6 days ago, the same collaborator has
   been actively committing, nobody has claimed it or opened a PR against it, and there
   is no contribution policy banning AI-assisted work. What the rubric couldn't weigh is
   how I'd feel actually doing the work — I picked #73 over the technically similar #63
   and #68 partly because a docs mismatch is something I can verify correctness of
   myself (does the `.env.example` name match what the code actually reads?) without
   first having to learn how the app's config loading works end to end.
3. **Anticipated difficulty in claiming it.** Very low. Zero comments, no assignee, and
   the fix is essentially picking one canonical variable name and updating whichever
   file is wrong. I don't expect contention over who claims it or back-and-forth about
   approach.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
