# Dependency and runtime flow

Source dependencies protect the operation's policy from delivery and integration technology:

```text
bootstrap ──▶ transport ──▶ validation / contract
    │              └──────▶ handler
    ├─────────────────────▶ handler ──▶ ports / local policy / shared
    └──────────────▶ adapters ─────────▶ ports
```

`bootstrap` imports concrete pieces, constructs adapters and handlers, then registers transports. It
does not contain contract mapping, retry policy, or workflow decisions. Nothing imports bootstrap.
Within an operation, transport may know the callable handler surface, but the handler never knows its
transport. An adapter implements a port; a port never imports its adapter. Stable shared primitives
have no dependency on any operation.

At runtime, an inbound adapter performs the following sequence:

1. Extract protocol values and identity or correlation context.
2. Parse structural input into the operation contract.
3. Reject invalid input before application policy or external effects run.
4. Invoke the handler directly or through an optional dispatcher.
5. Let the handler load current state, enforce state-dependent rules, and coordinate effects through
   its ports inside the chosen consistency boundary, using one transaction only when required.
6. Return a typed success or expected failure.
7. Map that result to the protocol and allow unexpected failures to reach one observable error
   boundary.

Avoid passing framework request objects, mutable ORM entities, transaction handles, or provider
responses across the whole path. Translate at the boundary that owns each representation. Request
metadata needed by policy becomes an explicit small value rather than an ambient global lookup.

Operation isolation is an additional rule: code below one root-qualified operation must not import code
below another. Define identity as `<normalized path to the owning operations root, operation>` so
equal-slug operations in separate modules or applications are not mistaken for the same slice. Manifest
role globs match canonical files below any `operations/<operation>` segment, including a root nested
inside a capability module. They cannot compare those captured identities, inspect multiple concerns
colocated in one file, or infer that an arbitrary process-wide path contains concrete adapters. Enforce
those cases with resolver-aware rules that classify operation identities and configured adapter roots
while understanding aliases, re-exports, and dynamic imports.
