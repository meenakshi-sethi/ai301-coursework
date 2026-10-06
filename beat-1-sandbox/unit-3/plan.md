# Plan: README/.env.example LLM API key mismatch (issue #73)

## Diagnosis

`README.md` (line 24) tells you to add `OPENROUTER_API_KEY` to `.env`, but
`.env.example` never lists that variable — it only has `OPENAI_API_KEY`, and its
`LLM_PROVIDER` comment names only `"mock"` and `"openai"` as options.

Repro evidence (from my Unit 2 repro comment): `core/config.py` already defines
`openrouter_api_key`, `openrouter_base_url`, and `openrouter_model` fields, and
`README.md`/`docs/SETUP.md` both instruct setting `OPENROUTER_API_KEY`. But checking
`ingestion/embeddings/provider.py`'s `get_embedding_provider()`, the only accepted
provider values are `"mock"` and `"openai"` — `openrouter_api_key` is read nowhere in
the codebase outside `core/config.py`. So the cause is a docs/example gap only for the
missing `OPENROUTER_API_KEY` line; it is NOT a case of "openrouter" being a selectable
provider option that `.env.example` forgot to list, since no such option exists in code
yet.

## Scope

**In scope:** `.env.example` only — add the missing `OPENROUTER_API_KEY` line so a
copied `.env` has a place for the key README/SETUP.md tell you to set.

**Not in scope:** `core/config.py`, `README.md`, `docs/SETUP.md`, and the
`LLM_PROVIDER` options comment — I am deliberately NOT adding `"openrouter"` as a
listed `LLM_PROVIDER` option, since `get_embedding_provider()` only accepts `"mock"`
and `"openai"`; listing openrouter there would describe an option that doesn't exist.
Wiring openrouter into the provider factory is a separate, larger change this issue
didn't ask for.

**Files touched:** `.env.example`

## Approach

1. In `.env.example`, add `OPENROUTER_API_KEY=sk-or-your-key-here` directly under the
   existing `OPENAI_API_KEY=sk-your-key-here` line, matching its placeholder format.
2. Leave the `LLM_PROVIDER` comment and its `"mock"`/`"openai"` options unchanged, since
   those are the only two values the code actually accepts today.

## Test plan

Re-running my Unit 2 repro steps against the change:

```
grep -n "LLM_PROVIDER\|API_KEY" .env.example
```

**Before** (from my Unit 2 repro comment): only `LLM_PROVIDER=mock` and
`OPENAI_API_KEY=sk-your-key-here` — no `OPENROUTER_API_KEY` line.

**Expected after:** the same grep shows an `OPENROUTER_API_KEY=sk-or-your-key-here` line
present; `LLM_PROVIDER` and its options comment are unchanged.

## Risks and unknowns

Minimal — this is a purely additive, docs-only change with no code paths touched. The
placeholder key format matches the existing `OPENAI_API_KEY=sk-your-key-here`
convention. One open question I'm not fixing here: `openrouter_api_key` is read nowhere
outside `core/config.py`, so it's unclear whether `docs/SETUP.md`'s claim that it's
"required for AI features" is accurate yet — that's a separate question from this
docs-sync fix.

## Deviations

Nothing changed. The build matches this plan exactly as posted: one line added to
`.env.example` (`OPENROUTER_API_KEY=sk-or-your-key-here`), nothing else touched. I know
this because `git diff` on the branch shows exactly that one-line addition to
`.env.example` and no other file changed.
