# Replace Database Adapters Safely — agent router

Read `patterns.yaml` first and open only the smallest indexed document needed for the task.

## Route by task

- Deciding whether and where the seam belongs: `structure/capability-and-adapter-shape.md` and
  `rules/port-semantics.md`.
- Checking imports or composition: `structure/runtime-and-dependency-flow.md` and
  `rules/adapter-isolation.md`.
- Defining results, failures, queries, or concurrency: `rules/port-semantics.md` and
  `rules/consistency-and-transactions.md`.
- Translating records or changing a schema: `rules/model-mapping.md`.
- Adding a storage technology: `recipes/add-a-repository-adapter.md`.
- Establishing substitutability: `structure/contract-and-conformance.md`,
  `rules/contract-testing.md`, and `recipes/build-the-contract-suite.md`.
- Planning a replacement: `structure/migration-lifecycle.md`, `rules/cutover-and-authority.md`, and
  `recipes/migrate-and-cut-over.md`.
- Responding to a failed cutover: `recipes/roll-back-an-adapter.md`.
- Removing the previous implementation: `recipes/retire-an-adapter.md`.
- Choosing test scope: `structure/testing-layers.md`.
- Reconsidering a consequential trade-off: open the matching file under `adrs/`.

## Non-negotiable constraints

- Keep the repository port narrow, capability-specific, and owned by its consumers.
- Do not expose storage records, query builders, clients, transactions, or vendor errors through the port.
- Keep every concrete adapter independent; an adapter must not import or delegate to a sibling adapter.
- Select the adapter in bootstrap, never inside a use case or through a service locator.
- Run one behavioral contract suite against each claimed substitute and representative backing resource.
- Do not call two repository adapters sequentially and label that operation safe dual write.
- For an online migration, establish durable capture and record `W0` before backfill; replay creates,
  updates, and tombstones without allowing older backfill state to overwrite newer source state.
- Fence the current authority, record `Wf`, and switch only after the candidate has applied exactly through
  `Wf` with no gaps or poison records. Never substitute a nonzero lag threshold for this gate.
- Name the authoritative writer, synchronization direction, verification gate, and rollback data source for
  every migration stage.
- The manifest covers root role paths and `src/modules/<capability>/...` with one capability segment. Add a
  project-owned architecture or restricted-import rule for other layouts, root-qualify adapter identities
  in a monorepo, and restrict direct ORM or driver imports.

Before finishing, run typecheck, unit tests, contract tests, adapter integration tests, migration tests,
architecture checks, and `patterns check --include-tests` when available. If a guarantee cannot be
observed consistently across adapters, narrow or split the port rather than weakening the test.
