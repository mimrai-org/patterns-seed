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

The manifest intentionally does not declare `apps/* → apps/*`: that glob would also reject valid
imports within one application because it cannot compare wildcard identities. A package-graph rule
must classify application packages and reject edges between different application identities.
