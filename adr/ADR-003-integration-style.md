# ADR-003: Integration Style Between SaaS and Backend

**Status:** Accepted

## Context

The customer-facing SaaS and the on-premises backend must exchange several types of information:

- Guest itineraries and bookings (SaaS → Backend)
- Capacity changes, maintenance windows and closures (Backend → SaaS / experience layer)
- Real-time status, travel times and occupancy estimates
- Guest context needed for guidance and the chatbot

Alternatives considered:

1. Pure point-to-point synchronous REST calls.
2. Fully event-driven architecture.
3. Hybrid of synchronous APIs and asynchronous events.

Pure REST creates tight coupling and can suffer under load or partial failures. A pure event-driven approach is excellent for decoupling but makes request-response use cases (e.g. “what is the current recommendation for this guest?”) more awkward.

## Decision

Adopt a hybrid API-led + event-driven integration pattern:

- Synchronous REST or GraphQL for on-demand queries and commands.
- Signed webhooks and an internal event backbone for state-change notifications.
- Prefer outbound connections originating from the on-premises side where possible.
- Use versioned contracts (OpenAPI / AsyncAPI) and CloudEvents where appropriate.

## Consequences

**Positive**
- Loose coupling between the two major systems.
- Supports both real-time push updates and on-demand queries.
- Better resilience and independent evolution of each side.

**Negative / Risks**
- More moving parts (webhooks, event bus, contract versioning, idempotency).
- Requires disciplined API and event design.
- Debugging distributed flows is more complex than a single monolithic call stack.
