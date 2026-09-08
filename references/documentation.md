# Comments and Documentation

Self-documenting code and useful documentation divide the work; they are not opponents.

## Prefer code for executable meaning

If a comment merely translates syntax, improve the code or remove the comment.

```ts
// Increment retry count
retryCount += 1;
```

The comment adds no information.

## Keep information code cannot faithfully encode

A comment earns its place when it preserves knowledge such as:

- why an apparently simpler approach is unsafe;
- an invariant that spans multiple operations;
- an external API quirk or compatibility constraint;
- a performance or security tradeoff;
- a non-obvious ordering requirement;
- a historical incident that makes a tempting refactor dangerous;
- a reason for a threshold or exception;
- negative knowledge: what was tried and why it failed.

Example:

```ts
// Billing accepts a rate up to 5 minutes old during provider failover.
// Do not reuse this fallback for settlement, which requires a live quote.
return cachedRate({ maxAgeMinutes: 5 });
```

## “Why, not what” is a heuristic

Sometimes a short summary of *what* is useful for a dense algorithm, protocol implementation, generated table, or unfamiliar bit-level operation. The real test is whether the comment gives the reader information faster and more accurately than the code alone.

## Public API documentation is a contract

For exported/public interfaces, document what callers must know that the type/signature does not make clear:

- preconditions;
- postconditions;
- side effects;
- errors/failure states;
- concurrency or ordering guarantees;
- units and valid ranges;
- ownership/lifetime expectations.

Avoid copying implementation details into API docs unless callers must rely on them.

## Put rationale at the right scale

- Local surprising choice -> nearby comment.
- Public contract -> docstring/API documentation.
- Cross-module design tradeoff -> ADR/design note.
- Operational incident/history -> incident record or linked issue.

Do not force architectural history into a ten-line inline comment, and do not bury a one-line safety invariant in a distant design document.

## Maintenance rule

A stale comment is misinformation. When behavior changes, review adjacent rationale and public documentation as part of the same change.
