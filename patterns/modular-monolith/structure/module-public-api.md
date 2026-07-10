# Module public API

`src/modules/<module>/index.ts` is the only entry point visible outside the module. It explicitly
exports a module factory or registration function, the stable API contract returned by that factory,
input and result types, and published integration-event contracts. The factory returns an initialized
surface such as `{ api, lifecycle }`; lifecycle may expose an opaque migration source without exposing
the migration files themselves. The entry point does not expose a process-global singleton.

Keep private:

- Domain entities and internal state shapes.
- Persistence records, ORM models, repositories, and migrations.
- Use-case handlers and adapter implementations.
- Dependency-injection tokens and framework modules unless registration requires an opaque handle.
- Internal events and implementation-specific errors.

The public API uses module-owned, technology-neutral contracts. A dependent module receives an
initialized API instance through its own factory dependencies; it does not reach into a global
container. The API does not expose an unrestricted service locator or generic repository. Version
changes by compatibility: adding an optional operation may be compatible; changing a result, error,
or event meaning is not.

Do not import the public entry point from inside the same module. Internal code uses direct inward
dependencies, avoiding cycles through its own facade.

When modules are workspace packages, package `exports` can make private paths unreachable. In a single
source tree, pair import rules with an architecture test that resolves every cross-module import and
accepts only the target module's root entry point.
