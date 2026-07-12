# Source of truth and runtime flow

Choose one authoritative representation for each contract. Do not maintain an interface, validator,
and protocol document by hand and assume they remain equivalent.

## Supported source-of-truth modes

### Authored runtime schema

Use a TypeScript-compatible runtime schema when JavaScript or TypeScript applications share a boundary
and must validate values at runtime. Export the schema and derive the static type from it. This is the
default for network requests, queue messages, persisted payloads, files, and process boundaries.

### Authored protocol artifact

Use OpenAPI, JSON Schema, Protocol Buffers, or another protocol definition as the authority when
consumers span languages, repositories, or independent release systems. Generate the TypeScript
artifact into a dedicated package. Generated files are outputs, never a second hand-edited source.

### Type-only contract

Use a type-only entry only for values that do not cross a runtime trust boundary or when validation is
already guaranteed by a separate authoritative protocol. This mode gives compile-time coordination,
not runtime safety. Document where validation occurs.

## Runtime flow

```text
unknown input
  │
  ▼
contract parser ──▶ application-owned model/use case ──▶ contract serializer
                                                          │
                                                          ▼
                                                    external value
```

The producer parses untrusted input before application logic uses it and maps the contract value to an
application-owned model. Before emitting data, it maps its model back to the contract and verifies the
outbound shape when the risk warrants it. A consumer parses received data before rendering, storing,
or acting on it.

This mapping is deliberate. The contract describes what crosses the boundary; the application model
describes what the application needs internally. Making one object serve as transport payload,
persistence record, and domain model couples all three reasons to change.

## Schema behavior

Keep contract parsing deterministic and portable. Defaults, coercions, transforms, and refinements are
runtime behavior, so use them only when every consumer can accept their semantics. Avoid hidden calls,
clock access, environment access, database lookups, or application imports inside schemas.

Separate normalization from business decisions. A parser may reject an invalid wire value or normalize
an agreed representation; it must not decide whether an application operation is permitted.
