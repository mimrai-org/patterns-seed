# Graph enforcement

Enforce architecture at both package and file level. Directory conventions and successful compilation
do not catch undeclared, identity-sensitive, or runtime-compatibility violations.

CI must perform these deterministic checks:

1. Install from the committed lockfile without silently changing it.
2. Build a workspace graph from every package manifest and reject cycles plus every edge whose
   dependency is an application, including app-to-app and contract-to-app edges, across all dependency
   fields used by the toolchain.
3. Verify every internal import has a direct declared dependency and resolves through a public export.
4. Reject source-relative cross-package paths, deep imports, aliases that bypass package identity, and
   forbidden dynamic imports or re-exports.
5. Run `patterns check --include-tests` for resolved contract-package → application imports and a
   resolver-aware check for identity rules and import forms the glob scanner cannot prove.
6. Run contract unit tests and a two-direction compatibility matrix: the candidate reader parses every
   supported old payload, and the previous supported reader or consumer artifact parses candidate
   producer output. Then run typecheck, build, and tests for every affected producer and consumer.
7. Confirm generated artifacts are current and reproducible when generation is the chosen strategy.

Package tasks live in the package that owns the work. The root script only delegates to the workspace
runner, allowing it to schedule and cache from declared dependency edges. A compiled contract's task
declares its emitted output; a typecheck that writes incremental metadata declares that output too.

Turborepo's `boundaries` command can detect undeclared packages, cross-package file access, and tag
rules, but it is experimental at the reviewed revision. Use it as an additional check, not as the only
control. Equivalent workspace-graph scripts, linters, or dependency analyzers are valid when they cover
package identities, aliases, tests, re-exports, and cycles.

A serialized fixture proves only what the reader executing that fixture accepts. To make the reverse
compatibility claim, CI must execute code from the previous supported parser package, release tag, or
consumer commit without rebuilding it against the candidate contract. Feed candidate writer fixtures
to that artifact and retain its exact version or commit in the test report.

Keep positive and negative fixtures for the policy. At minimum, prove that app → contract and
same-package imports pass, while contract → app, app A → app B, undeclared package imports, deep
imports, and a contract cycle fail.
