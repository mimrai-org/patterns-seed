# TypeScript hexagonal service

Keep application policy independent from delivery protocols, databases, queues, and vendors. Inbound
adapters translate external triggers into use-case calls. The application core declares the outbound
capabilities it needs as ports. Outbound adapters implement those ports, and a bootstrap composition
root selects the concrete implementations.

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

## Core rules

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

Start with [layers and roles](structure/layers-and-roles.md), then follow
[the add-use-case recipe](recipes/add-use-case.md). Agents should begin with `patterns.yaml` and use
[AGENTS.md](AGENTS.md) as the router. Research provenance and license notes are recorded in
[SOURCES.md](SOURCES.md).

Original content is licensed under [CC BY 4.0](LICENSE).
