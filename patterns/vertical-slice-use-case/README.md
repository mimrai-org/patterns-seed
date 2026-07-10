# Vertical slice use cases

Organize a TypeScript backend around independently changeable operations. One operation owns the
complete path from an inbound request to an observable result: transport mapping, input validation,
application handling, any outbound capability contracts or private adapters it needs, and tests. Code
that changes for the same outcome stays close; unrelated operations do not become coupled through
global service layers.

```text
incoming request
      │
      ▼
transport → validation → handler → slice-owned ports → external systems
                              │
                              └──────── observable result
```

The pattern applies vertical slicing at the use-case level. It does not require CQRS infrastructure,
a mediator, a message bus, DDD, event sourcing, or separate read and write databases. A direct
function call is a valid invocation mechanism, and each slice may start as a small transaction script
then gain a richer model only when its behavior demands one. Ports, adapters, and transactions are
also introduced only when the operation's real boundaries and guarantees justify them.

## Use it when

- The backend has several requests, jobs, or message handlers that change for different reasons.
- Changes routinely cross global controller, service, and repository folders only to complete one
  outcome.
- Shared services have broad interfaces or unrelated consumers that make local change risky.
- The team can name operation ownership and enforce isolation in CI.
- Different operations genuinely benefit from different validation, persistence, or modeling choices.

## Avoid it when

- The application is a small, short-lived endpoint set where this navigation cost exceeds the value.
- Most operations are inseparable views of one algorithm or state machine; a cohesive module may be a
  better unit.
- The team cannot yet recognize duplication, misplaced invariants, or transaction boundaries and
  cannot invest in refactoring.
- The required boundary is independent deployment, scaling, or failure isolation; an in-process slice
  does not provide those properties.
- A framework already imposes a simple structure that remains clear and changes are not crossing
  artificial layers.

## Reference shape

```text
src/
├── bootstrap/                       process startup and dependency assembly
├── operations/
│   └── <operation>/                 one request, job, or message outcome
│       ├── contract.ts              technology-neutral input, result, failures
│       ├── validation.ts            parsing and structural input rules
│       ├── handler.ts               application policy and orchestration
│       ├── ports.ts                 narrow outbound capabilities owned here
│       ├── transport.ts             HTTP, queue, CLI, or scheduler mapping
│       ├── adapters/                optional private concrete integrations
│       └── tests/                   behavior and boundary tests for the slice
└── shared/                          proven stable, domain-neutral primitives
```

The filenames communicate roles, not mandatory class shapes. A one-file operation may stay one file;
an operation with no external effect does not need an empty `ports.ts` or `adapters/` directory. Split
files when separate responsibilities become real, and keep the complete behavior under the operation
owner.

Manifest import checks apply when these responsibilities use the named canonical files. An import
graph cannot inspect concern mixing inside one file. If deterministic separation becomes important,
split the roles and register any process-wide adapter path with the repository's architecture checker.

## Core rules

1. One slice owns one observable operation and its technology-neutral input, result, and expected
   failures.
2. A handler does not import its transport, concrete adapters, bootstrap, or a sibling operation.
3. Structural validation happens before handler execution; state-dependent invariants remain with the
   policy that has the current state.
4. Outbound ports describe the smallest capability one handler needs. They do not become global
   repositories or service facades.
5. A state-changing operation owns its consistency decision. It uses one explicit transaction only
   when several consistency-sensitive actions must succeed or fail together, and always defines the
   failure semantics of external effects.
6. Cross-slice workflows receive an explicit coordinating owner and injected capabilities; repeated
   sibling imports are a signal to revisit the boundaries.
7. Only stable, domain-neutral primitives with independent consumers move to `shared`.
8. Manifest globs protect canonical role files under any `operations/<operation>` root. Resolver-aware
   checks compare `<owning operations root, operation>` identities and classify noncanonical files and
   process-wide adapters.
   Concern mixing inside a one-file slice remains a testing and review responsibility.

## Relationship to other patterns

This pattern chooses the **operation** as the unit of change. It does not choose an independent
deployment boundary or require a service-wide layer hierarchy.

- A hexagonal service pattern chooses the boundary between application policy and external
  technology. Its ports-and-adapters discipline can be used inside a slice when that operation needs
  isolation, but a simple slice does not inherit every hexagonal abstraction by default.
- A modular monolith chooses a larger capability boundary with a public API and data ownership. Slices
  may live inside that module as `operations/<operation>`; the module still owns cross-capability APIs,
  registration, and data rules.

Treat the reference trees as alternative nesting choices, not additive folder mandates. Choose one
outer layout, then apply slice cohesion inside its owner. The manifest's role globs recognize canonical
files below any `operations/<operation>` segment, including one nested in a module; an outer module's
public API and data boundaries still need that architecture's own checks.

## SOLID without horizontal ceremony

- Single responsibility means one owner for one outcome and its reasons to change, not one technical
  folder per class type.
- Open/closed behavior comes from adding an operation and wiring it at bootstrap instead of editing a
  central service for every request.
- Interface segregation produces handler-specific ports with only the capabilities that operation
  consumes.
- Dependency inversion protects volatile databases, networks, clocks, and providers when substitution
  or testing value justifies a port; it does not wrap every local function.
- Liskov substitution matters at actual port implementations and contract tests, not through an
  inheritance hierarchy imposed on every handler.

## Costs and failure modes

- Similar code may exist in several slices before a stable abstraction is visible. Premature sharing
  creates stronger coupling than small, intentional duplication.
- Colocation can become a dumping ground if an operation is named after a broad resource rather than a
  concrete outcome. Split by independent changes and behavior, not by file count.
- Direct database access can make tests slow or transaction semantics implicit. Introduce a narrow
  port or slice-local adapter when volatility, substitution, or test control makes the boundary useful.
- A mediator can hide dependencies and turn invocation into string or container magic. Keep handler
  contracts explicit and add dispatch infrastructure only for a measured need.
- Cross-slice calls can recreate a layered service mesh inside one process. Assign the workflow an
  owner, avoid cycles, and reconsider slices that collaborate on nearly every change.
- Local isolation does not create runtime isolation. Slices still share a process, release, resources,
  and failure domain unless a different deployment architecture is chosen.

Start with [the operation anatomy](structure/operation-anatomy.md), then follow the recipe for
[adding an operation](recipes/add-operation.md). Agents should begin with `patterns.yaml` and route
through [AGENTS.md](AGENTS.md). Research provenance and license notes are recorded in
[SOURCES.md](SOURCES.md).

Original content is licensed under [CC BY 4.0](LICENSE).
