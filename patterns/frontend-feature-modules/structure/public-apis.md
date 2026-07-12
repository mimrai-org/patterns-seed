# Feature public APIs

Each feature exposes a few deliberate, named files at `features/<feature>/`. Application composition
imports those files and treats `features/<feature>/internal/**` as private.

A public API may expose:

- A route-level or embeddable component.
- A small hook or command intended for application orchestration.
- Stable input and result types required by those exports.
- An event or callback contract that lets `app` compose capabilities without lateral imports.

Name entries by supported capability, such as `screen.tsx`, `routes.tsx`, or `contract.ts`, rather than
technical internals. Avoid a catch-all `index.ts` that re-exports the directory: broad barrels obscure
the public contract, can worsen cycles, and may interfere with tree-shaking. Internal refactors should
remain invisible to consumers, and exported types should not leak vendor response types or private
state shapes.

Enforce the contract with import restrictions that allow named root files but reject `internal/**`
from outside that feature. Boundary globs cannot compare two wildcard identities, so cross-feature
imports are declared as a manifest `isolations` rule; a module-aware linter or architecture test is
still required to qualify identity by architecture root (in a monorepo the identity is the pair
`<architecture root, feature>`, so equal slugs in different applications remain different owners).

When public entries grow into unrelated groups, the feature may contain more than one capability.
When two features repeatedly need the same business behavior, do not move that behavior to `shared`;
identify the owner and let `app` coordinate through public contracts.
