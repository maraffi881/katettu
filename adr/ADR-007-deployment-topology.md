# ADR-007: Overall Deployment Topology

**Status:** Accepted

## Context

The major runtime components are the BFF, the Chat Service, the on-premises Backend and the API Gateway that protects the Backend. We needed a clear decision on where each component runs.

## Decision

Adopt the following topology:

- **BFF and Chat Service** → Cloud (preferably the same region and cluster for low internal latency).
- **Backend and API Gateway** → On-premises (or private cloud next to the park).
- Connectivity between the two environments via secure private mechanisms (site-to-site VPN, private link, Cloudflare Tunnel, mTLS reverse tunnel, etc.). Prefer outbound connections from on-premises where feasible.

## Consequences

**Positive**
- Optimal latency for mobile users (cloud) and for sensors/operations (on-premises).
- Strong security boundary around the park’s operational systems.
- Ability to keep the live experience available even if the internet link is degraded (with graceful degradation).

**Negative / Risks**
- Requires robust, monitored and redundant connectivity between cloud and on-premises.
- Two distinct environments must be operated and observed.
- Network partitions must be designed for (caching, fallbacks, circuit breakers).
