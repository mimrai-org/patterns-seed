# patterns-seed — repository instructions

This repository contains publishable `patterns` bundles under `patterns/<slug>/`.

When adding or changing a bundle:

1. Read its `patterns.yaml` first and open only the indexed document relevant to the task.
2. Keep the bundle self-contained. It must not depend on root-level or sibling documentation.
3. Keep a bundle-local `LICENSE` with the repository's CC BY 4.0 notice so installation preserves it.
4. Preserve `scope: shareable`; use roles and placeholders instead of business-specific names.
5. Keep the directory name and manifest `name` identical and version each bundle independently.
6. Update the rich index whenever a document is added, renamed, or removed.
7. Express hard import prohibitions in `boundaries` when the glob model can represent them. Record
   finer module-identity rules in prose and name the enforcement mechanism. In monorepos, qualify a
   repeated local slug with its owning source, package, application, or module root.
8. Validate the bundle with the `patterns` CLI and test a local installation before considering it
   publishable.

Do not add application source, generators, copied starter files, credentials, or vendor-specific
business examples. References belong in each bundle's `SOURCES.md` and must distinguish inspiration
from copied material; the default is clean-room, original prose with no copied code.
