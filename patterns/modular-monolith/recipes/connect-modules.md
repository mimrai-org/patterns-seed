# Connect modules

1. State whether the caller needs an immediate result or only an independent reaction.
2. For an immediate result, import the target API contract through its root entry point and declare it
   as a dependency of the consuming module factory.
3. Have the host create the target first and inject its initialized API into the consumer; do not call a
   process-global exported instance or service locator.
4. For an independent reaction, publish a versioned integration event after the owning change commits.
5. Decide whether the reaction is local to one runtime instance or needs cross-replica, durable
   delivery; add external infrastructure only for the latter semantics.
6. Map contracts at the boundary; do not share mutable internal objects.
7. Define failure, timeout, idempotency, ordering, and retry behavior appropriate to the mechanism.
8. Add a contract test, a representative cross-module integration test, and composition tests for
   startup failure and reverse-order shutdown.
9. Run module-cycle and private-import checks; the runtime dependency graph must remain acyclic.

Do not add an event broker or eventual consistency by default. Start with injected in-process public
APIs or local events and add durable delivery only when failure recovery or cross-replica delivery
requires it.
