# Model mapping

External models stop inside their adapters.

- Inbound adapters map request, message, command-line, or schedule data to use-case inputs.
- Outbound adapters map application values to persistence records, SDK requests, or wire payloads.
- Adapter results are validated and mapped back to application-owned values before returning.

Do not pass HTTP DTOs, ORM entities, query-builder rows, SDK responses, or framework exceptions into
domain or application code. Their shapes and lifecycle rules belong to the technology that produced
them.

Keep mappers beside the adapter whose model they understand. If mapping reveals a business invariant,
the invariant belongs in the domain and the adapter should invoke it rather than duplicate it. Test
round trips and lossy conversions explicitly.
