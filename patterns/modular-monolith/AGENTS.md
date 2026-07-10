# Modular monolith — agent router

Read `patterns.yaml` first and use its rich index to select only the documents needed for the task.

## Route by task

- Choosing a module or host location: `structure/system-and-module-layout.md`.
- Changing a public surface: `structure/module-public-api.md`,
  `rules/module-encapsulation.md`, and `recipes/expose-module-operation.md`.
- Adding internal behavior: `rules/internal-dependency-direction.md` and
  `recipes/add-module-use-case.md`.
- Connecting capabilities: `structure/module-communication.md`,
  `rules/cross-module-communication.md`, and `recipes/connect-modules.md`.
- Adding or accessing data: `structure/data-ownership.md`, `rules/data-ownership.md`, and
  `recipes/add-module-persistence.md`.
- Wiring runtime dependencies: `rules/composition-roots.md`.
- Reusing code across modules: `rules/shared-code.md`.
- Adding enforcement: `structure/testing-and-enforcement.md` and
  `recipes/harden-boundaries-in-ci.md`.
- Challenging a consequential decision: open the matching file under `adrs/`.

## Non-negotiable constraints

- Import modules only through their public `index.ts` entry points.
- Keep module internals and persistence private to their owner.
- Keep modules independent of `host` and keep `shared` independent of both.
- Choose synchronous calls for required immediate results and events for independent reactions.
- Do not add distributed-system mechanisms before their consistency and reliability needs exist.

Before finishing, run typecheck, unit and integration tests, architecture tests, and
`patterns check --include-tests` when available. A successful runtime path is invalid if it crosses a
private module boundary. In a monorepo, module identity includes its deployable root; equal slugs under
different applications never grant source-level access.
