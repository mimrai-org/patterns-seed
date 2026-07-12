# Share Type-Safe Contracts Across Monorepo Apps

> **Known as:** Shared contract packages · Shared data contracts · TypeScript contract packages · Shared types package
>
> **Pattern ID:** `monorepo-shared-contracts` · **Version:** `0.2.0`
>
> **Category:** Contracts · **Target:** Multi-application TypeScript workspace
>
> **Language:** TypeScript · **Framework:** Independent
>
> **Runtimes:** Agnostic — any JavaScript runtime

Share validated TypeScript data contracts across workspace apps without coupling their business code
or release timing.

## Install

```sh
patterns add mimrai-org/patterns-seed/patterns/monorepo-shared-contracts#monorepo-shared-contracts-v0.2.0
```

This installs architecture guidance and checks; it does not create packages or change workspace code.

## What it solves

When a browser app, service, worker, CLI, or mobile app defines the same payload independently, the
copies drift. Importing one application from another avoids duplication by creating a worse coupling.
This pattern puts shared boundary vocabulary and runtime validation in an owned workspace package:

```text
apps/<producer>  ──▶  packages/contracts-<capability>  ◀──  apps/<consumer>
                                      ▲
apps/<worker>  ───────────────────────┘
```

The package is a boundary vocabulary, not a home for reusable business behavior. Each application
keeps its workflows, persistence, framework adapters, and deployment concerns behind the contract.

## Use it when

- At least two applications exchange or independently interpret the same data.
- Duplicate interfaces or validators drift between a service, browser app, worker, CLI, or mobile app.
- The repository already uses package-manager workspaces and can enforce a package dependency graph.
- Applications may deploy separately, so compatibility needs more than a synchronized source commit.

## Avoid it when

- One small application is the only owner and consumer; keep the shape local until sharing is real.
- The proposed package exists only to deduplicate a helper or business workflow. That is a different
  ownership decision, not a contract.
- Consumers live outside the repository or use several languages. Prefer a published protocol
  artifact and generated clients, while retaining the dependency rules from this pattern.
- Teams cannot identify who owns compatibility decisions. A shared package without an owner becomes a
  negotiation bottleneck.

## Reference shape

```text
apps/
├── <producer>/
│   ├── package.json
│   └── src/
├── <consumer>/
│   ├── package.json
│   └── src/
└── <worker>/
    ├── package.json
    └── src/
packages/
└── contracts-<capability>/
    ├── package.json        package identity, explicit exports, direct dependencies
    ├── tsconfig.json       package-local compilation policy when TypeScript needs one
    ├── src/
    │   ├── <operation>.ts  request, result, and validation for one supported operation
    │   ├── <event>.ts      one externally observed message contract
    │   └── <value>.ts      shared wire-level value used by those entries
    └── tests/              parsing and compatibility fixtures
```

Use one contract package per cohesive capability or compatibility boundary. Do not create a package
per interface, and do not collapse unrelated contracts into `shared-types`. Export deliberate
subpaths so consumers depend on the smallest stable surface.

## Key rules

1. Every cross-application contract is owned by a named workspace package.
2. Applications declare that package as a dependency and import only its public exports.
3. Contract packages never import applications; applications never import other applications.
4. Contract packages contain data shape, semantic constraints, parsing, and serialization metadata,
   not use cases, database entities, network clients, framework objects, or provider configuration.
5. A runtime schema is authoritative when data crosses a process, network, queue, file, or storage
   boundary; TypeScript types are derived or generated from that same source.
6. Workspace graph, deep-import, and cycle checks include tests; compatibility CI executes both the
   candidate reader against old payloads and the previous supported reader against candidate output.

## SOLID without a shared-code maze

- Single responsibility means a package changes with one contract family and one accountable owner.
- Interface segregation favors focused export subpaths over one workspace-wide barrel.
- Dependency inversion lets producers and consumers depend on stable boundary vocabulary rather than
  another application's implementation.
- Open/closed evolution comes from additive fields or parallel versions, not conditionals scattered
  through every consumer.

## Costs and failure modes

- A shared package increases coordination. Keep a contract local until a second real consumer exists.
- One repository commit does not imply one deployment moment. A breaking change can still fail an old
  consumer after the producer deploys.
- Runtime schemas add bundle weight and execution cost. Export narrow entry points and measure clients
  with constrained runtimes.
- Too many tiny packages make ownership and task graphs noisy; one broad package recreates hidden
  coupling. Split on owner, semantics, and compatibility cadence rather than file count.
- Source-exported TypeScript is simple but requires every consumer toolchain to transpile it. Compiled
  or generated artifacts add build steps but support more consumers.
- TypeScript project references can improve large builds, but a manually duplicated reference graph
  can drift from `package.json`. Treat workspace dependencies as the architectural truth.

## Relationships

- [A React app organized by feature](../frontend-feature-modules/) can consume contract packages while
  keeping feature behavior, UI, and framework adapters local.
- [A framework-independent TypeScript service](../typescript-hexagonal-service/) can translate shared
  wire contracts at an inbound or outbound adapter boundary.
- [One backend with strong module boundaries](../modular-monolith/) can expose contracts to other apps
  without publishing its private module models or workflows.

This pattern is for applications in the same TypeScript workspace. Multi-language or external
consumers usually need a published protocol artifact and generated clients instead.

## Full guide

- **Structure:** [workspace package graph](structure/workspace-package-graph.md),
  [contract package anatomy](structure/contract-package-anatomy.md),
  [source of truth and runtime flow](structure/source-of-truth-and-runtime-flow.md),
  [testing and release model](structure/testing-and-release-model.md)
- **Rules:** [dependency direction](rules/dependency-direction.md),
  [contract content](rules/contract-content.md),
  [package public APIs](rules/package-public-apis.md),
  [runtime validation](rules/runtime-validation.md),
  [compatibility and ownership](rules/compatibility-and-ownership.md),
  [graph enforcement](rules/graph-enforcement.md)
- **Recipes:** [add a contract package](recipes/add-contract-package.md),
  [add a contract](recipes/add-contract.md), [consume a contract](recipes/consume-contract.md),
  [evolve a contract](recipes/evolve-contract.md),
  [split a contract package](recipes/split-contract-package.md),
  [enforce boundaries in CI](recipes/enforce-boundaries-in-ci.md)
- **Decisions:** [use explicit contract packages](adrs/0001-use-explicit-contract-packages.md),
  [share contracts, not business implementation](adrs/0002-share-contracts-not-business-implementation.md),
  [validate untrusted values at runtime](adrs/0003-validate-untrusted-values-at-runtime.md),
  [keep applications free of dependents](adrs/0004-keep-applications-free-of-dependents.md),
  [select compilation by consumer needs](adrs/0005-select-compilation-by-consumer-needs.md)

Start with [the workspace graph](structure/workspace-package-graph.md), then use the recipe for
[adding a contract package](recipes/add-contract-package.md). Agents should begin with
`patterns.yaml` and follow [AGENTS.md](AGENTS.md). Research provenance and license notes are recorded
in [SOURCES.md](SOURCES.md).

Original content is licensed under [CC BY 4.0](LICENSE).
