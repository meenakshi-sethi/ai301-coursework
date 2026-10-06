# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

meenakshi-sethi

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-6026853706

```
My plan for #73, built on my repro above.

Cause: README.md and docs/SETUP.md both tell you to set OPENROUTER_API_KEY, and core/config.py already defines a field for it, but .env.example has no line for it at all. Checked get_embedding_provider() in ingestion/embeddings/provider.py - it only accepts "mock" and "openai", and openrouter_api_key isn't read anywhere outside core/config.py. So this is just a missing line in the example file, not a case where "openrouter" is already a working provider option.

Scope: just .env.example. Adding OPENROUTER_API_KEY=sk-or-your-key-here under the existing OPENAI_API_KEY line. Not touching core/config.py, README, SETUP, or the LLM_PROVIDER options comment, since listing "openrouter" there would describe an option the code doesn't actually support yet.

Test: re-run the grep from my repro comment and confirm OPENROUTER_API_KEY shows up in .env.example.

I'll post an update once the change is up.
```

---

## Your branch

**Branch**

docs/73-env-example-openrouter-key

**Evidence**

Before (from my Unit 2 repro comment):

```
$ grep -n "LLM_PROVIDER\|API_KEY" .env.example
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
```

After (re-run on the branch, after the fix):

```
$ grep -n "LLM_PROVIDER\|API_KEY" .env.example
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
20:OPENROUTER_API_KEY=sk-or-your-key-here
```

`git diff` on the branch confirms the only change is this one added line in `.env.example`:

```
diff --git a/.env.example b/.env.example
index 1be8b38..995865c 100644
--- a/.env.example
+++ b/.env.example
@@ -17,6 +17,7 @@ VECTOR_DB_URL=http://localhost:8001
 # Options: "mock" (default, no API key needed), "openai"
 LLM_PROVIDER=mock
 OPENAI_API_KEY=sk-your-key-here
+OPENROUTER_API_KEY=sk-or-your-key-here
 
 # App settings
 APP_ENV=development
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke test, `--limit 5`: 5/5 agree (partial run, no bar).
2. Full confirming run, `--save-run eval-run.txt`: **20/20 agree** — `categories:
   clear-accept 7/7 scope-creep 4/4 thread-convention 2/2 unbuildable 3/3 wrong-cause
   4/4` — PASS (bar: 18/20). This is the run committed in `eval-run.txt`. No revisions
   were needed between the smoke test and the full run.

**Package analysis**

`pkg-07` (processing/p5.js#8930). My rubric correctly graded this **reject**, matching
the gold label. The candidate plan blames the Friendly Error System (FES) being
tree-shaken out of the 2.x bundle as the cause of the missing error message. But the
package's own repro evidence includes a control run that calls an FES instance method
directly in the same build, and that control run prints a friendly error successfully —
which rules out "FES is absent from this build" as the cause. My `diagnosis-grounded`
check is written to grade the plan's stated cause directly against the repro evidence's
control runs, not against how confidently the plan states it, so it failed this plan on
exactly that contradiction, and the verdict rule holds it (required check, so one fail
is enough).

**Check rationale**

Quoting `diagnosis-grounded` from `rubric.md` as uploaded:

> the stated cause is consistent with what the repro evidence actually shows; graded
> against the repro evidence directly, never against what the thread merely believes —
> a plan that adopts a popular but evidence-contradicted theory from the thread still
> fails this

I wrote it this way because of the worksheet's calib-03 trap in the Unit 3 activity: a
plan can adopt a theory the thread has already settled on with full confidence, while
the package's own evidence (there, a timing matrix; here, a control run) contradicts it.
A check that only asked "does the plan sound confident and well-reasoned" would pass
that kind of plan; grounding the check explicitly in the repro evidence, and saying
outright that thread consensus doesn't count as grounding, is what makes it catch pkg-07
and the calib-03-style trap the same way.

**Trade-offs**

This check only has the repro evidence actually included in the package to check
against — if that evidence doesn't happen to include a control run or step that
contradicts a wrong cause, a wrong diagnosis could pass simply because nothing in the
package rules it out. I didn't need to loosen or tighten this check after the first full
run (it scored 20/20 on the first attempt, including both wrong-cause packages and the
non-scored calib-03 trap when I checked it by hand), so there's no re-run to point to;
the check's design was validated directly against calib-03's "defensible-sounding but
evidence-contradicted" case before I ever ran the harness, which is why it held up
without revision.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
