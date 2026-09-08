# Codesmith Review and Refactor Checklist

Do not optimize cosmetics before understanding the flow. The objective is fewer concepts to carry, not merely fewer lines.

## 1. Comprehend

- [ ] Trace the real path end to end before changing it.
- [ ] State the contract and important invariants.
- [ ] Identify meaningful failure, cancellation, persistence, concurrency, and data-size constraints.
- [ ] Distinguish essential domain complexity from accidental implementation complexity.

## 2. Subtract

For each new/existing mechanism:

- [ ] Does it need to exist now (YAGNI)?
- [ ] Is the same concept already implemented in the repository?
- [ ] Can stdlib/language/native platform capability replace it?
- [ ] Can an already-installed dependency replace custom machinery?
- [ ] Can direct functions/data replace an interface/factory/container/wrapper?
- [ ] Can a value be derived instead of tracked?
- [ ] Can dead config, flags, adapters, and one-use abstractions be deleted?

## 3. Vocabulary

- [ ] One concept uses one stable term inside its context.
- [ ] Nouns, verbs, states, units, relationships, and effects are apparent.
- [ ] Names reduce ambiguity without repeating scope.
- [ ] Function names do not disguise meaningful mutation/I/O.
- [ ] Tracked collections reveal lifecycle purpose (`inFlight...`, `pending...`, etc.).
- [ ] Junk-drawer names are challenged when scope does not make them precise.

## 4. Shape and boundaries

- [ ] The main path is scannable.
- [ ] Helpers/modules create semantic leverage rather than file jumps.
- [ ] Shallow pass-through layers are questioned.
- [ ] Dependency injection solves a real boundary/lifecycle/variability problem.
- [ ] Related protocol/domain knowledge stays together.
- [ ] Effects and failure behavior are discoverable from the relevant interface.

## 5. State and concurrency

- [ ] Stored state has an owner, source of truth, lifetime, and bound.
- [ ] Registrations/caches/pending maps clean up on success and failure.
- [ ] Cancellation/timeout/retry paths do not leak state.
- [ ] Shared mutable state is avoided where possible.
- [ ] Locks protect named invariants and release on exceptional paths.
- [ ] Check-then-act, lost-update, duplicate-completion, and cleanup races are considered.
- [ ] Native atomic/transaction/queue primitives are preferred to custom locking schemes.

## 6. Resources and scale

- [ ] File/data handling is bounded or explicitly safe to load whole.
- [ ] Handles/connections/resources close structurally.
- [ ] Partial-write behavior is safe where durability matters.
- [ ] No obvious N+1, repeated full scans, unbounded queues, or accidental O(n²) at plausible scale.
- [ ] Scalable native forms are used when they are equally simple.

## 7. Security and configuration

- [ ] No custom cryptography/security protocol where vetted primitives exist.
- [ ] Security-sensitive randomness/keys use appropriate trusted APIs.
- [ ] Config options correspond to real variation, not imagined future need.
- [ ] Every added knob has validation/default/testing ownership.

## 8. Evidence

- [ ] Tests express observable behavior.
- [ ] Material boundary and failure paths are covered.
- [ ] Cleanup is tested when resources/state can leak.
- [ ] Retry/idempotency/concurrency behavior is tested when present.
- [ ] Relevant tests pass after refactoring.

## Refactor order

When several layers are weak:

1. establish a passing baseline;
2. delete/reuse unnecessary machinery;
3. normalize vocabulary;
4. simplify main-path control flow;
5. inline shallow indirection / extract meaningful boundaries;
6. repair state, resource, and concurrency lifecycles;
7. update rationale/docs;
8. strengthen tests around the contract and failure modes.
