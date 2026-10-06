# Voice guide: how I talk upstream

## Who I am in threads

I'm coming from a finance/data-analysis background with no prior hands-on software
engineering experience — this is my first open-source contribution. I'm here to learn
the real workflow, not to perform expertise I don't have yet. Readers should expect
careful, modest claims backed by what I actually ran, not confident diagnoses.

## Rules I write by

### Rule: never promise a fix or a date
I only promise investigation, never an outcome or a timeline I don't control.

- Wrong: "I'll have a fix ready by tomorrow."
- Right: "I'll look into this and report back with what I find."

### Rule: state uncertainty as uncertainty
If I haven't confirmed something, I say so instead of asserting it as fact.

- Wrong: "This is definitely caused by the debounce logic."
- Right: "This looks like it could be related to the debounce logic, though I haven't confirmed it."

### Rule: no padding or hedging filler
I say what I found plainly, without stacking qualifiers to sound careful instead of being careful.

- Wrong: "I think maybe this could potentially possibly be an issue, not totally sure but..."
- Right: "I wasn't able to reproduce this yet — here's exactly what I tried."

### Rule: disclose AI assistance when the repo's policy asks for it
My workflow is AI-assisted; when a repo's stated policy requires disclosure, I say so plainly rather than presenting the report as entirely hand-written.

- Wrong: (silently posting a report as if fully hand-written on a repo whose policy requires disclosure)
- Right: "I used Claude Code to assist with this investigation; I reviewed and tested everything reported here myself."

## Things I never post

- A guaranteed timeline or a promised fix.
- A root cause or diagnosis I haven't actually verified with evidence.
- A "+1" or "same as above, can confirm" piggyback comment.
- Confidence language ("definitely," "guaranteed," "100%") beyond what I actually ran and observed.
