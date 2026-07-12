# Add an inbound adapter

1. Select an existing use case; a new protocol should not create a duplicate application operation.
2. Define the external request or trigger schema inside `src/adapters/inbound/<adapter>/`.
3. Validate and map it to the use-case input.
4. Invoke the use case through its callable contract.
5. Map successful and expected failure results to protocol-specific responses.
6. Keep authentication parsing, acknowledgements, status codes, and serialization inside the adapter.
7. Add adapter integration tests and one bootstrap wiring test.
8. Verify the adapter imports application and domain contracts, never an outbound adapter.

If the new protocol needs different business semantics, add or change a use case rather than branching
on protocol inside the application core.
