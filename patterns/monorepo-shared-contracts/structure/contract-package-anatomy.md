# Contract package anatomy

A contract package is independently discoverable by the workspace and exposes only supported contract
entry points.

```text
packages/contracts-<capability>/
├── package.json
├── tsconfig.json          optional when the selected strategy needs compilation
├── src/
│   ├── <operation>.ts
│   ├── <event>.ts
│   └── <value>.ts
└── tests/
    ├── parsing.test.ts
    └── compatibility.test.ts
```

## Package manifest

The package manifest provides:

- A unique, namespaced package name such as `@workspace/contracts-<capability>`.
- A stable release identity: a semantic version when package versioning is used, or an immutable tag,
  commit, or build identifier captured by compatibility CI for private unversioned packages.
- `private: true` unless external publication is an intentional release responsibility.
- An `exports` map with one entry per supported contract surface.
- A package-local build or typecheck script appropriate to its distribution strategy.
- Direct dependencies for runtime schema or generation libraries actually imported by this package.

Consumers import declared subpaths such as `@workspace/contracts-<capability>/<operation>`. Avoid a
catch-all barrel that exposes every contract, increases accidental coupling, or makes a private file
look supported. The package root may exist when the whole package is intentionally one small surface;
it must still re-export only stable entries.

## Source entries

Group the request and result for one operation when they share lifecycle and ownership. Give an event
or message its own entry when consumers subscribe to it independently. Small wire-level values may be
shared inside the package, but do not create abstract base schemas solely to reduce line count.

An entry may expose a runtime schema, its derived TypeScript type, and serialization metadata. It must
not expose application services, callbacks, framework request objects, ORM records, database clients,
transport clients, environment access, or application bootstrap code.

## Compatibility fixtures

Keep fixtures as serialized, synthetic boundary values rather than application models or executable
producer helpers. Fixtures used only by the package remain private under `tests/`. When multiple
consumer test suites deliberately share a corpus, expose a narrow `./testing/<contract>` subpath and
do not re-export it from a production entry. A consumer must never deep-import `tests/` to bypass the
package's public surface. The export may resolve to files under `tests/` or a dedicated `src/testing/`
folder; what matters is that consumers reach the corpus only through the declared subpath, never by
filesystem path into `tests/`.

Record how CI obtains the previous supported reader: a published package version, immutable build
artifact, release tag, or pinned consumer commit. Rebuilding old source against the candidate package
does not reproduce the reader that remains deployed or available for rollback.

## Ownership metadata

Record an accountable team or role, current consumers, compatibility promise, and deprecation policy
in the repository's normal ownership system. The contract package is jointly used but not ownerless.
Changes require review from the contract owner and evidence from affected producers and consumers.

## Dependencies and compilation

Contract packages should have a small dependency surface. Install dependencies where used; do not rely
on a schema library hoisted from the root or an application. Choose source-exported, compiled, or
generated artifacts deliberately and keep output paths aligned with `exports` and task-runner cache
configuration. See `adrs/0005-select-compilation-by-consumer-needs.md` for the decision.
