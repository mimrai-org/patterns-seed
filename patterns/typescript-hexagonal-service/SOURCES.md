# Research sources

This bundle is an original, framework-neutral synthesis. No source code, diagram, example domain,
folder tree, or prose from the references was copied.

Sources were reviewed on 2026-07-10:

- [Alistair Cockburn, “Hexagonal Architecture”](https://alistair.cockburn.us/hexagonal-architecture/).
  Consulted for the original inside/outside distinction and driving/driven adapter model. The article
  states that its content is all rights reserved; it is linked for conceptual provenance only.
- [`Sairyss/domain-driven-hexagon` at `5c2d15a`](https://github.com/Sairyss/domain-driven-hexagon/tree/5c2d15a7e2d69e83dfddf28468ee9f30e02c30de).
  Consulted for inward dependencies, application-owned capabilities, and explicit warnings about
  unnecessary complexity. The project is [MIT licensed](https://github.com/Sairyss/domain-driven-hexagon/blob/5c2d15a7e2d69e83dfddf28468ee9f30e02c30de/LICENSE).
- [`brocoders/nestjs-boilerplate` at `549cc37`](https://github.com/brocoders/nestjs-boilerplate/tree/549cc37a3925ab87a4e61b45efb3b86d2d8e234e)
  and its [architecture documentation](https://github.com/brocoders/nestjs-boilerplate/blob/549cc37a3925ab87a4e61b45efb3b86d2d8e234e/docs/architecture.md).
  Consulted for practical persistence ports, replaceable adapters, and boundary mappers. The project
  is [MIT licensed](https://github.com/brocoders/nestjs-boilerplate/blob/549cc37a3925ab87a4e61b45efb3b86d2d8e234e/LICENSE).

CQRS, event sourcing, tactical DDD, dependency-injection frameworks, ORMs, and message brokers were
deliberately excluded from the core contract because they are independent decisions.
