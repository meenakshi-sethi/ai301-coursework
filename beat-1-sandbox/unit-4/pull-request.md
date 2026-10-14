# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/206

**Branch**

docs/73-env-example-openrouter-key

## Eval iterations

**Run history**

1. Smoke test (`--only` on 5 packages spanning categories): confirmed the harness and the
   installed skill run end to end before spending a full run (partial run, no bar).
2. Full run #1, `--save-run eval-run.txt`: **18/20 agree** — both misses in `clear-accept`
   (my first `test-evidence-on-target` check required an explicit before/after transcript
   for every failure mode a plan's test plan named, and wrongly failed two genuine accepts
   whose second named case was only confirmed via a named regression test + a stated "passes"
   outcome). Already past the 18/20 bar, but with a category gap I wasn't satisfied with.
3. Revision `--only` rerun (the four `not-tested` rejects plus `pkg-02` as a canary):
   confirmed a loosened pass condition didn't flip any genuine reject.
4. Revision `--only` rerun (all 11 `clear-accept`/`not-tested` packages together): confirmed
   the rewritten check now passes the two previously-wrong accepts without flipping anything
   else.
5. Full run #2, `--save-run eval-run.txt`: **20/20 agree** — `categories: clear-accept 7/7
   not-tested 4/4 silent-drift 4/4 standards-wall 2/2 unreviewable 3/3` — PASS (bar: 18/20).
   This is the run committed in `eval-run.txt`.

**Package analysis**

`pkg-07` (golangci-lint/golangci-lint#6671). My rubric correctly graded this **reject**,
matching the gold label. The plan's own "Test plan:" sentence names one specific repro to
re-run: "re-run the two-file repro, expect `a.go` and `b.go` each correctly rewritten and
`go build ./...` to pass; the one-file control unchanged." The candidate PR's test evidence
only runs `golangci-lint run --fix ./onefile/ && go build ./onefile/` — the one-file
**control** case, which the bug report says already worked — and the one added unit test
(`TestGroupEditsByFileBuildsGroups`) only exercises edits that resolve to the same file
(`a.go:3`, `a.go:9`), never a genuinely cross-file case. The two-file repro that actually
demonstrates the fix (and is the only thing the plan's test plan asked to re-run) is never
shown. My `test-evidence-on-target` check is written to fail exactly this shape — evidence
that substitutes an already-working path for the named failure mode — so it failed this
package on that substitution, and the verdict rule holds it (required check, one fail is
enough).

**Check rationale**

Quoting `test-evidence-on-target` from `rubric.md` as uploaded:

> every failure mode or repro the plan's "Test plan:" sentence itself (not the scope or
> files list) explicitly names is addressed *somewhere* in the package — a before/after
> transcript, or a specifically named regression test in the diff whose passing outcome is
> stated — even if that outcome is only reported as "unchanged" or "passes" rather than a
> full transcript; fails only when a named failure mode has no corresponding transcript,
> named test, or stated outcome anywhere in the diff, description, or test evidence
> (silence), or when the evidence substitutes a different, already-working code path in its
> place; it does not fail merely because the fix's scope touches more than one function or
> file while the test-plan sentence names only one repro, and it does not fail merely
> because a named item's confirmation is terser than another's

I rewrote it to this exact shape after my first full run (18/20): the earlier version
required an explicit before/after transcript for every failure mode the plan's test plan
named, full stop. That correctly rejected the real `not-tested` traps (including `pkg-07`
above), but it also wrongly rejected two genuine clear-accepts whose second named test case
was confirmed only through a named regression test plus a stated "passes"/"unchanged"
outcome, not a full transcript — the check was punishing terseness, not absence. I rejected
a stricter alternative (require a transcript specifically, no named-test substitute) because
it would keep failing those same legitimate accepts; the fix instead narrows the fail
condition to two concrete things: silence (nothing named anywhere) and substitution (a
different, already-passing path standing in for the real one) — which is exactly what
catches `pkg-07`'s one-file-only evidence without penalizing a terse-but-present
confirmation elsewhere.

**Trade-offs**

This check only fails on silence or substitution, so a package whose test evidence asserts
a named test "passes" without ever actually showing that test's content or output would
still pass this check — it has no way to verify a stated-but-unshown outcome is true rather
than just claimed, since eval mode never runs code, only reads the bundle text. I accept
this gap because tightening it back to "always require a transcript" is the exact change
that broke the three genuine clear-accepts in full run #1 (confirmed by re-running
`pkg-02`/`pkg-05`/`pkg-19` as canaries in revision run 4): a check that can't be fooled by an
unverified "passes" claim is also a check that fails real accepts whose second test case is
reported tersely. Re-running all 7 `clear-accept` and all 4 `not-tested` packages together in
that same revision run confirmed the loosening fixed the two wrong rejects without flipping
any of the four genuine `not-tested` rejects (including `pkg-07`) back to accept.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
