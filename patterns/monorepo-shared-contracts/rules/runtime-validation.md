# Runtime validation

TypeScript types do not validate a value arriving from another process, network, queue, file, user, or
stored record. Treat such values as `unknown` and parse them at the first application-owned boundary.

For a runtime-capable contract:

1. Author one schema or protocol definition as the source of truth.
2. Derive or generate the TypeScript type from it; do not restate the shape manually.
3. Export the parser and type through the same deliberate package entry.
4. Parse inbound values before application logic or persistence sees them.
5. Map successful values into an application-owned model.
6. Convert parse failures into the boundary's explicit error behavior without leaking library-specific
   error objects across the contract.

Validate outbound values when data is assembled from untyped sources, when compatibility risk is high,
or when a serialized fixture is the release evidence. Do not use a type assertion as validation.

Strictness is part of compatibility. Decide whether unknown keys are rejected, removed, or preserved;
whether coercion occurs; and whether defaults change the observed value. Test these behaviors. Hidden
transforms, environment reads, database lookups, or clock-dependent refinements make a contract
non-portable and belong in application code.

If a type-only contract is justified, record which trusted boundary or external protocol performs
runtime validation. Never present compile-time agreement as proof that deployed payloads are safe.
