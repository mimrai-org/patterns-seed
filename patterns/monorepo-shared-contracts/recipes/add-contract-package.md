# Add a contract package

1. Confirm that at least two applications need the same boundary semantics and name the accountable
   owner, producers, consumers, and compatibility window.
2. Choose a capability-sized name under `packages/contracts-<capability>` and ensure the workspace
   configuration discovers that direct package directory without creating nested packages.
3. Decide whether the authority is an authored runtime schema, an external protocol with generated
   output, or a justified type-only contract.
4. Add a package manifest with a namespaced name, `private: true` unless publication is intentional,
   the repository's package version when versioning is used, direct dependencies, package-local
   scripts, and no exports beyond the first supported entries.
5. Add only the TypeScript configuration required by the selected compilation strategy. Do not create
   project references merely to make workspace resolution work.
6. Create narrow source entries and an explicit `exports` map. Keep business behavior, framework
   objects, clients, persistence, and environment access out.
7. Add parsing, serialization, export-surface, and compatibility tests using synthetic fixtures.
   Keep fixtures private unless a deliberate `./testing/<contract>` export serves several consumers.
8. Add the package as a declared workspace dependency of each producer and consumer and update the
   lockfile.
9. Register the package's own build, typecheck, lint, and test tasks; let the root task runner delegate
   and order them through the dependency graph.
10. Record whether future compatibility CI will obtain the previous reader from a package version,
    immutable artifact, release tag, or pinned consumer commit.
11. Run graph policy, cycle detection, contract tests, and affected application checks before merging.

Do not begin with `shared-types`. If the capability and compatibility owner cannot be named, the
proposed package boundary is not ready.
