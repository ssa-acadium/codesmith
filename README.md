# Codesmith

**Eloquent code is compressed understanding.**

Codesmith is an Agent Skill for writing, reviewing, and refactoring human-maintained code so that it is clear at a glance, economical in structure, and dependable when the unhappy path arrives.

It teaches an agent to think before it builds: understand the real flow, remove machinery that does not need to exist, choose precise language for the concepts that remain, compose the smallest trustworthy design, and prove the behavior at its important boundaries.

Codesmith is not code golf. Short code that hides lifecycle bugs, races, partial failures, or resource problems is not elegant. The goal is **the fewest trustworthy moving parts that make the program easier for a human to understand and maintain**.

**Current skill version:** `0.2.0`
**License:** MIT

---

## Why Codesmith?

AI can produce working code very quickly. It can also produce:

* an interface, factory, service, container, and configuration switch where a function would do;
* a new helper that already exists three files away;
* hand-rolled infrastructure that the language, platform, database, or an installed dependency already provides;
* tracked state with no clear lifetime or failure cleanup;
* elaborate locking around shared state that never needed to be shared;
* tests that prove the happy path and ignore the failure modes;
* file handling that works on a development fixture and collapses on a realistic input;
* configuration for imagined future requirements;
* technically descriptive names wrapped around a confused design.

Codesmith treats these as design problems, not merely style problems.

Good naming matters because **language helps expose the model**. If an agent can clearly name the nouns, verbs, states, units, effects, and invariants in a problem, it is much more likely to build code whose structure matches the problem. If everything wants to be called `manager`, `handler`, `data`, `state`, or `utils`, the design probably needs more thought.

---

## The Codesmith method

Codesmith works in five movements:

### 1. Comprehend

Read the touched code and trace the real flow before changing it.

Identify:

* the contract;
* the important domain concepts;
* invariants;
* state ownership and lifetime;
* failure modes;
* concurrency boundaries;
* side effects;
* realistic input and data size.

Minimalism without comprehension is just a small wrong answer.

### 2. Subtract

Before adding code, stop at the first solution that genuinely holds:

1. **Need it now?** If not, omit it.
2. **Already in the codebase?** Reuse it.
3. **Language or standard library does it?** Use it.
4. **Platform, framework, or database does it natively?** Use it.
5. **An installed, well-suited dependency does it?** Reuse it.
6. **A direct function, expression, or data structure is enough?** Keep it direct.
7. **Can the value be derived instead of stored?** Derive it.
8. **Only then:** add the minimum custom mechanism.

This is YAGNI with an engineering obligation attached: simpler must still be correct.

### 3. Name

Use names to clarify the design before reaching for abstractions.

Prefer vocabulary that reveals:

* **what** something represents;
* **what** an operation accomplishes;
* **which state** the system is in;
* **which unit** a value uses;
* **which effect** an operation causes;
* **which invariant** a boundary protects.

Use one stable term for one concept. Let scope carry context instead of building sentence-length identifiers.

### 4. Compose

Build modules and functions around meaningful boundaries rather than ceremony.

Codesmith prefers:

* direct composition over automatic dependency-injection architecture;
* derived values over unnecessary stored state;
* deep, useful modules over forests of tiny wrappers;
* native concurrency primitives over homemade locking schemes;
* vetted security and cryptographic implementations over clever custom routines;
* bounded or streaming resource handling when inputs can grow;
* configuration only when genuine variability exists.

An abstraction has to **remove cognitive load**. Moving code behind another name is not enough.

### 5. Prove

Tests should establish behavior where the design can actually fail.

That includes, when relevant:

* error and exception paths;
* cleanup after failure or cancellation;
* retry and idempotency behavior;
* partial writes and durability boundaries;
* races and concurrent access;
* resource lifetime;
* large or realistically sized inputs;
* trust and security boundaries.

Passing the sunny-day example is the beginning of confidence, not the end.

---

## What Codesmith optimizes for

