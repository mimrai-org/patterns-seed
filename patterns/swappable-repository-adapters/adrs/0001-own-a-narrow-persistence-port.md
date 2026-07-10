# Own a narrow persistence port

The consuming application owns one repository contract shaped around a cohesive persistence capability.
A universal `Repository<T>` and an infrastructure-owned CRUD interface were rejected because they expose
storage capabilities to policy, combine unrelated read and write needs, and grow into an accidental query
language. Direct data access remains the simpler choice when no meaningful volatility or test boundary
exists. The trade-off is that several capabilities may define several small ports over the same store.
