# Data ownership

Each module owns the data required to enforce its rules. Ownership includes persistence models,
migrations, query definitions, writes, and the meaning of the stored fields.

Modules may share one database server or physical database, but they do not read or write another
module's private tables or collections. Use separate schemas, database roles, migration locations, or
access tests where the infrastructure supports them.

A host-level migration runner receives each module's migration source through its public lifecycle,
discovers migrations in a deterministic order, and records globally unique applied identifiers. Module
migrations change only owned storage and avoid foreign keys into another module's private schema. Test
a clean build and an upgrade from the previous released schema; use expand/contract changes when
deployment replicas may run different application versions during a rollout.

To obtain another module's current interpretation of its data, call its public API. To maintain a
local projection for independent reads, subscribe to a published integration event and accept the
declared consistency delay. Do not share ORM entities or join private tables across module boundaries.

Cross-module atomic transactions tightly couple ownership. Prefer one module owning the transaction;
when an outcome truly spans modules, make the consistency and compensation policy explicit. Do not
introduce eventual consistency solely to make the architecture look distributed.

Backups, retention, and physical capacity may remain platform concerns, while schema meaning and
migration compatibility remain with the owning module.
