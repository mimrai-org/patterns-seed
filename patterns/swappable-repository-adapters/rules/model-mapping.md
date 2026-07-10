# Model mapping

Persistence records stop inside their adapter. A mapper translates from application- or domain-owned
values to the adapter's record shape before a write, and validates and reconstructs application values
after a read.

Keep these details out of the port and its consumers:

- ORM entities, decorators, lazy relations, query builders, document schemas, and generated clients.
- Database identifiers or cursors whose representation is technology-specific.
- Driver date, decimal, binary, JSON, nullability, and error types.
- Storage defaults, denormalized fields, index helpers, and migration metadata.

Place a mapper with the adapter whose record it understands. Different adapters usually need different
records and mappers; sharing a persistence DTO between them creates an accidental common database model.
Shared application value constructors may validate invariants, but they must not import adapter code.

Document intentional loss and normalization. Test identifier fidelity, time zones and precision, numeric
range, optional versus null fields, collection ordering, version fields, and records written by supported
older schemas. A write-read round trip can miss corruption if both directions share the same mistake, so
also test known persistence fixtures against expected application values. Keep those raw records inside
the adapter integration suite; a shared contract may request a semantic historical state through a fixture
hook but must not import an adapter's record shape.

Do not make mappers perform network calls or application decisions. Relation loading and query planning
belong to the concrete repository; business defaults and invariants belong to application or domain code.
When a candidate cannot represent a required value or relationship, resolve that compatibility gap before
cutover instead of silently dropping information.
