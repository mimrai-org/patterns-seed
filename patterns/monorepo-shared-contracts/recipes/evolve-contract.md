# Evolve a contract

1. Inventory deployed producers and consumers, delayed messages, stored payloads, parser strictness,
   and the oldest supported representation.
2. Classify the change by runtime compatibility. Do not rely on an atomic repository typecheck to call
   a change safe.
3. Prefer an additive representation. When semantics conflict, introduce an explicit parallel version
   or discriminator rather than context-dependent parser behavior.
4. Produce synthetic old and candidate payload fixtures, including the exact candidate writer output
   that deployed readers will receive.
5. Execute the candidate reader against the oldest supported fixtures and execute the previous reader
   package, release artifact, or pinned consumer commit against candidate writer output. Record the
   exact old artifact; do not rebuild it against candidate source.
6. Release consumers capable of both representations before any producer emits only the new one.
7. Release producers, monitor parse failures and version usage, and preserve rollback compatibility.
8. Remove legacy emission only after all supported readers have migrated; remove legacy parsing in a
   later change after retained data and queued messages have expired.
9. Update the package change record and published semantic version when external distribution applies.

If applications always deploy atomically, the sequence may collapse, but compatibility fixtures still
protect stored, queued, cached, and rollback data.
