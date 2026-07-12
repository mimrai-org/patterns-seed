# Dependency direction

Dependencies inside a slice point away from volatile edges and toward its operation contract and
policy.

- Transport may import the contract, validation, and callable handler surface.
- Validation may import the contract and stable shared primitives; it does not import the transport,
  handler, ports, or concrete adapters.
- Handler may import its contract, ports, local policy, and stable shared primitives.
- Concrete adapters may import the ports they implement and local mapping code.
- Transport does not import concrete outbound adapters.
- Bootstrap may import transports, handlers, adapters, and process-level facilities only to construct
  and assemble them; it contains no contract translation or workflow policy.
- Contract and ports do not import transport, concrete adapters, or bootstrap.
- Shared code imports no operation or bootstrap code.

The handler never selects a concrete adapter, reads a global container, or imports the executable
entry point. Dependency injection may use plain factories or a framework, but resolution belongs at
bootstrap and the handler receives named dependencies explicitly.

Code in one operation must not import another operation's handler, transport, adapter, tests, or
private contract. Coordinate through injected capabilities with an explicit workflow owner. Avoid
cycles even through re-exports, registries, or dynamic imports.

When two operation contracts need translation, a workflow-owned adapter may live at an explicit outer
composition edge and depend on their supported callable surfaces. It is not part of either operation's
internals, and those callable surfaces do not become general cross-slice APIs. Bootstrap constructs
that adapter but does not contain its mapping or policy.

When several roles remain in one file, an import graph cannot prove their internal separation. Split
the roles when deterministic enforcement is worth the navigation. What the manifest checks and what
needs resolver-aware tooling is defined in `recipes/enforce-boundaries.md`.

These rules do not recreate global horizontal layers. They preserve a small policy-versus-edge seam
inside each cohesive operation while allowing different slices to choose different internal designs.
