# Invocation and CQRS

Use command and query as semantic descriptions, not infrastructure requirements.

- A query returns information and does not intentionally change application state. Logging, tracing,
  and cache maintenance do not turn it into a command, but callers must not depend on those incidental
  effects.
- A command requests a state transition or external effect and may return an identifier, status, or
  representation useful to its caller. It need not return `void`.

Separate input and result types when read and write operations have different reasons to change. This
does not require separate folders, databases, deployment units, or an event-sourced model.

Invoke a handler directly by default. A typed factory can inject its ports and return an `execute`
function. This keeps control flow, ownership, and call dependencies visible.

Add a mediator or dispatcher only when centralized registration, several consistently applied pipeline
behaviors, or alternate in-process invocation materially reduces duplication. Keep one typed request
to one handler, fail startup on missing or duplicate registrations, and test the real pipeline order.
Observability, authorization, validation, consistency decisions, any required transaction, and
idempotency remain operation semantics even when infrastructure applies part of them.

An in-process bus is not automatically asynchronous, durable, or decoupled. Do not use it to conceal a
synchronous dependency, service location, or cyclic workflow. Events represent facts with zero or more
independent reactions; they are not a substitute for a command whose caller needs success or a result.
