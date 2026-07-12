# Enforce import boundaries

Use two complementary checks; the manifest covers layer edges and slug-level feature isolation, while
root-qualified feature identity needs a module-aware rule.

1. Run `patterns check --include-tests` to enforce the declared layer boundaries, private `internal/`
   edges, and the `isolations` rule that rejects imports between distinct features.
2. Configure a resolver-aware module rule that captures both the normalized architecture root and
   `<feature>` from `<root>/src/features/<feature>/**`. In a workspace, use the owning package or
   application plus its `src` path as `<root>`; a feature name alone is not globally unique. Typical
   implementations: eslint-plugin-boundaries with element types capturing `<root>` and `<feature>`, a
   dependency-cruiser forbidden rule with capture groups, or an architecture test over the resolved
   import graph; plain `no-restricted-imports` suffices only for the `internal/**` rejection.
3. Allow code inside feature A to import A's internals and `shared` only when both paths belong to the
   same architecture root.
4. Reject imports when feature names differ or roots differ, including the same feature slug under two
   applications. Deliberate cross-package use goes through package public exports and a workspace-graph
   rule, not a source-path exception.
5. Allow `app` to import named feature-root files from its own architecture root and reject
   `app → internal/**`.
6. Cover ordinary imports, re-exports, aliases, `require`, dynamic `import()`, and test files.
7. Add positive fixtures for A → A, app → public entry, and feature → shared inside one root.
8. Add negative fixtures for A → B, app → internal, shared → feature, feature → app, and the same
   feature slug in two different roots.
9. Run both checks in CI and update their path classification whenever aliases or source roots change.

Do not declare `features/** → features/**` as a forbidden boundary glob: it also rejects legitimate
imports within one feature. Declare it as an `isolations` rule instead (`within: "src/features/*/**"`),
which compares captured feature identities. The module-aware check remains necessary for
root-qualified identity: the manifest isolation treats equal slugs in different roots as the same
feature.
