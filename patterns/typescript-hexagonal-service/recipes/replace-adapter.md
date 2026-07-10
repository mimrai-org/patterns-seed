# Replace an adapter

1. Confirm the application capability and semantics remain unchanged. If they do not, evolve the port
   deliberately before replacing anything.
2. Run the existing port-contract suite against the current adapter to establish the baseline.
3. Implement a new outbound adapter without changing domain or use-case imports.
4. Add integration tests for the new technology's mapping, consistency, and failure behavior.
5. Make adapter selection an explicit bootstrap configuration.
6. Run both implementations through contract tests and representative end-to-end flows.
7. Switch composition, observe the new adapter, and retain a rollback path when appropriate.
8. Remove the old adapter and its configuration only after the migration window closes.

Changes outside adapters, bootstrap, tests, and configuration indicate either a leaky old boundary or
a real change in application semantics. Treat those cases explicitly rather than claiming a transparent
swap.
