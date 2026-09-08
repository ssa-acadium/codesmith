# Foundations

Codesmith is a synthesis. Its source traditions are useful precisely because they disagree about what “clean” code means.

## Expressive naming / intention-revealing names

From the Clean Code tradition, keep the demand that names remove guessing about purpose, role, state, units, and effects.

Reject the caricature that more words automatically mean more clarity. Names should compress context, not become prose pasted into identifiers.

## Domain-Driven Design: Ubiquitous Language

Within a bounded context, use the same rigorous vocabulary in requirements, code, APIs, tests, and discussion. Vocabulary is part of the model: inconsistent naming can reveal inconsistent concepts.

Do not impose one enterprise-wide word when two bounded contexts genuinely mean different things.

## Behavior-Driven Development

BDD contributes behavior-shaped specifications and tests. Name scenarios by conditions and observable outcomes; keep assertions attached to the contract rather than private choreography.

Use natural-language structure when it clarifies the model, not as ceremony.

## Self-documenting and literate code

Let source code explain executable behavior through names, types, flow, and boundaries. Let comments preserve rationale, invariants, negative knowledge, external constraints, and warnings that code cannot encode faithfully.

Knuth's human-reader emphasis remains valuable for algorithms and protocols whose reasoning deserves narrative explanation.

## Deep modules / information hiding

Ousterhout supplies an important counterweight to small-function dogma: a module should hide substantial complexity behind a smaller interface. Splitting coherent behavior into shallow wrappers can increase cognitive load even when every function is individually tiny.

Codesmith calls this **semantic leverage**: a boundary earns its place when its name and contract let callers forget details.

## Ponytail: consideration before construction

Ponytail contributes the subtraction reflex: after understanding the real flow, ask whether the feature needs to exist, whether the codebase/stdlib/platform/dependencies already solve it, and whether the direct form is enough before creating custom machinery.

Codesmith adopts the ladder but changes the optimization target. Ponytail intentionally focuses on minimalism; Codesmith asks for the **minimum trustworthy mechanism**. Correct resource lifetime, failure behavior, security, concurrency, and realistic scale cannot be simplified away merely to reduce lines.

## YAGNI and premature generality

Do not encode hypothetical requirements as interfaces, factories, config switches, extension systems, or stored state. Generalize from demonstrated variation rather than predicted variation.

This is not hostility to architecture. It is sequencing: first establish the concept and its real pressures, then introduce the smallest boundary that handles them.

## Native primitives over bespoke mechanisms

Standard libraries, platform features, database transactions/constraints, runtime concurrency primitives, and mature dependencies carry tested semantics that custom code must otherwise recreate.

This becomes a safety rule around cryptography and security protocols: use vetted implementations and high-level APIs rather than inventing algorithms or composing primitives casually.

## Resource and state discipline

Readable happy paths can conceal operational debt. Codesmith therefore treats ownership, lifetime, bounds, cleanup, and concurrency invariants as part of source legibility.

A state variable is not fully named until the design can answer who owns it and when it disappears. A lock is not fully designed until the protected invariant and release behavior are clear.

## Distilled synthesis

Codesmith optimizes seven properties:

1. **Necessity** — build only what the present contract requires.
2. **Semantic alignment** — vocabulary maps cleanly to the implemented concepts.
3. **Cognitive compression** — few moving parts; each abstraction removes more complexity than it adds.
4. **Honest interfaces** — important effects, states, and failure contracts are discoverable.
5. **Lifecycle correctness** — state/resources have owners, bounds, and complete cleanup paths.
6. **Operational sanity** — concurrency and data handling remain reasonable beyond toy inputs.
7. **Behavioral evidence** — tests protect material success and failure behavior.

## Source trail

- Dietrich Gebert, Ponytail: https://github.com/DietrichGebert/ponytail
- Martin Fowler, “Ubiquitous Language”: https://martinfowler.com/bliki/UbiquitousLanguage.html
- Eric Evans, *Domain-Driven Design Reference*: https://www.domainlanguage.com/ddd/reference/
- Dan North, “Introducing BDD”: https://dannorth.net/blog/introducing-bdd/
- Donald Knuth, *Literate Programming*: https://cs.stanford.edu/~knuth/lp.html
- John Ousterhout, *A Philosophy of Software Design*: https://web.stanford.edu/~ouster/cgi-bin/book.php
- OWASP, Cryptographic Storage Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html
- SEI CERT secure coding rules (locking/concurrency): https://wiki.sei.cmu.edu/
