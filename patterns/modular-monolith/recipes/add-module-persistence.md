# Add module persistence

1. Define the module-owned state and invariants before choosing a storage model.
2. Place schema definitions, migration files, mapping, repositories, and queries inside the module.
3. Namespace tables, collections, schemas, and migration identifiers by module and prohibit
   cross-module foreign keys to private storage.
4. Expose an opaque migration source through the module lifecycle and register it with one host
   migration runner. Use deterministic discovery and ordering, globally unique migration identities,
   and one applied-migration ledger.
5. Keep persistence types private and map them to domain or application-owned values.
6. Design expand/contract changes for deployments where old and new instances overlap; do not require
   the new application version and a breaking schema mutation to become active atomically.
7. Add integration tests for mapping, constraints, transactions, and migrations from both an empty
   database and the previous released schema.
8. Expose required information through a public operation or published event, never direct storage
   access from another module.
9. Document backup, rollback, and retention needs that differ from platform defaults.

If an existing table is already shared, assign ownership first. Migrate other consumers to public
contracts or local projections before enforcing exclusive access.
