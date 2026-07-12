# Harden boundaries in CI

1. Classify host, shared, each module root, contracts, registration, and internal paths with a
   resolver-aware dependency tool. Key a module as `<normalized deployable root, module>`; the module
   directory name alone is not unique across applications in a monorepo.
2. Allow cross-module imports only when both modules belong to the same deployable root and the target
   resolves to its public entry point. Cross-deployable collaboration uses an explicit package or
   protocol contract rather than another application's source tree.
3. Reject host deep imports, module-to-host imports, cross-module private imports, inward layer
   violations, and cycles.
4. Include static imports, re-exports, dynamic imports, path aliases, and test files.
5. Add a positive fixture for an allowed internal import and public module call inside one root.
6. Add negative fixtures for cross-module internals, forbidden layer edges, and equal module slugs in
   two deployable roots.
7. Run `patterns check --include-tests`: the manifest boundaries verify host, layer, and shared rules,
   and the manifest isolation rejects cross-module internal imports within one root. Keep the
   module-aware architecture suite for root-qualified identity (equal slugs across deployable roots)
   and for entry-point-only enforcement of `contracts/` and `register.ts`.
8. Add database access checks or SQL review when logical data ownership needs enforcement.
9. Make the checks required in CI before protecting the main branch.

Do not use `src/modules/** -> src/modules/**` as a generic prohibition: it also rejects valid imports
inside one module. Root-qualified identity comparison still requires the dedicated suite; the manifest
isolation covers sibling internals within one root.
