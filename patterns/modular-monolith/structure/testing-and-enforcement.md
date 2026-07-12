# Testing and enforcement

## Module behavior

Test domain and use-case behavior inside its owning module. Test the public facade with in-memory or
representative adapters so consumers can rely on documented outcomes without accessing internals.

## Contracts

Consumer-facing tests verify public operation inputs, results, failures, and integration-event schemas.
Adapters and persistence receive integration tests inside the module that owns them.

## System behavior

Host-level tests cover registration, configuration, and a small set of workflows that cross public
module APIs. End-to-end tests should not become the only place business rules are checked.

## Architecture

CI should inspect resolved imports and fail when:

- Host or another module imports `internal`, `contracts`, or `register.ts` directly.
- A module imports host code.
- Module domain or application code depends on its adapters.
- The module graph contains a cycle.
- Shared code imports a module.

The `patterns.yaml` boundaries enforce the layer and host rules, and its `isolations` rule rejects
cross-module `internal` imports within one source tree. A separate architecture test must still compare
root-qualified module identities: in a monorepo, two modules with the same directory name under
different deployable roots are different owners, and deep imports of `contracts/` or `register.ts` from
another module's non-internal files also need the test. Data ownership needs database permissions, SQL
analysis, or integration tests; an import graph cannot prove it.
