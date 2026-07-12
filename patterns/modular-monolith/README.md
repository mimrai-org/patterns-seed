# Structure One Backend with Strong Module Boundaries

> **Known as:** Modular monolith
>
> **Pattern ID:** `modular-monolith` · **Version:** `0.2.0`
>
> **Category:** System architecture · **Target:** Backend application
>
> **Language:** TypeScript · **Framework:** Independent · **Runtime:** Node.js

Divide one TypeScript backend into capability modules that own their public API, internal code, and
data while the system remains one versioned deployable.

## Install

```sh
patterns add mimrai-org/patterns-seed/patterns/modular-monolith#modular-monolith-v0.2.0
```

This installs architecture guidance and checks; it does not create modules or modify application code.

## What it solves

A single backend can keep simple local calls and one deployment without becoming a codebase where
every capability reaches into every other capability. Each module hides its implementation behind a
small public API, owns its data, and is assembled by a thin host.

```text
                 one versioned deployable; all modules per instance
┌──────────────────────────────────────────────────────────────────┐
│ host ──▶ module A public API    module B public API ◀── module A │
│              │                          │                        │
│          private internals          private internals            │
│          module-owned data          module-owned data            │
└──────────────────────────────────────────────────────────────────┘
```

This pattern keeps local calls, straightforward operations, and transaction options while making
ownership explicit. It is not a promise that modules can become services automatically; extraction is
possible only when contracts, data, and runtime assumptions are genuinely independent.

## Use it when

- One product contains several business capabilities that evolve at different rates.
- A single deployment is operationally preferable to distributed services.
- Teams need clear ownership and a limited blast radius inside one codebase.
- Module boundaries can be enforced in CI and, where valuable, in the database.

## Avoid it when

- The backend is small enough that capability boundaries add more navigation than clarity.
- A capability requires independent deployment, scaling, isolation, or regulatory controls now.
- The organization cannot assign ownership or enforce boundaries; directory names alone will not
  prevent a tightly coupled monolith.

## Reference shape

```text
src/
├── host/                         executable composition and lifecycle
├── modules/
│   └── <module>/
│       ├── index.ts             only public entry point
│       ├── contracts/           stable inputs, outputs, facade, integration events
│       ├── register.ts          module composition, re-exported by index.ts
│       └── internal/
│           ├── application/     use cases and outbound contracts
│           ├── domain/          optional policy and invariants
│           └── adapters/        inbound and outbound technology
└── shared/                       small domain-agnostic primitives
```

Simple modules stay flat inside `internal/` behind the same public entry point. Internal layering grows
only with real policy and external boundaries.

## Key rules

1. Every consumer imports a module through `src/modules/<module>/index.ts`.
2. A module never imports another module's `internal`, `contracts`, or `register.ts` paths directly.
3. Each module owns its persistence models, migrations, and writes.
4. Immediate collaboration uses a public module operation; decoupled reactions use versioned
   integration events.
5. Each module factory composes its internals and returns an API plus lifecycle; the host wires module
   APIs in dependency order.
6. `shared` contains stable, domain-agnostic primitives and depends on no module.

## SOLID at module scale

- A module has one cohesive capability and an explicit owner.
- Small public contracts apply interface segregation across module boundaries.
- Internal application policy owns the ports implemented by module adapters.
- Consumers depend on public abstractions rather than private implementations.
- New modules extend the host through registration instead of adding capability switches throughout
  shared infrastructure.

## Costs and failure modes

- All modules share release and scaling decisions, and a faulty module can fail its runtime instance.
- In-process calls make forbidden coupling easy unless imports and data access are checked
  automatically.
- A physically shared database can create contention even when logical ownership is sound.
- A large shared kernel becomes a hidden module with no owner. Prefer duplication or an explicit
  capability owner over cross-module business helpers.
- Events add operational and consistency cost. Do not introduce a broker, outbox, inbox, or eventual
  consistency when a synchronous public call is sufficient.
- In-process events reach only the current runtime instance. Cross-replica or durable delivery needs an
  explicit external mechanism.
- A facade that exposes persistence records or internal entities preserves none of the promised
  isolation.

## Relationships

- [Backend features organized by use case](../vertical-slice-use-case/) can organize the operations
  inside a module without replacing the module's public API or data ownership.
- [A framework-independent TypeScript service](../typescript-hexagonal-service/) can shape the inward
  dependency direction inside one or more modules.
- [Shared type-safe contracts](../monorepo-shared-contracts/) can connect this deployable to other
  workspace applications without exposing private module code.
- [Replace database adapters safely](../swappable-repository-adapters/) can give a module's owned data
  a replaceable persistence seam without leaking storage details past the module API.

A modular monolith does not promise future microservice extraction. Independent deployment is a
separate decision that requires genuinely independent contracts, data, runtime assumptions, and
operations.

## Full guide

- **Structure:** [system and module layout](structure/system-and-module-layout.md),
  [module public API](structure/module-public-api.md),
  [module communication](structure/module-communication.md),
  [data ownership](structure/data-ownership.md),
  [testing and enforcement](structure/testing-and-enforcement.md)
- **Rules:** [module encapsulation](rules/module-encapsulation.md),
  [internal dependency direction](rules/internal-dependency-direction.md),
  [cross-module communication](rules/cross-module-communication.md),
  [data ownership](rules/data-ownership.md), [composition roots](rules/composition-roots.md),
  [shared code](rules/shared-code.md)
- **Recipes:** [add a module](recipes/add-module.md),
  [add a module use case](recipes/add-module-use-case.md),
  [expose a module operation](recipes/expose-module-operation.md),
  [connect modules](recipes/connect-modules.md),
  [add module persistence](recipes/add-module-persistence.md),
  [harden boundaries in CI](recipes/harden-boundaries-in-ci.md)
- **Decisions:** [one deployable with strong modules](adrs/0001-single-deployable-strong-modules.md),
  [public module entry points](adrs/0002-public-module-entry-points.md),
  [module-owned data](adrs/0003-module-owned-data.md),
  [calls and integration events](adrs/0004-sync-calls-and-integration-events.md),
  [enforce architecture in CI](adrs/0005-enforce-architecture-in-ci.md)

Start with [the system layout](structure/system-and-module-layout.md), then use
[the add-module recipe](recipes/add-module.md). Agents should begin with `patterns.yaml` and route
through [AGENTS.md](AGENTS.md). Research provenance and license notes are recorded in
[SOURCES.md](SOURCES.md).

Original content is licensed under [CC BY 4.0](LICENSE).
