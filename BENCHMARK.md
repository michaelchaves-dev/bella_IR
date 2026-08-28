# Bella IR Benchmark Standard v0.1

**Goal:** set a hostile control early. Bella IR must work on real-world human/agent input, not curated compression demos.

## Principle

A benchmark is only useful if it can embarrass the system.

Bella IR MUST be evaluated on malformed, redundant, ambiguous, emotionally charged, code-switched, transcription-damaged, context-dependent input as well as clean prose.

No benchmark item may be rewritten into cleaner English before encoding.

## Control tracks

### C0 — Clean control
Well-formed prose and structured instructions. Establishes baseline behavior only.

### C1 — Natural conversational noise
Fragments, repetition, filler, corrections, slang, profanity, missing punctuation, pronouns with unclear antecedents, run-on sentences.

### C2 — Speech-to-text damage
Homophones, dropped words, substituted words, punctuation loss, false sentence boundaries, partial names, mistaken technical terms, accidental pluralization, repeated clauses, phonetic spellings, and mid-sentence corrections.

### C3 — Semantic collision
Input containing multiple plausible interpretations. Encoder MUST preserve uncertainty rather than collapse to one convenient meaning.

### C4 — Context dependency
Meaning requires prior turns, project aliases, repo names, standing rules, or referenced documents. System must either resolve valid context or fail closed with unresolved references.

### C5 — Mixed-domain shit salad
A single input may combine implementation detail, personal shorthand, architecture, jokes, TODOs, constraints, code, repo names, numbers, changed decisions, and unrelated side thoughts. The encoder must separate durable semantic units without laundering ambiguity.

### C6 — Adversarial deformation
After encoding, deliberately remove/reorder/collapse candidate representation until fidelity fails. This track measures the actual semantic compression boundary.

### C7 — Agent relay
Human -> agent -> Bella IR -> different agent -> natural-language reconstruction. Source agent may not provide hidden context during rehydration.

### C8 — Long-horizon memory
Encode an item, separate it from source context, then decode/retrieve after intervening unrelated material. Tests reference integrity, versioning, and semantic drift.

## Hard invariants

Each item declares which invariants are required:

`INT SEM PRV EVD REV BGT VAL UNC`

A run is an automatic FAILURE if a required invariant is materially changed, omitted, invented, or silently resolved from ambiguity.

## Ground-truth annotation

Every benchmark item SHOULD contain:

- `raw`: exact untouched source input
- `required_meaning`: minimum facts/intent that must survive
- `must_not_infer`: plausible but unsupported interpretations
- `invariants`: required invariant IDs
- `context_refs`: context genuinely available to the encoder/decoder
- `allowed_loss`: wording/style details that may be discarded
- `severity`: LOW/MED/HIGH/CRITICAL consequence of semantic corruption

Ground truth is written separately from the source and is NEVER supplied to the encoder.

## Scoring

Report metrics independently. Do not hide failures inside one composite score.

1. **Semantic Recall** — required meaning recovered.
2. **Semantic Precision** — reconstruction does not invent meaning.
3. **Invariant Survival** — pass/fail per declared invariant.
4. **Uncertainty Preservation** — ambiguity remains explicit where required.
5. **Reference Integrity** — referenced objects resolve to correct immutable targets.
6. **Token Ratio** — IR tokens / source tokens for each tokenizer tested.
7. **Byte Ratio** — IR bytes / source bytes.
8. **Encode Latency**.
9. **Decode Latency**.
10. **Total Cost** — including encode/decode overhead.
11. **Round-trip Failure Rate**.
12. **Catastrophic Semantic Failure Rate** — HIGH/CRITICAL items with material meaning corruption.

## Initial ship gates

Bella IR v0.x MUST NOT be described as production-safe unless the evaluated control set meets all of the following:

- `CRITICAL semantic failures = 0`
- `HIGH semantic failures = 0`
- `Invariant survival = 100%` on required HIGH/CRITICAL invariants
- `Uncertainty preservation = 100%` on HIGH/CRITICAL ambiguous cases
- `Reference integrity = 100%` for resolvable HIGH/CRITICAL references
- Every unresolved required reference fails closed
- Compression claims are reported per tokenizer/model, never universally

Token savings are a secondary objective. A run with excellent compression and semantic corruption is a failed run.

## Stress corpus composition

The canonical control corpus SHOULD contain at minimum:

- 20 clean items
- 30 ordinary conversational items
- 50 real or realistically generated talk-to-text damaged items
- 30 ambiguous/semantic-collision items
- 30 context-dependent items
- 50 mixed-domain shit-salad items
- 30 agent-relay items
- 20 long-horizon/reference/versioning items

Minimum initial corpus: **260 items**.

At least 25% SHOULD be held out from encoder-development iteration to reduce benchmark overfitting.

## Real-source rule

Whenever permission and privacy constraints allow, prefer naturally occurring input over synthetic examples. Raw source must be scrubbed or replaced where retaining sensitive data is unnecessary for the semantic test.

## Anti-cheating rules

- Do not clean grammar before encoding.
- Do not let decoder see original source.
- Do not tune against the held-out set.
- Do not exclude failures because the input is 'bad English.'
- Do not count references as compression unless referenced material already exists and its storage/lookup cost is accounted for.
- Do not compare token counts across different tokenizers without labeling them.
- Do not claim savings from output shortening if required meaning was lost.
- Do not let an LLM judge its own output as the sole evaluator on HIGH/CRITICAL cases.

## Human adjudication

HIGH and CRITICAL disagreements require human adjudication against the written ground truth. Automated semantic graders may assist but may not silently override explicit invariants.

## North-star test

If Bella IR can reliably decode the ugliest real speech-to-text project input into the same actionable intent, constraints, uncertainty, references, and state — using fewer resources end-to-end — then it is doing useful work.

If it only wins on polished prose, it fails the mission.
