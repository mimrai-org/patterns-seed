# Research sources

This bundle is an original, framework-neutral synthesis. No source code, diagram, example domain,
configuration, or prose from the references was copied.

Sources were reviewed on 2026-07-10:

- [`kgrzybek/modular-monolith-with-ddd` at `91c8ef2`](https://github.com/kgrzybek/modular-monolith-with-ddd/tree/91c8ef24b4cb6ef558c95d8267fa07d68c7059f8).
  Consulted for module facades, module-owned data, per-module composition, integration contracts, and
  architecture tests. Its asynchronous-only communication policy was treated as one strict variant,
  not copied into this generic pattern. The project is [MIT licensed](https://github.com/kgrzybek/modular-monolith-with-ddd/blob/91c8ef24b4cb6ef558c95d8267fa07d68c7059f8/LICENSE).
- [Microsoft guidance on common web application architectures at `e03d3b4`](https://github.com/dotnet/docs/blob/e03d3b47f45f8becd0226b21e6a367f967886b83/docs/architecture/modern-web-apps-azure/common-web-application-architectures.md).
  Consulted for the operational trade-offs of a single deployment with internal components. The
  documentation is [CC BY 4.0](https://github.com/dotnet/docs/blob/e03d3b47f45f8becd0226b21e6a367f967886b83/LICENSE);
  no content was reproduced.
- [Node.js package entry points at `4140d4c`](https://github.com/nodejs/node/blob/4140d4c2c8edf6c773603663a3684efac486ce63/doc/api/packages.md#package-entry-points).
  Consulted for package `exports` as an optional enforcement mechanism when modules are workspace
  packages. Node.js is covered by its
  [project license](https://github.com/nodejs/node/blob/4140d4c2c8edf6c773603663a3684efac486ce63/LICENSE).
- [`Nx` at `03483ea`](https://github.com/nrwl/nx/tree/03483ea23b4b36feb80d3b044591f087c2d19f00)
  and its [module-boundary guidance](https://nx.dev/docs/features/enforce-module-boundaries).
  Consulted for module-aware dependency constraints, public API checks, and cycle detection. Nx is
  [MIT licensed](https://github.com/nrwl/nx/blob/03483ea23b4b36feb80d3b044591f087c2d19f00/LICENSE).

DDD, CQRS, event sourcing, outbox/inbox delivery, message brokers, and framework modules remain optional
variants. They are not required by this pattern.
