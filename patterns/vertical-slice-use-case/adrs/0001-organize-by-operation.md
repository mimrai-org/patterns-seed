# Organize by operation

The primary code boundary is one request, job, or message outcome, with its transport-to-effect path
colocated under one owner. Global controller, service, and repository layers were rejected because
they couple unrelated changes through broad abstractions; operation folders can duplicate small code
and require active refactoring, but they make the unit of change and behavior visible.
