# Bella IR v0.1 — Core Specification

**Status:** Experimental / pre-benchmark

Bella Intermediate Representation (Bella IR) is a machine-oriented semantic representation layer for AI systems. It uses the **Zero Language** methodology to reduce redundant natural-language representation while preserving reconstructable, actionable meaning.

## 1. Design objective

Minimize representational cost subject to semantic fidelity:

`min(cost(R)) subject to fidelity(decode(R), source) >= τ`

Bella IR does not assume that fewer characters means fewer model tokens. Token, byte, latency, cost, and fidelity effects MUST be measured independently.

## 2. Invariants

Compression MUST NOT silently alter required:

- `INT` — intent
- `SEM` — semantic meaning
- `PRV` — privacy constraints
- `EVD` — evidence/provenance
- `REV` — reversibility boundaries
- `BGT` — budget/resource constraints
- `VAL` — validation requirements
- `UNC` — material uncertainty

If preservation cannot be demonstrated, retain the richer representation.

## 3. Layers

- **L0 Human:** original human/model language.
- **L1 Canonical Semantic:** normalized entities, predicates, constraints, relations, confidence, provenance.
- **L2 Symbol:** stable dictionary IDs for recurring concepts and protocols.
- **L3 Reference Graph:** references canonical objects rather than duplicating definitions.
- **L4 Delta:** stores only changes from referenced state when reconstruction remains deterministic.

Each layer is optional. The encoder stops when additional compression threatens fidelity or costs more than it saves.

## 4. Canonical record

Bella IR v0.1 uses an ASCII-safe wire form so existing tooling can inspect it without a proprietary tokenizer.

```text
BIR/0.1|K:<kind>|ID:<id>|S:<subject>|P:<predicate>|O:<object>|C:<constraints>|R:<refs>|D:<delta>|Q:<confidence>|V:<codec-version>
```

Fields MAY be omitted when absent. Canonical field order is fixed as shown above.

Core kinds:

- `MEM` memory/state
- `TASK` instruction/work item
- `MSG` agent message
- `FACT` grounded assertion
- `RULE` invariant/policy
- `REF` reference declaration
- `DELTA` state change

## 5. References

Canonical objects receive immutable IDs. Human-readable aliases MAY exist, but persisted references resolve by immutable ID + dictionary/codec version.

Rule: **Never store twice if a reference can recreate it.**

A reference MUST fail closed when its target or compatible decoder cannot be resolved. It MUST NOT silently guess the missing definition.

## 6. Delta representation

A delta MUST identify its base and operation explicitly.

```text
BIR/0.1|K:DELTA|R:@<base-id>|D:<path>:<old>><new>|V:0.1
```

A delta is valid only if base + delta deterministically reconstruct the intended state.

## 7. Uncertainty and loss boundary

Bella IR is not allowed to compress uncertainty away. Ambiguous source material SHOULD retain alternatives, confidence, or a pointer to richer source context.

When the encoder cannot establish fidelity above threshold `τ`, it MUST fall back to a less compressed layer.

## 8. Semantic Deformation Testing (SDT)

For candidate representation `R`:

1. Encode source into `R`.
2. Rehydrate `R` without access to the source text.
3. Compare source and reconstruction against required invariants.
4. Apply controlled deformation: remove wording, replace repetition with references, reorder non-semantic structure, or attempt a lower layer.
5. Rehydrate again.
6. Accept the deformation only when fidelity remains >= `τ` and all required invariants survive.

This determines the practical compression boundary.

## 9. Versioning

Every persisted Bella IR record MUST identify a codec/dictionary version. Existing symbol meanings are immutable within a version. Changed meanings require a new version or new symbol ID.

## 10. Trust boundary

Bella IR is an intermediate representation, not an authorization mechanism. Decoding an instruction does not grant permission to execute it. Existing human-review, security, privacy, and reversibility gates remain authoritative.

## 11. Benchmark contract

No savings claim is valid without measurement against the same source corpus.

Minimum metrics:

- source bytes vs IR bytes
- source tokens vs IR tokens, identified by tokenizer/model
- encode latency
- decode latency
- end-to-end inference cost
- reconstruction fidelity
- invariant failure rate
- unresolved-reference rate

Initial benchmark corpus SHOULD include real bellaOS methodology, repo documentation, memory records, RAG material, and agent messages rather than compression-friendly synthetic examples.

## 12. v0.1 non-goals

Bella IR v0.1 does NOT claim:

- a new tokenizer or model-native vocabulary
- universal compression across all models
- lossless compression of arbitrary prose
- quantum computation or quantum memory
- replacement of source evidence where provenance requires retention

The topological analogy is an engineering inspiration: preserve semantic invariants despite representational deformation.
