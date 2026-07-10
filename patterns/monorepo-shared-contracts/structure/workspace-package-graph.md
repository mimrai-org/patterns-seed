# Workspace package graph

Model the repository as a directed graph of packages, not as a collection of directories that happen
to resolve through aliases. There are two roles:

- **Applications** are deployable packages under `apps/<application>` with no package dependents. They
  may depend on contract packages and other deliberately reusable libraries, but nothing uses an
  application as a library. With the arrows below defined as `consumer → dependency`, applications
  may have outgoing edges but have no incoming package-dependency edges.
- **Contract packages** are non-deployable libraries under `packages/contracts-<capability>`. They own
  public data vocabulary shared by two or more applications.

The normal edge is:

```text
application ──▶ contract package
```

The reverse edge and every `application A → application B` edge are forbidden. A browser application
does not import a service router, and a worker does not import the browser's convenience types. Each
uses the contract package and implements its own side of the interaction.

## Graph truth

Use the workspace package manifests as the authoritative dependency graph. A consumer declares the
contract package in its own `package.json` using the package manager's local-workspace syntax, updates
the lockfile, and imports the package by name. TypeScript `paths`, root aliases, filesystem-relative
imports, and dependency hoisting do not declare an architecture edge and must not substitute for that
dependency.

Keep the root package private and limited to repository tooling. Build, lint, typecheck, and test logic
belongs to the packages that perform it; the root may delegate orchestration to the workspace task
runner. This makes affected execution and dependency ordering follow the same graph as the code.

## Contract-to-contract dependencies

Prefer a self-contained contract package. Allow one contract package to depend on another only when a
stable public value truly has the same meaning and owner in both contract families. The edge must be
declared and the resulting graph must remain acyclic.

Do not introduce `contracts-common` merely to remove a few repeated primitives. Duplication of a small
shape is often cheaper than a global compatibility dependency. If a common contract is justified, it
must have its own owner, public exports, consumers, and compatibility policy.

## Package granularity

Create a package around a cohesive capability, protocol surface, or release boundary. Keep related
operations and events together when they evolve under the same owner. Split when parts have unrelated
owners, consumers, or compatibility cadence. Package count and file count alone do not decide the
boundary.

The manifest boundary catches resolved imports from any scanned file under `**/packages/contracts-*`
into any scanned file under `**/apps/*`, including discovered tests when `--include-tests` is enabled.
It cannot inspect an unresolved import, a package-manifest-only edge, ignored build output, or compare
two wildcard application identities. A workspace-graph and resolver-aware assertion must separately
cover those cases and reject app-to-app edges and cycles.
