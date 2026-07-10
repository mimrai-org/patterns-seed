# Keep applications free of package dependents

Deployable applications may depend on contract packages and reusable libraries, but no application or
library declares an application as a dependency. With graph arrows written as `consumer → dependency`,
an application can have outgoing edges and must have no incoming package-dependency edges. Collaboration
occurs through contract packages and runtime adapters. Direct app reuse was rejected because it couples
builds, frameworks, bootstrap code, and deployment assumptions. A reusable in-process capability must
be extracted under a distinct library responsibility rather than disguising an application as one.
