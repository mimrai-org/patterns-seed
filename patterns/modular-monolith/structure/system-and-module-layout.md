# System and module layout

## Host

`src/host` is the executable shell. It validates configuration, initializes runtime-wide resources,
creates modules through their public entry points in dependency order, starts their lifecycles and
transports, and coordinates shutdown. It does not implement capability policy or reach into module
internals.

## Modules

Each directory under `src/modules` owns one cohesive business capability. Its root `index.ts` is the
only path available to the host or another module. A module may stay flat when simple; as policy grows,
its `internal` area separates application behavior, domain rules, and technology adapters.

Module boundaries should follow reasons to change, vocabulary, data ownership, and team ownership.
Do not create one module per database table, route, or technical service. A capability should make
sense to a domain expert without explaining its framework.

## Shared

`src/shared` holds a deliberately small set of domain-agnostic primitives such as logging contracts,
base observability types, or language-level utilities. Shared code imports no module and no host code.
Cross-module business behavior needs an explicit owner, not a shared helper directory.

## One versioned deployable

Every released version contains all modules. A deployment may run many replicas, worker processes, or
runtime roles, but each participating instance loads the module set required by that version and the
modules scale and roll back together. Local calls are allowed through injected public APIs, and
infrastructure may be shared physically. In-process events are visible only inside one instance unless
an external delivery mechanism is added. Logical ownership remains separate even when deployment and
resources are not.
