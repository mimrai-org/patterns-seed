# Enforce boundaries in CI

Use complementary checks because no single file glob represents package identity and runtime
compatibility.

1. Classify direct workspace packages as `application`, `contract`, or another explicit library role.
2. Build the dependency graph from package manifests and fail on app → app, contract → app, any package
   → app, and cycles. Include dependency fields used by builds, tests, and scripts.
3. Verify internal imports correspond to declared direct dependencies and resolve through `exports`;
   reject source-relative cross-package paths and alias or deep-import bypasses.
4. Run `patterns check --include-tests` to reject resolved imports from scanned contract-package code,
   including discovered tests, into scanned application code.
5. Add a resolver-aware import check for re-exports, dynamic imports, generated code, and tests. The
   manifest isolation (`within: apps/*/**`) already compares application identities for resolved
   imports; the resolver-aware check covers the forms the scanner cannot see and compares package
   identities rather than forbidding `apps/** → apps/**` as a blanket glob.
6. Add positive fixtures for app → contract, same-app, and same-package imports.
7. Add negative fixtures for contract source → app source, contract test → app source, contract source
   → app test, app A → app B, undeclared dependency, private subpath, and a contract cycle.
8. Run contract tests and build, typecheck, lint, and test every affected dependent when a contract
package changes.
9. For generated contracts, regenerate in CI and fail when the working tree differs.

If using Turborepo, package scripts own the work and root scripts delegate with `turbo run`. The
experimental `turbo boundaries` check may supplement the policy, but retain a stable graph assertion
for the required roles and negative fixtures.
