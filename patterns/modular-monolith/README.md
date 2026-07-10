# Modular monolith

Build one deployable backend from strongly encapsulated capability modules. Each module owns its
behavior and data, exposes a small public API, and hides its implementation. A thin host assembles the
modules into each runtime instance.

```text
                 one versioned deployable; all modules per instance
┌──────────────────────────────────────────────────────────────────┐
│ host ──▶ module A public API     module B public API ◀── module A │
│              │                           │                        │
│          private internals           private internals            │
│          module-owned data           module-owned data            │
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

Simple modules may remain flat behind the same public entry point. Internal layering grows only with
real policy and external boundaries.

## Core rules

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

Start with [the system layout](structure/system-and-module-layout.md), then use
[the add-module recipe](recipes/add-module.md). Agents should begin with `patterns.yaml` and route
through [AGENTS.md](AGENTS.md). Research provenance and license notes are recorded in
[SOURCES.md](SOURCES.md).

Original content is licensed under [CC BY 4.0](LICENSE).
