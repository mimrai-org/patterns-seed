# Dependency and runtime flow

Compile-time dependencies and runtime calls point in different directions.

```text
Compile time: bootstrap -> adapters -> application -> domain
Runtime:      inbound adapter -> use case -> outbound port -> injected adapter
```

The application imports the port contract but not its implementation. Bootstrap constructs an adapter
and passes it into the use case. At runtime the call leaves the core through that injected object,
without reversing the source-code dependency.

A typical request follows these translations:

1. External payload to inbound DTO.
2. Inbound DTO to technology-neutral use-case input.
3. Use case input to domain concepts and behavior.
4. Domain or application values to an outbound port request.
5. Port request to an external model inside the outbound adapter.
6. Use-case result to an external response inside the inbound adapter.

Passing an adapter through a global locator hides this flow and makes dependencies implicit. Prefer
constructor parameters or explicit factories. A framework container may perform the final wiring, but
its tokens and decorators must not leak into domain policy.
