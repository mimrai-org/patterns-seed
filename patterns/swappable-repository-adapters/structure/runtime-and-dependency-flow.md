# Runtime and dependency flow

Compile-time dependencies point toward the repository's consumer:

```text
bootstrap -> concrete adapter -> application-owned port and values
application use case ---------> application-owned port and values
```

At runtime, an injected object carries the call outward:

```text
use case -> repository operation -> adapter mapper/query -> backing store
                                      |              |
                               application value  persistence record
```

The use case imports the port but not its implementation. The adapter imports the port and the values it
must persist. Bootstrap constructs a database client, constructs one adapter, and injects it into the
consumer. This apparent reversal between source dependencies and runtime calls is deliberate dependency
inversion, not a circular dependency.

Keep construction explicit. A factory or framework container may perform the final assembly, but global
service location, decorators, tokens, and configuration types must not leak into the port or application
policy. Changing the selected adapter should change bootstrap and infrastructure configuration, not a
branch in a use case.

Concrete adapters must not depend on one another. Shared low-level helpers may contain mechanical client
setup or neutral encoding only when they import no adapter and define no capability semantics. The
manifest's adapter isolation rule distinguishes same-adapter from cross-adapter imports by capturing the
adapter directory as an identity; `patterns check` fails any import that crosses sibling adapter
directories. Because that identity is not root-qualified, equal adapter slugs under different owning roots
still need package entry points, an import-path lint rule, or an architecture test keyed by both the
owning `persistence/adapters` root and the adapter directory. Module-to-root persistence imports are
intentionally left unconstrained: shared low-level helpers of the mechanical kind above may live at the
root, while root persistence code is forbidden from reaching into module persistence internals.

## Executable boundary coverage

The manifest recognizes root roles under `src/application`, `src/persistence`, and `src/bootstrap`, plus
the canonical modular roles `src/modules/<capability>/application`, `persistence`, and `bootstrap` where
`<capability>` is one path segment. The leading glob also supports those shapes inside monorepo applications.
It forbids root-to-module, module-to-root, and module-to-module policy leaks without matching the legitimate
application-owned port at `application/persistence` as concrete infrastructure. The manifest also isolates
sibling adapter directories under any `persistence/adapters` root, failing imports that cross from one
adapter into another.

Those globs cannot infer equivalent roles under arbitrary names or deeper module identities, nor can they
decide that equal slugs in different monorepo roots have different owners. Add a local house pattern,
root-qualified import-path lint rule, or architecture test for those cases. The scanner also cannot form a
local file edge to a bare package such as an ORM or database driver. Apply restricted-import or dependency
rules to application paths so “no storage tool in policy” is executable rather than only prose.

Migration code is intentionally outside the steady-state call path. A temporary comparison or change-
replay component may call current and candidate stores, but it is migration infrastructure with a removal
date, not the repository port implementation used indefinitely by application code.
