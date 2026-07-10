# Ports and adapters

A port is a technology-neutral application contract. An adapter translates between that contract and
one external mechanism.

## Inbound side

An inbound adapter receives an external trigger, validates protocol shape, builds a use-case input,
invokes the use case, and maps the outcome to a protocol response. The use case function or class is
already an inbound port when its callable contract is clear; adding a duplicate interface solely for
symmetry is optional ceremony.

## Outbound side

An outbound port expresses a capability required by application policy, for example loading one
aggregate, obtaining current time, reserving a resource, or publishing a committed event. It is owned
by the application because the consumer defines the smallest useful contract. An outbound adapter
implements that capability using a concrete tool.

## Adapter isolation

Inbound adapters do not call outbound adapters directly. They call use cases, which call ports.
Outbound adapters do not depend on delivery protocols. Two adapters for one port should share a port
contract test where behavior can be observed consistently.

Do not create a port for a pure, stable library with no external side effect or replacement pressure.
A boundary earns an abstraction when it protects policy from volatility, enables an important test,
or supports multiple implementations.
