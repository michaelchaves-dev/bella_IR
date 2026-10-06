# Bella IR

**Bella IR (Bella Intermediate Representation)** is a machine-oriented semantic representation layer for AI systems. Its purpose is to reduce repeated natural-language overhead while preserving the **minimum reconstructable meaning** needed for instructions, memory, relationships, state, retrieval, and agent-to-agent communication.

Bella IR uses the **Zero Language** methodology: separate meaning from verbose wording, preserve the semantic invariants that matter, and reconstruct human-readable or executable context only when needed.

## Core idea

Traditional agent systems repeatedly move large amounts of prose between prompts, memory stores, tools, and agents.

Bella IR explores a different path:

```text
Natural language
      ↓
Semantic extraction
      ↓
Bella IR / Zero Language
      ↓
Memory · Agents · Retrieval · State
      ↓
Reconstruction when needed
```

The target is **semantic conservation**, not lossy summarization.

> Preserve the smallest sufficient representation that can be reconstructed without silently changing meaning.

## Intended representation classes

Bella IR was designed around five broad kinds of semantic objects:

1. **Instruction** — what must or must not happen.
2. **Memory** — durable facts, decisions, observations, or learned state.
3. **Relation** — links and dependencies between semantic objects.
4. **State** — current conditions, supersession, status, or execution context.
5. **Agent message** — compact inter-agent communication.

## Design principles

Bella IR is intended to support:

- **reference-first storage** rather than repeating the same language;
- **canonical semantic references** for known concepts;
- **delta-state persistence** so only changes need to be carried forward;
- **versioned dictionaries / schemas** for reconstructability;
- **relationship preservation**, not just keyword compression;
- **provenance and source references** where meaning depends on origin;
- **temporal validity and supersession** so stale state does not silently survive;
- **fail-closed uncertainty** — preserve more information or fall back when fidelity is uncertain;
- **semantic deformation testing** to detect meaning loss;
- **interop across agents, models, memory, and retrieval systems** without requiring the same natural-language phrasing.

A key invariant is:

> **No conservation optimization may silently change semantic state.**

## Relationship to the memory stack

Bella IR is one layer in the broader bellaOS memory / representation architecture.

```text
CNS Memory Core
      ↓
ZeroFold / Zero Atoms / Zero Language
      ↓
Bella IR
      ↓
RAG / agents / workflows / reconstruction
```

The intended responsibilities are distinct:

- **ZeroFold** — conservation, deduplication, semantic atoms, compact context, and fidelity-aware reduction.
- **CNS Memory Core** — persistence, provenance, lifecycle, supersession, ROI/value, and durable state.
- **Bella IR** — portable semantic representation between those systems and the agents/models that consume the information.
- **bellaOS** — governance and authority remain above the representation layer.

Bella IR is **not** an authority system and should not grant execution permission merely because information is encoded in it.

## Minimum reconstructable meaning

The design goal can be expressed as:

```text
preserve:
  intent
  constraints
  relationships
  provenance
  state
  uncertainty
  authority boundaries

remove:
  redundant wording
  repeated context
  unnecessary formatting
  duplicate explanation
```

Compression is useful only when the reconstructed meaning survives.

## Status

**Representation / optimization research layer.**

This repository currently documents the design direction. It does **not** claim that a complete Bella IR grammar, full interop suite, production runtime, or validated universal token-savings benchmark exists on the main branch.

Earlier work explored versioned specs, dictionaries, examples, semantic invariants, and fidelity testing. Those ideas remain design targets unless and until the corresponding implementation and tests are present and verified here.

No percentage savings claim should be inferred from this README without a reproducible benchmark and preserved-output fidelity test.

## Intended evaluation

A valid Bella IR benchmark should measure both efficiency **and** semantic survival, including:

- representation size;
- reconstruction fidelity;
- critical-concept survival;
- provenance preservation;
- relationship preservation;
- uncertainty preservation;
- round-trip deformation;
- downstream task quality.

A smaller representation that changes meaning is a failure, not an optimization.

---

**Subtract Architect Studios**  
*Intelligence by Subtraction*

<!-- SAS-IP-FOOTER-v1 -->
---
**Subtract Architect Studios™**  
Copyright © 2026 Michael F. Chaves. All rights reserved in original Subtract Architect Studios materials except as expressly licensed. See [IP_NOTICE.md](./IP_NOTICE.md). Existing open-source and third-party licenses remain controlling for materials they cover.
