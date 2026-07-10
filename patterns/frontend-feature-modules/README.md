# Frontend feature modules

Organize a growing React codebase around user-facing capabilities. Each feature owns its UI, remote
operations, local state, types, and tests; the app layer composes features; shared holds only
domain-agnostic primitives. Dependencies point in one direction:

```text
app  ──▶  features  ──▶  shared
 │                         ▲
 └─────────────────────────┘
```

The pattern is intentionally smaller than Feature-Sliced Design. It introduces three layers and one
hard isolation rule, leaving teams free to add internal feature segments only when the code needs
them.

## Use it when

- A React application has several independently changing capabilities.
- Technical folders such as global `components/`, `hooks/`, and `services/` obscure ownership.
- Changes routinely touch unrelated areas because feature internals are imported directly.
- The team can enforce import rules in linting and CI.

## Avoid it when

- The application is a short-lived prototype or has only one small flow.
- Most UI is a thin projection over one tightly coupled data model and feature boundaries would be
  artificial.
- The real need is a framework-level package boundary across independently deployed applications; use
  workspace packages or microfrontends instead.

## Reference shape

```text
src/
├── app/                    composition, routes, providers, global initialization
├── features/
│   └── <feature>/          one user-facing capability
│       ├── screen.tsx      example of a small named public entry
│       ├── contract.ts     optional stable public types
│       └── internal/
│           ├── api/        capability-owned remote operations
│           ├── ui/         capability-owned UI
│           ├── model/      state and pure capability rules
│           └── lib/        private helpers
└── shared/                 domain-agnostic UI, platform wrappers, and utilities
```

These segments are a menu, not a checklist. Empty directories and one-file abstractions add navigation
cost without clarifying ownership.

## Core rules

1. `app` may import feature public APIs and shared primitives.
2. A feature may import shared primitives, never `app` or another feature.
3. Cross-feature behavior is composed in `app` through props, callbacks, routes, or orchestration.
4. Consumers import small named files from a feature root and never its `internal/` paths.
5. Code enters `shared` only with a domain-neutral contract and an explicit cross-feature owner.
6. Manifest boundaries and module-aware import checks run in CI; directory conventions alone are not
   an architecture control.

## SOLID without ceremony

- Single responsibility means one owner and one reason to change, not one function per file.
- Dependency inversion appears at volatile browser or vendor boundaries, not around every component.
- Interface segregation favors small feature public APIs over a global frontend service layer.
- Open/closed behavior comes from composing features and shared primitives rather than editing a
  central switch for every new capability.

## Costs and failure modes

- Some shared primitives are discovered only after duplication. That is cheaper than a speculative
  shared layer that accumulates hidden business coupling.
- Strict isolation moves cross-feature orchestration into `app`; if that layer starts implementing
  business rules, the feature boundaries are wrong or a new capability is missing.
- Public entry files can multiply without a coherent contract. Keep them few, name them by supported
  capability, and avoid a catch-all barrel that re-exports private implementation.
- A feature that grows without internal cohesion may actually contain several capabilities. Split by
  reasons to change, not file count.

Start with [the layer model](structure/layers.md), then use the recipe for
[adding a feature](recipes/add-feature.md). Agents should begin with `patterns.yaml` and follow
[AGENTS.md](AGENTS.md). Research provenance and license notes are recorded in
[SOURCES.md](SOURCES.md).

Original content is licensed under [CC BY 4.0](LICENSE).
