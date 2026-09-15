# ADR-001: Customer-Facing Platform Choice

**Status:** Accepted

## Context

We need a system that supports discovery of attractions, itinerary planning, booking of visit times, ticket purchase, payments and guest identity management.

Alternatives considered:

1. Build the entire customer-facing stack ourselves.
2. Use a generic e-commerce platform (e.g. Shopify) with heavy customisation.
3. Adopt a vertical SaaS platform purpose-built for attractions, zoos, theme parks and similar venues.

Building everything would give maximum control but at high cost and risk. A generic e-commerce platform handles payments well but lacks native support for timed entry, capacity management, memberships, access control and attraction-specific workflows.

## Decision

Use a vertical attractions SaaS platform (examples: Accesso, RocketRez, or equivalent) as the system of record for commerce, guest identity and pre-visit planning.

## Consequences

**Positive**
- Significantly faster time-to-market.
- Lower risk around payments, PCI, capacity management and membership logic.
- Clear separation of concerns between commerce and the live experience layer.

**Negative / Risks**
- Vendor lock-in and dependency on the vendor’s API quality, roadmap and pricing.
- Some differentiating experience features must still be built outside the SaaS.
- Integration effort is required to connect the SaaS with the on-premises backend and the custom mobile apps.
