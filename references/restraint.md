# Restraint: Understand, Then Subtract

Codesmith treats every new moving part as a cost that must earn its place.

## The consideration ladder

Run this after tracing the real flow, not instead of understanding it:

1. **Need:** does the behavior need to exist now? Speculative future need is not a requirement.
2. **Reuse:** does the repository already have the concept, helper, type, policy, or path?
3. **Stdlib:** does the language ship a correct implementation?
4. **Native:** can the OS, browser, framework, runtime, database, or protocol do it directly?
5. **Installed dependency:** does a dependency already in the project solve the actual problem cleanly?
6. **Direct form:** can a function call, expression, query, data structure, or language feature express it without a new abstraction?
7. **Derived form:** can the value be computed from authoritative state instead of tracked separately?
8. **Custom mechanism:** only now write the minimum code that owns a new idea.

If two rungs work, prefer the earlier one unless it hides an important contract or introduces a worse operational cost.

## One clear line, not one line at any cost

A one-liner is good when it is the natural form of the operation. Do not compress branching, cleanup, mutation, or error handling into expression puzzles merely to reduce line count.

The target is low cognitive surface area, not low newline count.

## Abstractions must earn rent

Question:

- interfaces with one implementation;
- factories with one product;
- wrappers that only delegate;
- classes that only hold one function;
- configuration for a value that never varies;
- generic extension points with no current extension;
- adapters between two shapes the codebase already controls.

Keep an abstraction when it establishes a meaningful contract, hides substantial mechanics, owns a lifecycle, isolates an unstable/external boundary, or already has multiple real consumers/implementations.

## Dependencies

Prefer existing capability before adding packages. A new dependency creates upgrade, supply-chain, compatibility, startup, bundle, and maintenance obligations.

Do not reverse this rule for security-sensitive primitives. Cryptography, password hashing, authentication protocols, certificate validation, secure randomness, compression formats, and difficult parsers are common cases where a mature implementation is safer than a “small” custom one.

**Never invent cryptographic algorithms or protocols.** Prefer vetted high-level APIs and platform key stores over assembling primitives yourself.

## Configuration is deferred variability

A config value is justified when something must actually vary by deployment, environment, operator policy, user choice, or tested runtime condition.

Do not create knobs because a value *might* vary someday. Keep a named constant close to its use until the system has a real second value or operational need.

Each config option adds states the program must validate, test, document, migrate, and support.

## Delete before generalize

When a feature or abstraction has one caller and no current variation, first ask whether deletion or inlining leaves the code clearer. Generality is useful after a pattern exists; before that it is prediction encoded as maintenance cost.
