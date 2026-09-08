# Structure, Composition, and Boundaries

Good structure keeps the policy visible while mechanics stay behind boundaries that actually reduce cognitive load.

## Make the main path readable at a glance

At the level where the reader decides *what happens*, show the meaningful decisions. Keep incidental parsing, mapping, I/O details, and protocol mechanics below that level when a useful boundary exists.

Prefer direct sequence and early exits when they reduce nesting. Do not compress branching into clever expressions that require decoding.

## Use functions directly until indirection earns a purpose

A direct function call is often the cleanest dependency relationship.

Before introducing an interface, provider, factory, service locator, dependency-injection container, or adapter, ask what current problem the indirection solves.

Indirection is justified when it provides something concrete, such as:

- multiple real implementations selected at runtime/build time;
- lifecycle/resource ownership;
- isolation of an external or unstable boundary;
- plugin/extensibility contract that exists now;
- transaction/security boundary;
- a test seam that cannot be achieved more simply with function/module injection.

For ordinary testability, passing a function/object explicitly is frequently enough:

```ts
async function sendReceipt(order, sendEmail = email.sendReceipt) {
  // ...
  await sendEmail(order);
}
```

Do not build a container graph merely to avoid calling a function.

## Extract for semantic leverage

Extract when at least one is true:

- the block has a useful domain/technical name;
- callers should be able to forget its mechanics;
- it creates a genuine reuse or test boundary;
- it isolates an effect, policy, protocol, or lifecycle;
- it materially simplifies the surrounding control flow.

Do not extract because of arbitrary line-count rules.

## Inline shallow indirection

Consider inlining a helper/module when it:

- merely renames an obvious operation;
- forwards all arguments unchanged;
- has one caller and no independent contract;
- forces a file jump without hiding meaningful complexity;
- exists only to satisfy a pattern or imagined future implementation.

Modularity is about coherent boundaries, not maximum file/function count.

## Prefer deep modules

A good module exposes a smaller conceptual surface than the complexity it owns. Keep protocol rules, file-format knowledge, pricing logic, or state-transition rules together instead of scattering the same knowledge across callers.

If several modules must understand the same internal detail, the boundary may be leaking.

## Keep effects discoverable

Readers should be able to discover meaningful effects without tracing the entire call graph:

- persistence writes;
- network calls;
- shared-state mutation;
- filesystem changes;
- retries/timeouts;
- transactions;
- clock/randomness dependence;
- emitted messages/events.

Not every effect belongs in a function name. The interface and structure simply must not lie about them.

## Example

Before:

```ts
async function process(a: Account, n: number) {
  const x = await subscriptions.find(a.id);
  if (!x) return false;
  if (n > 30) {
    await subscriptions.update(x.id, { status: "hold" });
    await email.send(a.email, "Past due");
    return false;
  }
  return true;
}
```

After:

```ts
const BILLING_GRACE_PERIOD_DAYS = 30;

async function enforceRenewalPolicy(
  account: Account,
  daysPastDue: number,
): Promise<RenewalStatus> {
  const subscription = await subscriptions.findByAccountId(account.id);
  if (!subscription) return "not-subscribed";
  if (daysPastDue <= BILLING_GRACE_PERIOD_DAYS) return "eligible";

  await subscriptions.placeBillingHold(subscription.id);
  await renewalNotices.sendPastDueNotice(account.email);
  return "on-hold";
}
```

The second version is better because it removes hidden meaning, not because it uses more words.
