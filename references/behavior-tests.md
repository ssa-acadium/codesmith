# Contract and Failure Tests

Readable code still needs evidence. Tests should protect the observable contract and the obligations introduced by state, resources, concurrency, and external failure.

## Name behavior, not implementation

Prefer:

```ts
it("places subscriptions on hold after the billing grace period", ...)
it("releases the upload slot when storage fails", ...)
it("creates only one lease when two requests race", ...)
```

Over:

```ts
it("tests enforceRenewalPolicy", ...)
it("calls cleanup once", ...)
it("uses mutex", ...)
```

A sentence-shaped name is useful only when the assertions also protect behavior.

## Test the contract, not choreography

Prefer observable outputs, persisted state, emitted domain events, resource ownership, and externally visible failures. Assert internal call counts only when that interaction itself is the contract.

A refactor that preserves behavior should not usually require rewriting the behavioral suite.

## Cover the obligations the design creates

Do not mechanically generate every category below. Select the ones the feature actually has:

| Obligation | Typical test |
|---|---|
| Boundary/threshold | below / at / above limit |
| Invalid/absent state | reject or return defined result |
| External failure | dependency errors propagate/translate correctly |
| Resource lifetime | resource/registration released on failure |
| Retry/idempotency | duplicate attempt does not duplicate effect |
| Timeout/cancellation | state is cleaned and operation stops safely |
| Concurrency | invariant survives competing operations |
| Persistence | partial failure does not leave invalid durable state |
| Large input | bounded-memory/streaming path remains valid |

The common AI failure is happy-path confidence: code passes the obvious example while the failure path leaks state, double-applies an effect, or leaves a half-written resource.

## Failure-path tests are first-class

When code mutates tracked state before an operation that can fail, add a test that makes the operation fail and checks cleanup.

When code coordinates concurrent work, test the invariant under overlapping calls rather than merely testing sequential calls twice.

When code writes durable state, test the relevant partial-failure behavior if corruption or inconsistency is possible.

## Use domain vocabulary

Tests are executable examples of the model. Requirements, production code, and tests should use the same terms unless they intentionally represent different bounded contexts.

Vocabulary drift is often evidence that the model itself is drifting.

## Given/When/Then selectively

Use Given/When/Then when setup, action, and outcome are meaningful to the scenario. Do not wrap tiny pure functions in ceremony just to resemble BDD.
