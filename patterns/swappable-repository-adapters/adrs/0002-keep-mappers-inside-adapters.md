# Keep mappers inside adapters

Each adapter owns its persistence records and the mapper between those records and application values.
Sharing ORM entities, document schemas, or one “neutral” persistence DTO across implementations was
rejected because storage constraints would then become part of the application contract and couple
otherwise independent adapters. The cost is duplicated translation and dedicated mapper tests; the benefit
is that each store can evolve its schema and representation without changing application policy.
