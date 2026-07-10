# Introduce a dispatcher

1. Record the measured problem direct invocation no longer solves: repeated ordered pipeline behavior,
   centralized handler registration, or several equivalent in-process entry mechanisms.
2. Confirm that a plain handler decorator or composition helper would not solve the smaller problem.
3. Keep existing operation input and result contracts independent from the dispatcher library. Wrap or
   register them at bootstrap rather than inheriting framework types throughout every slice.
4. Register exactly one handler per request and fail startup on missing or duplicate mappings. Avoid
   runtime string keys and container lookup inside application policy.
5. Define pipeline order and applicability explicitly. Validation, authorization, idempotency,
   transactions, logging, and tracing have different failure and side-effect semantics.
6. Preserve direct handler tests, then add integration tests through the real dispatcher for ordering,
   short-circuit behavior, result typing, and unexpected failures.
7. Measure startup, tracing clarity, and debugging cost. Document how an engineer finds a request's
   handler without executing the application.
8. Keep durable messaging separate. An in-process dispatcher does not provide persistence, retries
   across process failure, or delivery guarantees.

Remove the dispatcher if it becomes a service locator or if every request gains boilerplate without a
shared behavior that earns the indirection. CQRS semantics remain valid with direct function calls.
