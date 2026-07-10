# Input validation

Treat every transport payload, path value, header, environment-derived value, and message body as
unknown until parsed. The transport calls the operation's validator and receives either a typed input
or explicit structural failures. Do not use a TypeScript assertion to turn untrusted data into the
contract.

Structural validation covers shape, primitive ranges, syntax, required combinations, and bounded
normalization that do not require current application state. Keep protocol concerns such as HTTP
status codes, header syntax, and queue envelope fields in the transport mapping.

State-dependent decisions belong in the handler or its local domain policy, inside the relevant
consistency boundary. Existence, uniqueness under concurrency, allowed state transitions, quotas, and
authorization based on current state cannot be made safe by an edge schema alone. Back important
invariants with storage constraints where possible and translate their failures deliberately.

Authentication may be established by process middleware, but pass the resulting actor or principal
context explicitly when policy needs it. Authorization that determines whether the operation is
allowed remains visible in the slice; do not rely on a route declaration when the same handler can be
invoked by another transport.

Validation failures are stable operation values with safe field or rule identifiers. Do not leak
parser internals, SQL details, provider responses, or sensitive input in errors or logs. Bound work by
limiting collection sizes, nesting, and expensive parsing before invoking the handler.
