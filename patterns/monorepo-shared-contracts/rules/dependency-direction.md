# Dependency direction

Allowed production dependencies point from deployable applications to non-deployable contract
packages:

```text
apps/*  ──▶  packages/contracts-*
```

Enforcement must reject:

- A contract package importing any application.
- One application declaring or importing another application.
- A library package declaring an application dependency.
- A filesystem-relative import that crosses a package root.
- A cycle among contract packages or between contracts and other workspace libraries.
- The same violations from tests, scripts, generated files, dynamic imports, or re-exports.

Every valid internal edge is declared in the importing package's manifest. With pnpm or Bun, use an
explicit `workspace:` range; use the package manager's equivalent local-workspace declaration
elsewhere. Update and commit the lockfile. Never make an edge work by adding a root dependency,
relying on hoisting, or mapping another package's source through TypeScript `paths`.

If application A needs behavior from application B, both depend on a contract and communicate through
the intended runtime boundary. If no runtime boundary is needed, the code may not represent two
applications; reconsider ownership or extract a genuinely reusable library under a separate pattern.

The manifest boundary checks `**/packages/contracts-*/** → **/apps/*/**` for repositories at any
nesting depth. With `patterns check --include-tests`, this includes test files discovered by the
scanner instead of limiting enforcement to `src/`. The checker observes only supported files and
resolved imports; package-manifest edges, unresolved or dynamic imports, and ignored generated output
still require graph-aware and resolver-aware controls.

A from→to glob cannot express the app-to-app rule without also rejecting imports inside one
application, so the manifest declares it as an isolation instead: `within: apps/*/**` captures each
application's identity and rejects any resolved import between two different applications while
allowing same-application imports. Like the boundary rule, it observes only files the scanner
resolves; package-manifest-only edges, unresolved or dynamic imports, and ignored generated output
still require the graph-aware check, which must also classify packages and reject application
dependents wherever they are declared.
