# Use an explicit composition root

Concrete adapters, configuration, and resource lifecycles are selected in bootstrap and injected into
the core. Global service location was rejected because it hides dependencies and couples isolated tests
to runtime state; explicit assembly adds wiring code but keeps construction policy visible.
