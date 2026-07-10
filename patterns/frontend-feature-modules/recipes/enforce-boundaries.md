# Enforce import boundaries

Use two complementary checks; the manifest glob model cannot express the whole feature-identity rule.

1. Run `patterns check --include-tests` to enforce the declared `app`, `features`, `shared`, and private
   `internal/` layer edges.
2. Configure a resolver-aware module rule that captures both the normalized architecture root and
   `<feature>` from `<root>/src/features/<feature>/**`. In a workspace, use the owning package or
   application plus its `src` path as `<root>`; a feature name alone is not globally unique.
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

Do not declare `features/** → features/**` as a blanket forbidden glob: it also rejects legitimate
imports within one feature. The module-aware check must compare root-qualified feature identities.
