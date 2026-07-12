# Split a contract package

1. Identify cohesive groups by owner, semantics, consumer set, and compatibility cadence. Do not split
   merely because the directory is large.
2. Choose names such as `contracts-<capability-a>` and `contracts-<capability-b>`; avoid a residual
   `common` package without an explicit owner.
3. Map existing public entries and consumers to their destination packages. Check whether any shared
   value truly deserves a contract-to-contract dependency or can remain duplicated.
4. Create the new package manifests, source-of-truth strategy, exports, tests, and package tasks.
5. Add new dependencies to consumers and migrate imports through public entries. Do not add aliases
   that silently redirect old deep paths.
6. Keep temporary compatibility re-exports only with an owner and removal date; ensure they do not
   create a dependency cycle.
7. Remove unused entries and dependencies, regenerate the lockfile, and run graph plus affected checks.
8. Update ownership and compatibility records for both packages.

A successful split lets an application depend on only the contract family it uses and allows each
package to evolve without review from unrelated consumers.
