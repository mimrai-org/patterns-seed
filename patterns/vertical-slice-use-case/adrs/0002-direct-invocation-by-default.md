# Invoke handlers directly by default

Transports call typed handler functions directly unless a dispatcher solves a measured registration or
pipeline need. Mandating a mediator was rejected because it can hide dependencies and add uniform
boilerplate without changing slice cohesion; direct invocation keeps control flow explicit while still
allowing a mediator to be introduced behind bootstrap later.
