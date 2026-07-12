# Organize Backend Operations by Use Case — agent router

Read `patterns.yaml` first. Use its summaries to open only the document required by the current task.

## Route by task

- Placing code or deciding what belongs in a slice: `structure/operation-anatomy.md` and
  `rules/slice-cohesion-and-ownership.md`.
- Adding a request, job, or message outcome: `recipes/add-operation.md`.
- Changing imports or runtime wiring: `structure/dependency-and-runtime-flow.md` and
  `rules/dependency-direction.md`.
- Parsing or validating input: `rules/input-validation.md`.
- Adding a database, provider, clock, or other external dependency: `rules/port-design.md`.
- Defining atomic writes or reliable effects: `rules/transaction-boundaries.md` and
  `recipes/add-transaction.md`.
- Coordinating multiple operations: `structure/cross-slice-composition.md` and
  `recipes/compose-operations.md`.
- Combining operation slices with a hexagonal service or modular monolith: the “Relationships”
  section in `README.md` and `structure/cross-slice-composition.md`.
- Sharing repeated code: `rules/shared-primitives.md` and `recipes/promote-shared-primitive.md`.
- Choosing direct calls, commands, queries, or a dispatcher: `rules/invocation-and-cqrs.md` and
  `recipes/introduce-dispatcher.md`.
- Adding tests: `structure/testing-strategy.md`.
- Installing or changing architecture checks: `recipes/enforce-boundaries.md`.
- Challenging a consequential choice: open the relevant file under `adrs/`.

## Non-negotiable constraints

- Keep one observable outcome and its delivery-to-effect path under one operation owner.
- Do not import sibling operation implementations. Inject explicit capabilities into a coordinating
  slice or revise the ownership boundary.
- Keep handlers independent from transport, concrete adapters, and bootstrap.
- Parse untrusted input at the edge; do not mistake structural validation for business invariants.
- Make the state-changing use case own its consistency decision. Add one transaction only when its
  atomicity requirement needs one; never hide independent commits inside repository calls.
- Keep external effects and their retry, idempotency, and success-or-commit semantics explicit.
- Use direct handler invocation by default. A mediator or bus is infrastructure, not the architecture.
- Promote only proven stable, domain-neutral primitives to `shared`.
- Keep bootstrap limited to construction and wiring. Contract translation or workflow policy belongs
  in a workflow-owned adapter at an explicit outer composition edge.
- Keep boundaries honest: the manifest's isolation rule enforces sibling-slice isolation by operation
  name and its globs protect named role files below any `operations/<operation>` root. Its blind spot
  is equal operation slugs under two different `operations/` roots — those are different slices but
  capture the same identity. Retain resolver-aware checks for that case, aliases, re-exports, dynamic
  imports, and mixed concerns inside one file; see `recipes/enforce-boundaries.md`.

Before finishing, run the repository's typecheck, behavior tests, adapter integration tests,
resolver-aware dependency checks, cycle detection, and `patterns check --include-tests` when
available. Include test files in slice-isolation checks so helpers cannot bypass the architecture.
