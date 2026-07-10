# Dependency direction

Dependencies flow `app → features → shared`. Enforce this in linting and CI with restricted path zones,
module-boundary rules, or an equivalent architecture test.

The enforcement must reject:

- Any import from `shared` into `features` or `app`.
- Any import from a feature into `app`.
- Any direct import from one feature into another feature.
- Any import into a feature's `internal/` directory from outside that feature.

Application composition may import named files at a feature root. Code inside a feature may use
relative imports within that same feature and public imports from `shared`.

Avoid exemptions by filename or developer. If framework-generated route files must live under
`src/app`, keep them as thin adapters. If a cycle appears, resolve ownership instead of adding a lint
exception: move orchestration upward, pass a callback or value downward, or extract a truly
domain-agnostic primitive.

The manifest boundaries check layer direction and prevent `app` from reaching `internal/`, including in
nested `src` roots. They cannot compare wildcard identities or prove that two matched paths belong to
the same root. A module-aware linter or architecture test must use the pair
`<normalized architecture root, feature>`: allow imports inside feature A only within its root, reject
A → B, and reject same-slug feature imports between applications or workspace packages.

Use resolver-aware enforcement that covers aliases, re-exports, `require`, and dynamic `import()` as
well as ordinary static imports. Run it on source and tests, and run `patterns check --include-tests`.
Cross-package collaboration uses declared package dependencies and public exports rather than direct
source paths. A test-only backdoor still couples modules and teaches future code the wrong import path.
