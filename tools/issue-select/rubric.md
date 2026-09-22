# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-active | last push to any branch (repo facts block) | last push within 90 days of the captured date | required |
| scope-settled | issue description + comment thread (look for: unresolved "should we do this" debate, closed/abandoned PRs previously attempting this exact fix, repeated claim-then-auto-unassign cycles) | issue describes one bounded problem (a bug report's implicit goal, e.g. "stop it from freezing," counts as expected behavior even if several alternative fix approaches are suggested); no open "should we do this" debate; no more than 1 prior closed/abandoned PR attempt at this same fix | required |
| maintainer-alive | maintainer first-response sample (repo facts) + who's committing in last 5 commits | at least one owner/member/collaborator commented on a recent issue within 60 days, OR a human (non-bot) appears in the last 5 commits within 90 days | required |
| unclaimed | assignee, linked/open PRs, and recent issue comments | no assignee, no open linked PR, and no *recent* comment indicating someone is actively working on it (stale old claims don't count) | required |
| ai-policy-allowed | contribution policy line (repo facts / CONTRIBUTING.md) | no outright ban on AI-assisted contributions (silence, or conditions like disclosure/testing, are both fine) | required |

## Verdict rule

Accept if and only if every required check passes. Unclear counts as fail. Preferred
checks never change the verdict — they only rank accepted issues.

