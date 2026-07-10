# Monorepo shared contracts

Share payload shapes, runtime schemas, and TypeScript types across several applications through
explicit workspace packages. Producers and consumers depend on the same capability-sized contract;
they do not import each other:

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

## Core rules

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

Start with [the workspace graph](structure/workspace-package-graph.md), then use the recipe for
[adding a contract package](recipes/add-contract-package.md). Agents should begin with
`patterns.yaml` and follow [AGENTS.md](AGENTS.md). Research provenance and license notes are recorded
in [SOURCES.md](SOURCES.md).

Original content is licensed under [CC BY 4.0](LICENSE).
