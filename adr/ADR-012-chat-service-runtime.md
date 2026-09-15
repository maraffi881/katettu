# ADR-012: Chat Service Runtime

**Status:** Accepted

## Context

The Chat Service (and the BFF) need a production runtime in the cloud. Alternatives considered:

1. Classic virtual machines.
2. Serverless containers (Cloud Run, Azure Container Apps, AWS App Runner, etc.).
3. Kubernetes (managed or self-managed).

Virtual machines are simple but scale poorly and have higher idle cost. Serverless containers offer excellent scaling-to-zero but can introduce cold-start latency and give less control over networking to the on-premises backend.

## Decision

Deploy the Chat Service and the BFF on managed Kubernetes (GKE Autopilot, AKS, EKS Auto Mode / Fargate, or equivalent).

Run the Chat Service as a Deployment with multiple replicas, horizontal pod autoscaling, readiness/liveness probes and appropriate resource requests/limits.

## Consequences

**Positive**
- Fine-grained control over scaling, rolling updates and co-location with the BFF.
- Mature networking, service-mesh and observability options.
- Suitable for a latency-sensitive conversational workload that also talks to on-premises systems.

**Negative / Risks**
- Higher operational complexity than pure serverless.
- The team must be comfortable with Kubernetes concepts (or use a highly managed offering that reduces the burden).
- Resource configuration and autoscaling policies need careful tuning.
