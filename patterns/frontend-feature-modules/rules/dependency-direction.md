# Dependency direction

Dependencies flow `app → features → shared`. Enforce this in linting and CI with restricted path zones,
module-boundary rules, or an equivalent architecture test.

The enforcement must reject:

- Code in `shared` importing anything from `features` or `app`.
- Code in a feature importing `app`.
- Code in one feature importing another feature.
- Code outside a feature importing that feature's `internal/` directory.

Application composition may import named files at a feature root. Code inside a feature may use
relative imports within that same feature and public imports from `shared`.

Avoid exemptions by filename or developer. If framework-generated route files must live under
`src/app`, keep them as thin adapters. If a cycle appears, resolve ownership instead of adding a lint
exception: move orchestration upward, pass a callback or value downward, or extract a truly
domain-agnostic primitive.

The manifest boundaries check layer direction and prevent `app` from reaching `internal/`, including in
nested `src` roots. The manifest `isolations` rule rejects imports between different feature
identities, but its identity is the feature slug alone; the module-aware check must additionally
qualify identity by architecture root so equal slugs in different applications stay distinct, and must
cover aliases, re-exports, `require`, and dynamic `import()`.

Use resolver-aware enforcement that covers aliases, re-exports, `require`, and dynamic `import()` as
well as ordinary static imports. Run it on source and tests, and run `patterns check --include-tests`.
Cross-package collaboration uses declared package dependencies and public exports rather than direct
source paths. A test-only backdoor still couples modules and teaches future code the wrong import path.
