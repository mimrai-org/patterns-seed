# Organize a React App by Feature — agent router

Read `patterns.yaml` first. Use its summaries to open only the document required by the current task.

## Route by task

- Placing a file or choosing a layer: `structure/layers.md` and
  `rules/ownership-and-placement.md`.
- Organizing code inside one capability: `structure/feature-anatomy.md`.
- Adding or changing imports: `rules/dependency-direction.md` and
  `structure/public-apis.md`.
- Creating a capability: `recipes/add-feature.md`.
- Making a feature callable by composition code: `recipes/expose-feature-capability.md`.
- Coordinating several features: `recipes/compose-features.md`.
- Moving code into shared: `rules/shared-code.md` and `recipes/promote-shared-code.md`.
- Installing or changing import checks: `recipes/enforce-boundaries.md`.
- Adding tests: `rules/testing.md`.
- Challenging a structural decision: open the relevant file under `adrs/`.

## Non-negotiable constraints

- Preserve `app → features → shared` dependency direction.
- Do not create feature-to-feature imports. Compose features from `app`.
- Import a feature through a named root entry file; never import another feature's `internal/` paths.
- Keep business vocabulary out of `shared`.
- Create only the internal segments a feature actually uses.
- Update import-boundary configuration when paths or layers change.

Before finishing, run the repository's typecheck, tests, module-aware import checks, and
`patterns check --include-tests` when available. The manifest checker covers layer direction,
`app → internal`, and cross-feature imports via `isolations`; the module-aware check must additionally
distinguish root-qualified feature identities (same-slug features in different applications) and
resolver-level import forms. A passing unit test does not excuse a forbidden dependency.
