# Port design

The consuming operation owns each outbound port. Name it after the capability the handler needs and
include only the methods and values used by that operation. A port may be a small object interface or
an injected function; it is not required to be a class.

Introduce a port when an external dependency is volatile, needs semantic error translation, requires
controlled testing, or has multiple useful implementations. A stable language or library API can be
used directly when wrapping it would add no boundary. Do not create ports preemptively for every
function.

Port contracts use operation or technology-neutral values. They do not expose ORM entities, database
connections, HTTP clients, SDK responses, queue envelopes, or vendor exceptions. Model absence,
conflict, timeout, cancellation, and retryable failure only when the handler must make a decision about
them.

Prefer intent-specific capabilities over a universal repository, gateway, or service interface. A
generic persistence abstraction often leaks query language and couples unrelated slices to one model.
Two similar slice-owned ports may coexist until their semantics and change cadence are demonstrably
the same.

Adapters translate at the boundary and obey the port's lifecycle and failure contract. When several
adapters implement one port, run the same contract suite against each. Bootstrap chooses the concrete
implementation and manages connection, startup, and shutdown concerns.

When an explicit transaction is required, transaction-scoped ports must be created or bound inside its
callback so every participating write uses the same context. Do not pass an untyped transaction handle
through the handler merely to let adapters discover it.
