# ADR-009: Context Provisioning

**Status:** Accepted

## Context

The Chat Service needs contextual information (current zone, active itinerary, next booked attraction, language preference, etc.) to give useful answers. Alternatives considered:

1. Always fetch the latest context from the Backend and SaaS on every request.
2. Let the mobile app supply all context.
3. Hybrid approach.

Always fetching everything increases latency and load. Fully trusting the app opens the door to stale or manipulated data.

## Decision

Adopt a hybrid context model:

- The mobile app sends the context it currently knows with each chat request.
- The Chat Service treats app-supplied context as a **hint**.
- For any data that must be fresh, accurate or security-sensitive (live wait times, closures, entitlements, precise timing guidance), the Chat Service still fetches or validates the information from the Backend or SaaS.

## Consequences

**Positive**
- Lower average latency.
- Better behaviour under poor connectivity (app can use its local cache).
- Reduced load on Backend and SaaS for simple questions.

**Negative / Risks**
- App-supplied context can be stale or tampered with → must never be the sole source of truth for critical decisions.
- The API contract between app and Chat Service becomes richer and must be versioned.
- Developers must clearly document which fields are trusted hints versus authoritative server data.
