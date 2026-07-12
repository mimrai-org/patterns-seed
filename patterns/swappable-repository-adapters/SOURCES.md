# Research sources

This bundle is original, clean-room architecture guidance. No source code, prose, folder tree, example
domain, or diagram from the references was copied. The sources supplied implementation evidence and
terminology; this bundle narrows, reorganizes, and critiques those ideas independently.

Sources and licenses were reviewed on 2026-07-10:

- [`brocoders/nestjs-boilerplate` at `549cc37`](https://github.com/brocoders/nestjs-boilerplate/tree/549cc37a3925ab87a4e61b45efb3b86d2d8e234e),
  especially its [architecture documentation](https://github.com/brocoders/nestjs-boilerplate/blob/549cc37a3925ab87a4e61b45efb3b86d2d8e234e/docs/architecture.md)
  and parallel relational/document persistence directories. Consulted as practical evidence for a
  repository port, adapter-local persistence models and mappers, and selectable implementations. The
  project is [MIT licensed](https://github.com/brocoders/nestjs-boilerplate/blob/549cc37a3925ab87a4e61b45efb3b86d2d8e234e/LICENSE).
- [`ipfs/js-stores` at `58d2e06`](https://github.com/ipfs/js-stores/tree/58d2e06ef438f9aec0bf525eee9db5a7a0009c66),
  particularly its [datastore compliance-test package](https://github.com/ipfs/js-stores/tree/58d2e06ef438f9aec0bf525eee9db5a7a0009c66/packages/interface-datastore-tests).
  Consulted as primary implementation evidence for packaging one behavior suite and running it through
  setup and teardown fixtures supplied by different storage implementations. The repository is dual
  licensed under [Apache-2.0 or MIT](https://github.com/ipfs/js-stores/blob/58d2e06ef438f9aec0bf525eee9db5a7a0009c66/LICENSE).
- [`microsoft/TypeScript-Website` at `c8170c3`](https://github.com/microsoft/TypeScript-Website/tree/c8170c35bda4811c9516cbb69c39241ae4beb6d9),
  including the handbook sections on
  [structural typing](https://github.com/microsoft/TypeScript-Website/blob/c8170c35bda4811c9516cbb69c39241ae4beb6d9/packages/documentation/copy/en/handbook-v2/Everyday%20Types.md)
  and [type erasure](https://github.com/microsoft/TypeScript-Website/blob/c8170c35bda4811c9516cbb69c39241ae4beb6d9/packages/documentation/copy/en/handbook-v2/Basics.md).
  Consulted for the limitation that assignable TypeScript shapes do not verify runtime semantics. The
  documentation is [CC BY 4.0](https://github.com/microsoft/TypeScript-Website/blob/c8170c35bda4811c9516cbb69c39241ae4beb6d9/LICENSE),
  while repository code has a separate MIT license; neither is redistributed here.
- [`testcontainers/testcontainers-node` at `161268d`](https://github.com/testcontainers/testcontainers-node/tree/161268db89059e5f5406fc7027e9350f06b7d3ce).
  Consulted for disposable, representative external-resource fixtures used by adapter integration and
  contract suites. It is [MIT licensed](https://github.com/testcontainers/testcontainers-node/blob/161268db89059e5f5406fc7027e9350f06b7d3ce/LICENSE).
- Martin Fowler's original [Repository catalog entry](https://martinfowler.com/eaaCatalog/repository.html).
  Consulted for the repository's role as a mediator between domain-facing operations and data mapping.
  The page has no source revision or reuse license and remains copyrighted; it is linked only for
  conceptual provenance.

Framework dependency injection, ORMs, CQRS, event sourcing, universal CRUD repositories, and automatic
dual-write wrappers were deliberately excluded from the core pattern. They are separate choices and, in
the latter two cases, can obscure the narrow contract or data authority this pattern requires.
