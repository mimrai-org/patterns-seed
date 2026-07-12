# Testing strategy

Test at the narrowest boundary that proves the behavior.

## Domain tests

Exercise invariants and value semantics with no mocks, network, filesystem, clock, or database. Supply
values directly and keep these tests fast.

## Use-case tests

Invoke a use case with small in-memory or recording implementations of its outbound ports. Assert the
application outcome and meaningful interactions, not private method order.

## Port-contract tests

Define observable examples every implementation of a port must satisfy. Run the same suite against an
in-memory adapter and each production adapter. Contract tests make substitutability evidence-based.
Keep in-memory and recording port implementations used by use-case tests beside those tests under
`src/application`; when an in-memory implementation is a real deliverable under `src/adapters`, its own
test file imports the shared contract suite (never the reverse), so the import direction stays inward
and `patterns check --include-tests` passes.

## Adapter integration tests

Exercise mapping, queries, protocol behavior, and failure translation with the real framework or a
representative external resource. These tests belong with the adapter.

## End-to-end tests

Use a small number to verify bootstrap wiring and one complete path through real boundaries. Do not
move all business scenarios to the slowest layer.

Apply architecture checks to tests as well as source. Test helpers must not introduce dependencies the
production architecture forbids.
