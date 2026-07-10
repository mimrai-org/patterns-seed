# Consume a contract

1. Add the contract package to the consuming application's own manifest using the package manager's
   explicit local-workspace syntax; update the lockfile.
2. Import only the required declared subpath. Do not point an alias or relative path at package source.
3. At an inbound runtime boundary, receive `unknown`, parse with the exported contract, and translate a
   failure into the application's explicit boundary behavior.
4. Map the parsed contract value into an application-owned model before business logic, storage, or UI
   state uses it.
5. At an outbound boundary, map application state to the contract representation and validate it when
   untyped sources or compatibility risk justify the check.
6. Add a consumer test using public contract exports. Keep consumer-specific fixtures local; when the
   contract intentionally shares a synthetic corpus, import only its documented
   `./testing/<contract>` subpath and never deep-import the package's `tests/` directory.
7. Run the application's typecheck, build, tests, import-policy check, and the workspace graph check.

Do not import the producer application to reuse a client, router, handler, or convenience type. Build
the application's adapter around the shared contract or adopt a separate generated client package with
its own responsibility.
