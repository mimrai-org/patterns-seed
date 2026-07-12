# Application owns outbound ports

Outbound contracts are defined by the application code that consumes them, following dependency
inversion and interface segregation. Infrastructure-owned generic interfaces were rejected because
they expose tool capabilities to policy and force consumers to depend on methods they do not need.