| Instead of                | Codesmith prefers                                                  |
| ------------------------- | ------------------------------------------------------------------ |
| Abstraction by default    | Direct code until indirection earns its place                      |
| New helper by reflex      | Reuse what the codebase already knows                              |
| Custom mechanism          | Stdlib, platform, database, or trusted dependency                  |
| Stored derived values     | Derive from the source of truth                                    |
| Shared mutable state      | Eliminate sharing or use a native coordination primitive           |
| Lock choreography         | One clear invariant and the simplest correct coordination model    |
| Commenting confusing code | First make the code say what it means                              |
| Comment-free dogma        | Explain rationale, hazards, contracts, and non-obvious constraints |
| Tiny methods everywhere   | Boundaries that actually reduce cognitive load                     |
| Happy-path tests          | Behavioral and failure-boundary tests                              |
| “Works at dev scale”      | Resource behavior appropriate to plausible production inputs       |
| Config “for later”        | A constant until real variability exists                           |

---

## What Codesmith is not

Codesmith is **not**:

* a formatter or linter;
* a demand that every function fit on one screen;
* a ban on dependency injection;
* a ban on dependencies;
* a ban on comments;
* a requirement that code literally read like English;
* an excuse to remove validation, security controls, error handling, or accessibility;
* an argument for the fewest possible lines at any cost.

Sometimes the elegant solution is longer because the failure path must be visible. Sometimes the correct abstraction is substantial because it hides real complexity behind a small interface. Codesmith is interested in **earned complexity**.

---

## Installation

Codesmith is a standard Agent Skill: the repository root contains `SKILL.md`, with detailed guidance under `references/`.

Clone the repository into a skills directory supported by your agent runtime.

### Cross-runtime Agent Skills directory

```bash
git clone https://github.com/ssa-acadium/codesmith.git ~/.agents/skills/codesmith
```

### Claude Code

```bash
git clone https://github.com/ssa-acadium/codesmith.git ~/.claude/skills/codesmith
```

If your runtime uses a different skills location, install the repository as a `codesmith` skill folder so that `SKILL.md` remains at the folder root.

---

## Using Codesmith

Codesmith is a reference skill intended to activate when an agent is writing, reviewing, or refactoring human-maintained source code where clarity, compactness, or dependable behavior matters.

Examples of useful requests:

```text
Implement this feature with Codesmith principles.

Review this module for unnecessary state and over-engineering.

Refactor this so a maintainer can understand the main path at a glance.

Find places where we reimplemented something the standard library already provides.

Design the simplest correct concurrency model for this workflow.

Review the failure paths, not just the happy path.

Make this code smaller without making its obligations less visible.
```

A compatible agent can also discover Codesmith from its skill description without the user explicitly naming it.

---

## Repository structure

```text
codesmith/
├── SKILL.md
├── LICENSE
├── README.md
└── references/
    ├── behavior-tests.md
    ├── documentation.md
    ├── evals.md
    ├── foundations.md
    ├── naming.md
    ├── resource-scale.md
    ├── restraint.md
    ├── review-checklist.md
    ├── state-concurrency.md
    └── structure.md
```

`SKILL.md` contains the compact operating doctrine. The references provide deeper guidance only when the current coding task needs it.

---

## Intellectual roots

Codesmith deliberately combines several traditions rather than treating any one of them as doctrine:

* **intention-revealing naming and clean-code practice** — names should expose purpose rather than implementation trivia;
* **Domain-Driven Design** — code should use the language of the domain it models;
* **Behavior-Driven Development** — important behavior should be expressible and testable in terms humans recognize;
* **information hiding and deep modules** — good boundaries hide meaningful complexity instead of fragmenting it;
* **YAGNI and implementation restraint** — code that does not need to exist cannot become a maintenance burden;
* **Ponytail-style minimalism** — look for deletion, reuse, standard-library support, and native capability before inventing machinery.

Codesmith keeps the tensions between these ideas on purpose. “Clean” does not mean microscopic functions. “Self-documenting” does not mean no comments. “Minimal” does not mean fragile.

---

## Contributing

Codesmith should improve from real coding failures, not abstract style debates.

Issues and pull requests are especially useful when they include a concrete case where an agent:

* over-engineered a straightforward change;
* chose an abstraction that made the code harder to understand;
* duplicated existing or native capability;
* introduced unnecessary state or configuration;
* leaked state or resources on a failure path;
* missed a race or concurrency hazard;
* produced inefficient resource handling;
* wrote good-looking code with weak tests;
* simplified code so aggressively that correctness became obscure.

The best contribution is a reproducible example showing what the agent did, what would have been better, and which Codesmith principle should have guided it.

See [`SKILL.md`](SKILL.md) for the active skill and [`references/evals.md`](references/evals.md) for evaluation scenarios.

---

## License

Codesmith is released under the [MIT License](LICENSE).

