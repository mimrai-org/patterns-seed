# Research sources

This bundle is an original, clean-room synthesis of public workspace and contract-design concepts. No
source code, configuration, diagram, example domain, or prose was copied into the bundle.

Sources were reviewed on 2026-07-10:

- [`create-t3-turbo` at `8f945b7`](https://github.com/t3-oss/create-t3-turbo/tree/8f945b7bb3bfb3ca8358d48b1ff0214079bc11ee),
  especially its [`packages/validators`](https://github.com/t3-oss/create-t3-turbo/tree/8f945b7bb3bfb3ca8358d48b1ff0214079bc11ee/packages/validators),
  [workspace declaration](https://github.com/t3-oss/create-t3-turbo/blob/8f945b7bb3bfb3ca8358d48b1ff0214079bc11ee/pnpm-workspace.yaml),
  and application package manifests. Consulted as a current example of several web and native
  applications consuming explicit internal packages. At this revision, `packages/validators` is an
  explicitly unused placeholder suggesting where future shared validators could live, so it is
  placement evidence rather than evidence of a deployed cross-application contract. The project is
  [MIT licensed](https://github.com/t3-oss/create-t3-turbo/blob/8f945b7bb3bfb3ca8358d48b1ff0214079bc11ee/LICENSE).
- [Turborepo at `9f3d24a`](https://github.com/vercel/turborepo/tree/9f3d24ab5bd1bcbf0b1f60d32e43eb5a342bc985),
  especially the official guidance on
  [package types](https://github.com/vercel/turborepo/blob/9f3d24ab5bd1bcbf0b1f60d32e43eb5a342bc985/apps/docs/content/docs/core-concepts/package-types.mdx),
  [internal packages](https://github.com/vercel/turborepo/blob/9f3d24ab5bd1bcbf0b1f60d32e43eb5a342bc985/apps/docs/content/docs/core-concepts/internal-packages.mdx),
  [repository structure](https://github.com/vercel/turborepo/blob/9f3d24ab5bd1bcbf0b1f60d32e43eb5a342bc985/apps/docs/content/docs/crafting-your-repository/structuring-a-repository.mdx),
  and the [experimental boundaries command](https://github.com/vercel/turborepo/blob/9f3d24ab5bd1bcbf0b1f60d32e43eb5a342bc985/apps/docs/content/docs/reference/boundaries.mdx).
  Consulted for deployable applications as package-graph ends, package graph discovery, export
  strategies, task ordering, and graph enforcement trade-offs. The project is
  [MIT licensed](https://github.com/vercel/turborepo/blob/9f3d24ab5bd1bcbf0b1f60d32e43eb5a342bc985/LICENSE).
- [pnpm workspace documentation at `3afd3d1`](https://github.com/pnpm/pnpm.io/blob/3afd3d1a0ff0a1b8b6de72f6cf77b1eaf63223d8/docs/workspaces.md).
  Consulted for explicit local dependency resolution with the `workspace:` protocol and the warning
  that cyclic workspace dependencies lose reliable topological script ordering. The documentation is
  [MIT licensed](https://github.com/pnpm/pnpm.io/blob/3afd3d1a0ff0a1b8b6de72f6cf77b1eaf63223d8/LICENSE).
- [Bun workspace guidance at `65aa83b`](https://github.com/oven-sh/bun/blob/65aa83bbafc4bbe172928e897bb9f6d3b58fd0c9/docs/guides/install/workspaces.mdx).
  Consulted for Bun's package-local dependency guidance and `workspace:*` protocol. Bun is
  [MIT licensed](https://github.com/oven-sh/bun/blob/65aa83bbafc4bbe172928e897bb9f6d3b58fd0c9/LICENSE.md),
  with separately licensed linked components documented in that notice.
- [TypeScript project references at `c8170c3`](https://github.com/microsoft/TypeScript-Website/blob/c8170c35bda4811c9516cbb69c39241ae4beb6d9/packages/documentation/copy/en/project-config/Project%20References.md)
  and the [module reference](https://github.com/microsoft/TypeScript-Website/blob/c8170c35bda4811c9516cbb69c39241ae4beb6d9/packages/documentation/copy/en/modules-reference/Reference.md).
  Consulted for the compilation behavior and costs of project references, package exports, and
  type-only imports. The documentation is [CC BY 4.0](https://github.com/microsoft/TypeScript-Website/blob/c8170c35bda4811c9516cbb69c39241ae4beb6d9/LICENSE).
- [Semantic Versioning 2.0.0 at `f99d548`](https://github.com/semver/semver/blob/f99d5485190a47c0863949e7da810a5553e0ed4d/semver.md).
  Consulted for version communication when a contract package is externally published. The
  specification is [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/).

The references are evidence and further reading, not endorsements or claims that this pattern is an
official derivative of any project. Turborepo-specific controls are optional examples; the pattern's
package graph and compatibility rules are tool-independent.
