# Package public APIs

The package manifest is the public-boundary declaration. Define an `exports` map and keep each entry
small, named by supported capability, and stable for its compatibility window.

Consumers must:

- Declare the contract package directly in their own `package.json`.
- Import by package name and declared subpath.
- Use `import type` when an entry is genuinely type-only.
- Import a runtime schema as a runtime dependency when parsing values.

Consumers must not:

- Import `../../packages/...`, `src/`, `dist/`, or another unexported path.
- Deep-import the package's test directory to obtain compatibility fixtures.
- Bypass `exports` through a TypeScript path alias.
- Receive the dependency accidentally through the repository root or a sibling's hoisted install.
- Import a producer's router, handler, model, or generated application types.

Avoid a barrel that re-exports every file. A broad root entry makes unrelated changes invalidate more
consumers, hides which surface is used, and can accidentally expose private helpers. Prefer subpath
exports and add a root entry only when the package is intentionally one small API.

Keep `exports`, emitted files, declaration files, source maps, and task outputs aligned. A source-export
strategy points exports to source only when every consumer can transpile that syntax. A compiled or
generated strategy points runtime and type conditions to files produced and verified by that package.

Keep compatibility fixtures private when only the contract package uses them. If several consumer
test suites intentionally share one serialized corpus, expose it through a narrow, documented
`./testing/<contract>` subpath. Treat that subpath as a versioned test API, keep it out of production
root exports, and consume it only from test or development code.

Run an export-surface test from outside the package directory so the test observes the same resolution
rules as a real consumer. Internal relative imports can otherwise conceal a broken package manifest.
