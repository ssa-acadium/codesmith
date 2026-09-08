---
name: codesmith
description: Use when writing, reviewing, or refactoring human-maintained source code where clarity, compactness, and dependable behavior matter. Use when code risks over-engineering, duplicated capability, needless abstractions/configuration/state, vague naming, hidden failure paths, fragile concurrency, or dev-scale shortcuts. Do NOT use for generated/vendor code, throwaway exploration, or configuration-only edits.
license: MIT
metadata:
  author: you
  version: "0.2.0"
---

# Codesmith

**Core principle:** eloquent code is compressed understanding: the fewest trustworthy moving parts that state the intent clearly and remain correct when failure, concurrency, and realistic scale arrive.

Sleek is not merely short. A reader should be able to glance at the main path and understand what happens, which concepts matter, what changes state, and where failure can escape.

## Route by need

| Need | Load |
|---|---|
| Names and domain vocabulary | `references/naming.md` |
| Reuse, YAGNI, dependencies, config | `references/restraint.md` |
| Functions, modules, interfaces, DI | `references/structure.md` |
| State, races, locks, concurrency | `references/state-concurrency.md` |
| Files, memory, batching, scale | `references/resources-scale.md` |
| Tests and failure coverage | `references/behavior-tests.md` |
| Comments and rationale | `references/documentation.md` |
| Review/refactor | `references/review-checklist.md` |
| Methodological foundations | `references/foundations.md` |
| Evaluation cases | `references/evals.md` |

## Understand, then subtract

Read the touched code and trace the real flow. Identify the contract, invariants, failure modes, state lifetime, concurrency, and plausible data size. Then stop at the first rung that genuinely solves the problem:

1. Need it now? If not, omit it.
2. Already in the codebase? Reuse it.
3. Language/stdlib does it? Use it.
4. Platform/framework/database does it natively? Use it.
5. Installed, well-suited dependency does it? Reuse it.
6. Direct function/expression/data structure enough? Keep it direct.
7. Can the value be derived instead of stored? Derive it.
8. Only then add the minimum custom mechanism.

A small change in the wrong place is not elegance. Understanding is never the part to optimize away.

## Naming is design

Name the nouns, verbs, states, units, effects, and invariants before inventing machinery. Good names expose the model; vague names often expose vague design. Use one stable term per concept and let scope provide context instead of manufacturing sentence-length identifiers.

## The code must earn its simplicity

- Prefer direct composition over ceremonial indirection. One implementation rarely needs an interface, factory, container, and config switch.
- Prefer derived state. Stored/tracked state needs ownership, lifetime, bounds, and cleanup across failure/cancellation/retry.
- Avoid shared mutable state before designing locks. Prefer native atomic, transactional, queue, and concurrency primitives.
- Use vetted security/cryptographic primitives; never trade a trusted implementation for hand-rolled cleverness.
- Use bounded/streaming resource handling when inputs can grow. “Works at dev scale” is not a contract.
- Treat configuration as an interface; add knobs for real variability, not imagined futures.
- Test material boundaries and failure behavior, not only the happy path.

## Known gotchas

- One line can be denser than ten clear lines; hidden obligations are not compression.
- Modularity can become fragmentation; a boundary must remove cognitive load.
- Dependency injection is useful when it buys real variability/lifecycle/boundary control, not as automatic ceremony.
- Simplicity is subordinate to correctness at trust, persistence, resource-lifetime, and concurrency boundaries.
