# Layers and roles

## Domain

`src/domain` holds business concepts, invariants, value semantics, and pure domain services. It has no
knowledge of use-case orchestration, protocols, persistence, frameworks, environment variables, or
bootstrap. It may use a deliberately accepted deterministic, side-effect-free library when the
library's types remain internal and no policy boundary is gained by wrapping it. A domain object
protects a rule that must remain true regardless of how the operation was triggered.

## Application

`src/application` coordinates outcomes. A use case validates application-level preconditions, invokes
domain behavior, calls outbound ports, and returns a technology-neutral result. It does not parse HTTP,
build SQL, publish through a vendor SDK, or choose an implementation.

Application-owned ports describe the external capabilities use cases require. They usually live near
the consuming use cases or under `application/ports` when several use cases share one cohesive need.

## Adapters

`src/adapters/inbound` translates a protocol or trigger into a use-case input and maps the result back
to that protocol. `src/adapters/outbound` translates an application port into calls to persistence,
messaging, time, identity, or another external system. Adapters may use frameworks and vendor types;
those types stop at the boundary.

## Bootstrap

`src/bootstrap` reads configuration, creates resources, selects adapters, wires use cases, starts the
inbound runtime, and owns shutdown. It contains assembly policy, not business policy.

A layer is a responsibility, not a requirement to create one class per file. Keep trivial behavior
simple while preserving the dependency direction. When several services live in one repository, the
normalized service or package root is part of every layer's identity; matching `src/domain` path shapes
do not make two services one core.
