# Testing

Tests belong to the same architectural owner as the behavior they verify.

- Keep a component or pure-function test beside its source when the relationship is one-to-one.
- Use a feature-level test for behavior spanning several internal files.
- Use app-level integration or end-to-end tests for routing, providers, and cross-feature composition.
- Test shared primitives without importing application or feature fixtures.

Prefer public behavior over private structure. A feature test should exercise its exported component or
capability and mock only external boundaries. It should not deep-import private helpers merely to raise
line coverage.

Test utilities follow the same ownership rule. A feature-specific render helper stays in that feature;
only a domain-agnostic provider harness used independently by several features belongs in shared test
support.

Apply import-boundary linting to test files. Tests may depend on their owner's public or private code,
but they must not introduce feature-to-feature or upward dependencies that production code forbids.
