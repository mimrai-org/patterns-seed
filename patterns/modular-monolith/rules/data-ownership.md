# Data ownership

A module is the only writer and semantic owner of its persistence model.

- Keep schema definitions, migrations, repositories, and queries inside the module.
- Do not import or share another module's ORM entity or persistence type.
- Do not issue direct reads or writes against another module's private storage.
- Request current behavior through its public API or build an event-fed local projection.
- Keep identifiers in public contracts opaque; an identifier does not grant storage access.

Physical colocation is not shared ownership. Separate schemas or roles are preferred when their
operational cost is justified. At minimum, review SQL and add integration tests that fail on forbidden
access.

Cross-module reports may use a deliberately owned reporting projection. They should not turn
application queries into an undocumented mesh of private joins. Document freshness, rebuild, and
access rules for the projection.
