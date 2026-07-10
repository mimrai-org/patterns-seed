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

The `patterns.yaml` boundaries enforce the layer and host rules its glob model can express. A separate
architecture test must compare root-qualified source and target module identities to distinguish
legitimate internal imports from forbidden cross-module deep imports. In a monorepo, two modules with
the same directory name under different deployable roots are still different owners and must not import
each other's source. Data ownership needs database permissions, SQL analysis, or integration tests; an
import graph cannot prove it.
