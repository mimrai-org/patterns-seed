# Slice cohesion and ownership

One slice owns one externally observable application outcome. Its name should complete a sentence such
as “the system can `<operation>`,” and its tests should describe that same result. Transport, mapping,
validation, orchestration, required persistence, and provider integration remain close because they
usually change together for that outcome.

Do not define slices as broad resources with generic create, read, update, and delete services when
those requests have different policy or consumers. Do not define them as technical actions such as
“run repository” or “send HTTP.” Ownership follows application intent.

Split an operation when it contains outcomes that can be released, authorized, retried, or changed
independently. Merge slices when nearly every change crosses them, they exchange mutable internals, or
they cannot state separate contracts without duplicating the same invariant. File count alone is not a
boundary signal.

Each slice may use the simplest internal model that protects its behavior. A transaction script, pure
functions, a state machine, or a rich domain model can coexist in different slices. Refactor when
complexity appears; do not mandate the most elaborate option application-wide.

An operation has one accountable owner even when several teams contribute. Changes to shared runtime
facilities do not transfer ownership of the use-case contract.
