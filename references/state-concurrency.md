# State and Concurrency

State is where elegant-looking code most often acquires invisible obligations.

## Prefer derived state

If a value can be cheaply and reliably computed from authoritative data, derive it instead of storing a second truth.

Store state only when persistence, performance, coordination, or history requires it. Then make these explicit:

- owner;
- source of truth;
- lifetime;
- maximum size/cardinality;
- mutation points;
- cleanup/expiry path;
- behavior on failure, retry, timeout, cancellation, and restart.

A tracked set/map/list with no removal or bound is a potential memory leak even if the happy path looks correct.

## Pair acquisition with release

Resources and registrations should have structural cleanup: context managers, `defer`, `finally`, RAII, scoped subscriptions, abort handlers, or the language's equivalent.

Inspect exceptional exits, not just normal returns. A cleanup path that runs only after success is not a cleanup path.

## Concurrency ladder

Prefer, in order:

1. no shared mutable state;
2. immutable values / ownership transfer / message passing;
3. database transactions, atomic operations, queues, or runtime concurrency primitives;
4. one simple lock protecting one named invariant;
5. multiple/fine-grained locks only when measured contention or a hard requirement justifies the added proof burden.

Locks are not architecture. They are one mechanism for protecting a specific invariant.

## Locking discipline

When locks are necessary:

- name the data/invariant each lock protects;
- keep critical sections narrow but complete;
- use a stable global lock order when multiple locks can be acquired;
- ensure release on exceptions/cancellation;
- avoid blocking I/O or `await` while holding a lock unless the design explicitly requires and proves it safe;
- do not add lock layers to compensate for unclear ownership.

## Race-condition scan

Actively look for:

- check-then-act (`if absent: create`);
- read-modify-write lost updates;
- duplicate work after retries;
- two callbacks completing the same operation;
- cleanup racing with use;
- lazy initialization races;
- stale cached state used after mutation;
- file/database time-of-check vs time-of-use;
- shutdown/cancellation while work is registered as active.

Prefer atomic/native operations that collapse the race window rather than adding bespoke coordination around a non-atomic sequence.

## Test the interleaving that matters

Concurrency tests should target the invariant: duplicate creation, double charge, lost update, leaked registration, deadlock, or use-after-cleanup. Avoid tests that merely assert a mutex method was called.
