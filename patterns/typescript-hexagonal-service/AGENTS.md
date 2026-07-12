# Build a Framework-Independent TypeScript Service — agent router

Read `patterns.yaml` first and select the smallest relevant document from its rich index.

## Route by task

- Placing code or evaluating a dependency: `structure/layers-and-roles.md` and
  `rules/dependency-direction.md`.
- Defining an external capability: `structure/ports-and-adapters.md` and `rules/port-design.md`.
- Translating external data: `rules/model-mapping.md`.
- Wiring dependencies: `rules/composition-root.md`.
- Adding business behavior: `rules/use-case-boundaries.md` and `recipes/add-use-case.md`.
- Supporting another trigger: `recipes/add-inbound-adapter.md`.
- Integrating or replacing external technology: `recipes/add-outbound-adapter.md` or
  `recipes/replace-adapter.md`.
- Choosing test scope: `structure/testing-strategy.md` and `recipes/test-port-contract.md`.
- Reconsidering a consequential trade-off: open the matching file under `adrs/`.

## Non-negotiable constraints

- Keep domain and application code free of adapter and bootstrap imports.
- Define outbound ports from application needs and keep vendor types out of them.
- Translate external models inside adapters.
- Select implementations only in bootstrap.
- Do not add CQRS, event sourcing, generic repositories, or symmetric inbound interfaces unless a use
  case requires them; outbound ports for external capabilities are always required.
- In a monorepo, treat each service or owning package root as part of layer identity and reject direct
  source imports across roots; use declared package exports or protocols for deliberate collaboration.

Before finishing, run typecheck, tests, architecture checks, and `patterns check --include-tests` when
available. Add contract tests when an adapter claims to be replaceable and commit/rollback tests when a
use case claims atomic behavior.
