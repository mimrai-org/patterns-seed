# Add a repository adapter

1. Read the application-owned port and its contract suite before reading the current adapter. Treat the
   contract, not current query code, as the required behavior.
2. Perform a semantic feasibility review: value ranges, identity, uniqueness, relationships, ordering,
   pagination, atomicity, visibility, concurrency, and expected failure translation.
3. If the candidate cannot meet a required guarantee, stop and choose whether to fix the candidate design,
   narrow or split the port, or make an application contract change. Do not add an adapter exception.
4. Create an isolated adapter directory with its own records or schemas, mapper, repository implementation,
   configuration, and migrations.
5. Implement only port operations. Keep candidate-specific optimizations and query mechanisms behind the
   adapter.
6. Add a lifecycle fixture and run the unchanged shared contract suite against a representative candidate
   resource.
7. Add adapter integration tests for mapper fixtures, schema versions, indexes, driver failures, resource
   lifecycle, timeouts, and retries.
8. Test capacity, latency, backup restore, and recovery against the migration acceptance targets.
9. Add candidate construction to bootstrap without changing the production selection.
10. Verify that application files did not change unless the port semantics deliberately changed, and that
    neither adapter imports the other.

Passing typecheck is necessary but not sufficient. Do not call the adapter substitutable until the shared
behavior suite and operational acceptance gates both pass.
