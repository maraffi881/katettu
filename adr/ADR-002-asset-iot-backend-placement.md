# ADR-002: Asset & IoT Backend Placement

**Status:** Accepted

## Context

The system must ingest data from LoRa/LoRaWAN sensors, maintain asset master data, support scheduled maintenance, generate alerts, produce reports and run predictive models. It also needs to supply real-time operational state (wait times, closures, travel-time estimates, animal/plant status) to the customer-facing experience.

Alternatives considered:

1. Fully cloud-hosted backend.
2. Fully on-premises backend.
3. Hybrid (core operational systems on-premises, analytics in the cloud).

A pure cloud approach would simplify operations but introduces latency to sensors, potential availability issues if the park loses internet connectivity, and weaker control over sensitive operational and animal/plant data.

## Decision

Deploy the asset, IoT and operations backend on-premises (or in a private cloud physically close to the park). Place an API Gateway in front of it as the controlled entry point.

## Consequences

**Positive**
- Lowest possible latency to sensors and local systems.
- High availability even when the public internet connection is degraded or lost.
- Stronger data sovereignty and control over operational data.
- Clear security boundary around the park’s critical systems.

**Negative / Risks**
- Requires secure and reliable connectivity to the cloud-hosted BFF and Chat Service.
- The organisation retains more operational responsibility for the on-premises infrastructure.
- Disaster recovery and backup strategies must be designed for the on-prem environment.
