# Research sources

This bundle is an original, clean-room synthesis of public architecture concepts. No source code,
configuration, diagram, example domain, or prose was copied into the bundle.

Sources were reviewed on 2026-07-10:

- [Bulletproof React at `9506629`](https://github.com/alan2207/bulletproof-react/tree/9506629ed003a561c6627735480cce4994244bb4),
  especially its [project-structure guidance](https://github.com/alan2207/bulletproof-react/blob/9506629ed003a561c6627735480cce4994244bb4/docs/project-structure.md).
  Consulted for capability modules, optional internal segments, feature isolation, and application
  composition. The project is [MIT licensed](https://github.com/alan2207/bulletproof-react/blob/9506629ed003a561c6627735480cce4994244bb4/LICENSE).
- [`eslint-plugin-boundaries` at `d46b0dc`](https://github.com/javierbrea/eslint-plugin-boundaries/tree/d46b0dcb500539bafe06c0ccefc0d42efc586ded).
  Consulted for module-aware dependency classification and enforcement that can distinguish feature
  identities. The project is [MIT licensed](https://github.com/javierbrea/eslint-plugin-boundaries/blob/d46b0dcb500539bafe06c0ccefc0d42efc586ded/LICENSE).
- [`dependency-cruiser` at `6bb22b6`](https://github.com/sverweij/dependency-cruiser/tree/6bb22b6fe0815e6bdac0d6a52e676f2b2dc4cd5e).
  Consulted as a graph-level alternative for forbidden dependencies and cycles. No configuration was
  copied. The project is [MIT licensed](https://github.com/sverweij/dependency-cruiser/blob/6bb22b6fe0815e6bdac0d6a52e676f2b2dc4cd5e/LICENSE).
- [ESLint `no-restricted-imports` at `9835414`](https://github.com/eslint/eslint/blob/983541491393323d17717683f6a2e6dbef4bc3f4/docs/src/rules/no-restricted-imports.md).
  Consulted to distinguish static import restrictions from broader dependency analysis. ESLint is
  [MIT licensed](https://github.com/eslint/eslint/blob/983541491393323d17717683f6a2e6dbef4bc3f4/LICENSE).
- [TypeScript `paths` documentation at `c8170c3`](https://github.com/microsoft/TypeScript-Website/blob/c8170c35bda4811c9516cbb69c39241ae4beb6d9/packages/tsconfig-reference/copy/en/options/paths.md).
  Consulted to document that aliases do not create runtime or architecture boundaries. The
  documentation is [CC BY 4.0](https://github.com/microsoft/TypeScript-Website/blob/c8170c35bda4811c9516cbb69c39241ae4beb6d9/LICENSE).
- [React guidance on sharing state at `ef208e1`](https://github.com/reactjs/react.dev/blob/ef208e178341057f36efa7b57637957da9fd7abe/src/content/learn/sharing-state-between-components.md).
  Consulted for nearest-owner state placement. The documentation is
  [CC BY 4.0](https://github.com/reactjs/react.dev/blob/ef208e178341057f36efa7b57637957da9fd7abe/LICENSE-DOCS.md).

The references are evidence and further reading, not endorsements or claims that this pattern is an
official derivative of any project.
