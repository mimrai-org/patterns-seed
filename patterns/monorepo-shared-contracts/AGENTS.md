# Share Type-Safe Contracts Across Monorepo Apps — agent router

Read `patterns.yaml` first. Use its summaries to open only the document required by the current task.

## Route by task

- Choosing package placement or dependency direction: `structure/workspace-package-graph.md` and
  `rules/dependency-direction.md`.
- Deciding what belongs in a contract package: `structure/contract-package-anatomy.md` and
  `rules/contract-content.md`.
- Choosing authored schemas, generated artifacts, or TypeScript types:
  `structure/source-of-truth-and-runtime-flow.md` and `rules/runtime-validation.md`.
- Adding a package or entry: `recipes/add-contract-package.md` or `recipes/add-contract.md`.
- Connecting an application: `recipes/consume-contract.md` and `rules/package-public-apis.md`.
- Changing a deployed shape: `rules/compatibility-and-ownership.md` and
  `recipes/evolve-contract.md`.
- Splitting an over-broad package: `recipes/split-contract-package.md`.
- Installing architecture checks: `rules/graph-enforcement.md` and
  `recipes/enforce-boundaries-in-ci.md`.
- Challenging a structural decision: open the relevant file under `adrs/`.

## Non-negotiable constraints

- Preserve `applications → contract packages`; never reverse it.
- Never add an application-to-application dependency or source-relative cross-package import.
- Share boundary vocabulary and validation, not business workflows or infrastructure clients.
- Parse untrusted data at the boundary with the contract's runtime source of truth.
- Add every internal dependency to the consuming package manifest and expose only declared exports.
- Treat independent deployment as a compatibility problem even when compilation is atomic.

Before finishing, run the package manager's frozen install check, contract package tests, the
cross-version reader matrix, affected consumer typechecks/builds, the workspace graph policy, cycle
detection, resolver-aware import rules, and `patterns check --include-tests` when available. The
manifest boundary catches resolved imports from scanned contract-package code, including discovered
tests, into scanned application code. A graph-aware check must additionally compare package identities
and inspect dependency manifests, unresolved or dynamic imports, ignored generated output, and other
files the import scanner cannot prove. It rejects app-to-app edges, contract cycles, undeclared
dependencies, deep imports, and violations outside the manifest check's observable graph.
