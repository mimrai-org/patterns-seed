# Enforce boundaries

1. Use the manifest's isolation rule (`within: "**/operations/*/**"`) for sibling-slice isolation: it
   captures the operation name at any nesting depth and rejects any import edge between two different
   operation names. Its one blind spot is equal operation slugs under two different `operations/`
   roots — they capture the same identity, so that cross-module edge is a false negative.
2. Use the manifest for the path edges it actually represents: shared-to-operation,
   shared-to-bootstrap, operation-to-bootstrap, and forbidden edges from canonical `handler.ts`,
   `transport.ts`, `contract.ts`, `validation.ts`, and `ports.ts` files toward canonical transport or
   concrete-adapter paths.
3. Add a resolver-aware, identity-keyed rule for what the manifest cannot see: key a slice as
   `<normalized path to its owning operations root, operation>` and reject equal operation slugs under
   two modules or applications. Do not encode `operations/** → operations/**` as a blanket prohibition
   because it would also reject valid imports inside one slice.
4. Add repository rules for noncanonical policy files, vendor imports, and process-wide adapter roots.
   The manifest cannot infer architectural roles from arbitrary path names.
5. Resolve TypeScript path aliases, package exports, re-exports, index files, dynamic imports, and the
   extensions used by the build. Analyze generated source separately or exclude it deliberately.
6. Include tests and test helpers so fixtures cannot become cross-slice APIs.
7. Add positive fixtures for transport-to-own-handler, handler-to-own-port, adapter-to-own-port, and
   bootstrap wiring in both a root `src/operations` layout and an `operations` root nested in a module.
8. Add negative fixtures for sibling imports, equal operation slugs in two owning roots,
   handler-to-transport, handler-to-concrete-adapter, transport-to-concrete-adapter,
   shared-to-operation, operation-to-bootstrap, and a cycle in both layouts.
9. Record that a one-file operation has no internal import edge to inspect. Split responsibilities
   when deterministic import enforcement becomes necessary; until then, use focused tests and review.
10. Run `patterns check --include-tests` for manifest rules, then the resolver-aware suite for identity,
    semantic path classification, and cycle detection.
11. Make both checks required in CI and require an owner, reason, and expiry condition for any temporary
    exception. Update checks and fixtures whenever operation roots, aliases, or file roles change.

A directory convention or editor warning is not enforcement. Conversely, an overbroad rule that blocks
valid local imports encourages bypasses; keep every automated prohibition aligned with the documented
dependency model and state explicitly which checker owns each rule.
