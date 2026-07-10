# Keep one versioned deployable with strong modules

The backend remains one versioned deployable and every runtime instance loads its required module set
while capability boundaries are enforced internally. Distributed services were rejected as the
default because they add network, consistency, deployment, and observability costs; the chosen model
cannot scale or release modules independently, even when the deployment runs many replicas or workers.
