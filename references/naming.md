# Naming as Design

A name is not decoration applied after implementation. It is a small model of the problem.

Before inventing classes, helpers, state, or configuration, name the concepts the code must express:

- **nouns:** the things that exist (`invoice`, `renewal`, `lease`);
- **verbs:** meaningful actions (`settleInvoice`, `renewLease`);
- **states:** distinctions that change behavior (`pending`, `expired`, `onHold`);
- **relationships:** how values connect (`ordersByCustomerId`);
- **units:** where types do not make them safe (`timeoutMs`, `distanceMeters`);
- **effects:** mutations/I/O callers need to anticipate (`archiveExpiredSessions`);
- **invariants:** facts that must remain true (`availableBalance`, not an unexplained `x`).

If the implementation needs a vague name because the concept itself is vague, clarify the concept before adding machinery.

## Names should compress context

The goal is **minimum ambiguity for minimum visual weight**.

A good name lets the reader stop carrying an explanation in working memory. A bad “descriptive” name merely pastes the explanation into forty characters.

Scope supplies context:

```ts
// Too much context repeated locally
const currentAuthenticatedUserAccountIdentificationNumber = user.id;

// Enough
const userId = user.id;
```

Public APIs and long-lived state need more context than a three-line local loop.

## One stable term per concept

Within a bounded context, do not rotate `fetch`, `load`, `retrieve`, and `get` for variety. If the repository gives those words distinct meanings, preserve the distinction.

Likewise, do not blur different states into near-synonyms. `disabled`, `suspended`, and `onHold` should either mean different things or become one term.

Use domain vocabulary for domain concepts and technical vocabulary for technical mechanisms.

## Functions are promises

A function name should help the caller predict the relevant contract:

- predicates: `isEligible`, `hasPermission`, `canRenew`;
- queries: names that honestly signal retrieval/computation;
- commands: action names that honestly signal state change;
- transformations: names that identify the resulting concept.

A `get...` call that charges a card, writes a file, or mutates shared state is dishonest unless the local convention makes that behavior genuinely expected.

Do not encode every internal side effect into a giant name. Expose the effects that materially change how the caller must reason.

## Name states by lifecycle, not container

Tracked state should reveal why it exists and when it stops existing:

```ts
pendingUploads
activeLeases
inFlightRequestsById
recentFailures
```

These names invite the right questions: when is an item removed, what bounds the collection, and who owns cleanup?

`cache`, `map`, `state`, or `items` alone often hide those obligations.

## Avoid semantic junk drawers

Scrutinize names such as:

- `data`, `info`, `thing`, `item`, `obj`;
- `helper`, `utils`;
- `manager`, `processor`, `service`, `handler`;
- `config`, `options`, `context`, `state` when they become miscellaneous bags.

They are acceptable when the surrounding scope supplies a precise meaning. They are a smell when unrelated responsibilities accumulate behind them.

## Avoid implementation leakage

Prefer the caller's stable concept over today's mechanism:

```ts
// Mechanism exposed unnecessarily
loadUserFromRedis()

// Contract survives storage changes
findUserById()
```

Expose the mechanism when it changes semantics, performance, consistency, transactionality, or operational control.

## Rename test

Ask:

1. What would a new maintainer expect this name to mean?
2. Does the implementation honor that expectation?
3. Is missing context already supplied by scope/type/module?
4. Does another term already mean this concept?
5. Does the name reveal a lifecycle/effect the reader otherwise might miss?
6. Could it survive an implementation change that preserves the contract?
