# Build a Framework-Independent TypeScript Service

> **Known as:** Hexagonal architecture · Ports and adapters
>
> **Pattern ID:** `typescript-hexagonal-service` · **Version:** `0.2.0`
>
> **Category:** Service architecture · **Target:** Backend service
>
> **Language:** TypeScript · **Framework:** Independent · **Runtime:** Node.js

Keep TypeScript business logic independent from HTTP frameworks, databases, queues, and external
providers. External technology translates into or implements contracts owned by the application.

## Install

```sh
patterns add mimrai-org/patterns-seed/patterns/typescript-hexagonal-service#typescript-hexagonal-service-v0.2.0
```

This installs architecture guidance and checks; it does not generate or overwrite service code.

## What it solves

When business behavior imports framework controllers, ORM models, queue clients, or vendor SDKs, a
technology change reaches into the center of the service. This pattern keeps that policy on the inside:
inbound adapters translate external triggers into use-case calls, outbound adapters implement the
capabilities the use cases require, and bootstrap chooses the concrete implementations.

```text
                    compile-time dependencies

 bootstrap  ──▶  adapters  ──▶  application  ──▶  domain
                    │                 │
 inbound trigger ───┘                 └── port ──▶ outbound effect
                         runtime calls
```

The architecture separates inside from outside; it does not require a fixed number of classes. CQRS,
domain events, aggregates, an ORM, and a dependency-injection framework are optional decisions, not
parts of this pattern.

## Use it when

- Business rules must outlive the current HTTP framework, persistence tool, or provider.
- The service has several external integrations or more than one delivery mechanism.
- Important use cases need fast isolated tests.
- Replacement cost at an external boundary is high enough to justify a contract and mapper.

## Avoid it when

- The service is small CRUD with little policy and one stable integration.
- The proposed ports merely repeat a library API without expressing an application need.
- The team would create one interface and implementation for every function without a real boundary.

## Reference shape

```text
src/
├── domain/                  pure business concepts and invariants
├── application/
│   ├── use-cases/           application outcomes and orchestration
│   └── ports/               outbound capabilities required by use cases
├── adapters/
│   ├── inbound/             HTTP, message, schedule, CLI, or test drivers
│   └── outbound/            persistence, messaging, clock, identity, or provider drivers
└── bootstrap/               configuration, lifecycle, and dependency composition
```

Keep a small use case flat. Add subdirectories only when several files share a coherent role.

## Key rules

1. Domain code depends only on domain code and deliberately accepted deterministic, side-effect-free
   libraries; it has no framework or external-effect dependency.
2. Application code depends on domain code and application-owned ports.
3. Adapters depend inward; the core never imports an adapter.
4. External models are mapped at the adapter boundary.
5. Bootstrap is the only place that chooses concrete implementations.
6. Ports are narrow capabilities shaped by use cases, not generic technology mirrors.
7. In a monorepo, each service root is a separate architecture owner. Cross-root collaboration uses
   declared package exports or protocols, never another service's source paths.

## SOLID at the boundary

- Dependency inversion lets application policy own the contracts implemented by infrastructure.
- Interface segregation keeps ports focused on what one group of use cases actually needs.
- Single responsibility separates policy, translation, external effects, and assembly.
- Substitutability is demonstrated by shared port-contract tests, not assumed from matching TypeScript
  types.
- Adapter replacement extends the system without editing core policy when the required capability has
  not changed.

## Costs and failure modes

- Ports, mappers, and dependency wiring add navigation and maintenance. Spend that cost only at
  meaningful volatility boundaries.
- TypeScript interfaces do not exist at runtime. Constructor injection, tokens, or factories must be
  resolved explicitly in bootstrap.
- A generic repository with every possible method couples use cases to accidental infrastructure
  capabilities. Prefer use-case-shaped ports.
- Leaking ORM entities or HTTP DTOs through a port makes the dependency arrow point inward only on a
  diagram, not in the code.
- Transactions that span several outbound ports need an explicit unit-of-work policy; hiding them in
  one adapter can make application behavior misleading. Separately injected adapters are not atomic
  merely because one use case called them.

## Relationships

- [Backend features organized by use case](../vertical-slice-use-case/) chooses the operation as the
  unit of change. A slice can use ports and adapters where an external boundary justifies them.
- [One backend with strong module boundaries](../modular-monolith/) chooses a larger capability and
  deployment boundary. Individual modules can apply this pattern internally.
- [Replace database adapters safely](../swappable-repository-adapters/) specializes the outbound
  adapter idea for persistence, behavioral conformance, and reversible migration.

These relationships are optional composition choices, not a requirement to introduce every layer in
every use case.

## Full guide

- **Structure:** [layers and roles](structure/layers-and-roles.md),
  [ports and adapters](structure/ports-and-adapters.md),
  [dependency and runtime flow](structure/dependency-and-runtime-flow.md),
  [testing strategy](structure/testing-strategy.md)
- **Rules:** [dependency direction](rules/dependency-direction.md),
  [use-case boundaries](rules/use-case-boundaries.md), [port design](rules/port-design.md),
  [model mapping](rules/model-mapping.md), [composition root](rules/composition-root.md)
- **Recipes:** [add a use case](recipes/add-use-case.md),
  [add an inbound adapter](recipes/add-inbound-adapter.md),
  [add an outbound adapter](recipes/add-outbound-adapter.md),
  [replace an adapter](recipes/replace-adapter.md),
  [test a port contract](recipes/test-port-contract.md)
- **Decisions:** [separate inside from outside](adrs/0001-separate-inside-from-outside.md),
  [application-owned outbound ports](adrs/0002-application-owns-outbound-ports.md),
  [explicit composition root](adrs/0003-use-explicit-composition-root.md),
  [use-case-shaped ports](adrs/0004-shape-ports-around-use-cases.md)

Start with [layers and roles](structure/layers-and-roles.md), then follow
[the add-use-case recipe](recipes/add-use-case.md). Agents should begin with `patterns.yaml` and use
[AGENTS.md](AGENTS.md) as the router. Research provenance and license notes are recorded in
[SOURCES.md](SOURCES.md).

Original content is licensed under [CC BY 4.0](LICENSE).
