# Add an outbound adapter

1. Start from an application need and define or select the narrow outbound port it requires.
2. Create `src/adapters/outbound/<adapter>/` for the concrete technology.
3. Map application-owned values to external requests or records.
4. Validate and map external results back to application-owned values.
5. Translate expected technology failures to the port's failure semantics; do not leak vendor errors.
6. Run the shared port-contract suite against the adapter.
7. Add integration tests for mapping, configuration, and real resource behavior.
8. Select the adapter in bootstrap. If it replaces an existing implementation, follow
   `replace-adapter.md` for parallel verification and rollback.

Do not expand the port merely because the external SDK offers more operations. A port changes when the
application's required capability changes.
