# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

meenakshi-sethi

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-5885496931

```
Hi! I'd like to work on this as a first contribution, syncing README and .env.example on which LLM API key to set. I'll look into this and report back with what I find.
```

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-5885618526

```
Did a quick repro on this. Cloned the repo at commit `2f4e82f` and checked the three files mentioned.

`README.md` (line 24) tells you to add `OPENROUTER_API_KEY` when setting up. But .env.example only has `LLM_PROVIDER=mock` and `OPENAI_API_KEY` - there's no `OPENROUTER_API_KEY` line in it at all and the comment above only mentions "mock" and "openai" as options.

Checked `core/config.py` and it actually already has fields for both `openai_api_key` and `openrouter_api_key`(plus openrouter_base_url and openrouter_model), so the code supports openrouter as a provider - `.env.example` just never got updated to mention it.

So the fix is basically adding `OPENROUTER_API_KEY` (and probably "openrouter" as a listed option for LLM_PROVIDER) to `.env.example` so it matches what `README` says and what the code already supports.
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke test, `--limit 4`: 4/4 agree (partial run, no bar).
2. Full run, no `--save-run`: 16/20 agree — `categories: clear-accept 5/8 disclosure 0/1
   no-evidence 4/4 unfollowable-comms 3/3 wrong-target 4/4` — below bar. Four
   disagreements: pkg-05 (false reject, `steps-followable`), pkg-09 and pkg-10 (false
   reject, `behavior-matches-issue`), pkg-20 (false accept, disclosure category).
3. Targeted re-test, `--only pkg-05,pkg-09,pkg-10,pkg-20,pkg-06,pkg-16,pkg-04` (the four
   disagreements plus three canaries) after revising `steps-followable`,
   `behavior-matches-issue`, and `conventions-respected`: 7/7 agree, no regressions.
4. Full confirming run, `--save-run eval-run.txt`: **20/20 agree** — `categories:
   clear-accept 8/8 disclosure 1/1 no-evidence 4/4 unfollowable-comms 3/3 wrong-target
   4/4` — PASS (bar: 18/20). This is the run committed in `eval-run.txt`.

**Package analysis**

`pkg-20` (ghostty-org/ghostty#13604). My rubric's first version graded this **accept**,
but the gold label is **reject**. The reproduction itself is excellent on every proof
check: environment recorded, exact steps, and an artifact (a control run vs. the buggy
run) that shows precisely the behavior the issue describes. What sinks it is a
different family entirely: ghostty's stated AI policy requires disclosing *all* AI
usage, and neither the claim comment nor the repro report contains any disclosure
statement. My first `conventions-respected` check only failed a package if it detected
language actively hiding AI use; it did not fail a package that simply never mentioned
AI at all, so it treated silence as compliance. That is backwards for this course: every
package here is AI-assisted work by default, so on a policy that requires disclosure,
the absence of a disclosure statement is itself the violation. I rewrote the check to
say so explicitly, and it now correctly grades this package reject.

**Check rationale**

Quoting `conventions-respected` from `rubric.md` as uploaded:

> every package in this course is AI-assisted work by default; if the repo's policy
> requires disclosing AI assistance, ANY comment with no explicit disclosure statement
> fails this check, even if the comment never mentions AI — silence is not proof of no
> AI use; if the policy states nothing about AI, silence passes; separately, the claim
> comment must not promise a fix, a timeline, or certainty it has not earned

I wrote it this way after pkg-20 showed that "does the comment say anything bad about
AI" is the wrong question when disclosure is required — the right question is "does the
comment say anything at all," because in this course the assumption of AI assistance is
already true going in. I also kept the original claim-comment clause (no promised fix
or timeline) in the same check rather than splitting it out, since both are really
"does this comment respect what the repo/course expects of it."

**Trade-offs**

Treating silence as an automatic fail whenever a disclosure policy exists is calibrated
specifically for this course, where every package really is AI-assisted; it would be too
strict as a general-purpose rule, since a real stranger's comment that never mentions AI
usually means no AI was used, not that they're hiding it. I re-ran `--only
pkg-20,pkg-05,pkg-09,pkg-10` plus three canaries (`pkg-06`, `pkg-16`, `pkg-04`, one from
each other single/small category my other two edits touched) specifically to confirm
this change didn't flip any already-correct package, and none did. The check also still
only looks at whether a disclosure statement exists, not whether it's genuine or
detailed — a one-word "AI-assisted" would pass exactly as well as a full explanation,
which I accepted rather than trying to grade disclosure quality from one example.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
