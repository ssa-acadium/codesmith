# Codesmith Evals

Use these as fresh-context evaluation cases before declaring Codesmith stable. The v0.1 skill is the baseline: it handled vocabulary, flow, deep abstractions, documentation, and behavior-focused tests, but did not explicitly govern the restraint/reuse ladder, DI ceremony, security primitives, state cleanup, concurrency, realistic file scale, or premature configuration. Those gaps motivated v0.2.

## Trigger positives

Codesmith should trigger for requests like:

1. “Implement this feature, but keep the code small enough to understand without inventing an architecture around it.”
2. “This works but uses interfaces, factories, providers, and a DI container for three direct function calls. Simplify it.”
3. “Review this PR for readable code and also catch lifecycle/race/failure-path slop.”
4. “The model hand-rolled an encryption helper. Replace it with the right established primitive.”
5. “This service keeps an in-memory set of active jobs and sometimes leaks entries when uploads fail.”
6. “This importer reads a 4GB file into RAM because the fixtures are tiny. Make the code sane.”
7. “We have a web of locks around a shared map and still see duplicate work. Simplify the concurrency design.”
8. “This check-then-create path races under parallel requests.”
9. “Why are there sixteen config flags for behavior nobody has ever varied?”
10. “The code is technically correct, but a human has to chase helpers and generic names through six files to understand it.”

## Trigger negatives / adjacent primary skills

Codesmith should not replace specialist analysis when the primary task is:

1. exploit/threat modeling;
2. profiling a measured performance regression;
3. choosing a distributed-system architecture from requirements;
4. generated/vendor code transformation;
5. throwaway exploration where maintainability is explicitly irrelevant.

It may still complement those tasks when implementation quality is part of the request.

## Application A: unnecessary dependency injection

Input: one implementation of `Clock`, `Mailer`, and `InvoiceRepository`, each wrapped in interface + provider + factory + container registration solely for unit tests.

Expected:

- trace actual variation/lifecycle requirements;
- retain meaningful external boundaries;
- prefer direct imports or explicit function/object parameters where enough;
- remove ceremonial layers without making dependencies invisible;
- keep tests focused on behavior.

Failure: blindly declare all DI bad, or preserve every layer because “testability.”

## Application B: hand-rolled encryption

Input: custom XOR/AES-mode/key-derivation code written to avoid a dependency.

Expected:

- reject custom cryptographic design;
- identify the platform/mature high-level library appropriate to the language and threat model;
- preserve key-management requirements;
- add tests around application behavior, not proof of home-grown crypto.

Failure: optimize for fewer dependencies by retaining bespoke crypto.

## Application C: tracked-state leak

Input: `activeRequests.add(id)` before an awaited operation; `.delete(id)` only on success.

Expected:

- identify ownership/lifetime;
- move cleanup to a structural exceptional path (`finally`/RAII/etc.);
- consider cancellation/timeout;
- test failure cleanup;
- check whether the state can be derived/eliminated entirely.

Failure: rename `activeRequests` nicely and miss the leak.

## Application D: incomplete failure coverage

Input: tests cover successful upload only; storage, metadata write, and notification can each fail.

Expected:

- identify which failures create materially different observable state;
- add focused tests for cleanup/partial durability/idempotency where needed;
- avoid generating a combinatorial matrix with no behavioral distinction.

Failure: chase percentage coverage while leaving the dangerous transitions untested.

## Application E: dev-scale file handling

Input: importer calls `readFile()`/equivalent on an unbounded user-supplied file, splits the whole string, then creates another full copy.

Expected:

- ask whether size is contractually bounded;
- if not, use a native streaming/bounded iteration primitive;
- keep error/close behavior structural;
- avoid inventing a complex pipeline if a standard iterator solves it.

Failure: “premature optimization” defense for obviously unbounded memory use.

## Application F: lock maze

Input: per-object locks + global lock + nested acquisition + awaited network call inside critical section.

Expected:

- name the invariant;
- first try eliminating shared mutable state or using transaction/queue/atomic primitives;
- if locks remain, simplify ownership/order and exceptional release;
- test the race/invariant rather than lock calls.

Failure: add another mutex to patch the observed race.

## Application G: check-then-act race

Input:

```text
if record does not exist:
    create record
```

called concurrently.

Expected:

- recognize the race;
- prefer atomic insert/upsert/unique constraint/compare-and-swap appropriate to the platform;
- handle the losing caller intentionally;
- avoid application-level locking if native atomicity solves it.

## Application H: premature config

Input: a new feature introduces `ENABLE_NEW_HANDLER`, `HANDLER_STRATEGY`, `HANDLER_BATCH_SIZE`, `HANDLER_RETRY_MODE`, and `HANDLER_PARALLELISM` before any second environment or operator requirement exists.

Expected:

- retain only real operational variability;
- use named constants/default native behavior for the rest;
- explain the trigger that would justify promoting a constant to config later.

Failure: preserve knobs because “future flexibility.”

## Application I: naming as reasoning

Input: requirements use `lease`, code proposes `Manager`, `process()`, `state`, and `items`.

Expected:

- identify actual nouns, actions, states, and relationships first;
- allow those names to suggest smaller functions/data shapes;
- avoid merely renaming an unchanged god-object into `LeaseManager`.

Failure: vocabulary polish without design improvement.

## Application J: minimalism vs correctness

Input: shortest implementation omits validation, atomic write behavior, or cleanup at a trust/durability boundary.

Expected:

- reject false minimalism;
- use the shortest *trustworthy* form;
- keep native/stdlib primitives where they make correctness cheaper.

Failure: choose fewer lines over preserving the contract.
