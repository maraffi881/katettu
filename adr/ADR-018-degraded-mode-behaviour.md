# ADR-018: Degraded / Offline Mode Behaviour

**Status:** Accepted

## Context

The park may experience loss or severe degradation of the internet link between the on-premises environment and the cloud. The system must still provide a useful (if reduced) experience to visitors and staff rather than failing completely.

## Decision

Define explicit degraded-mode behaviour:

**When cloud ↔ on-prem connectivity is lost or severely degraded:**

- Mobile app continues to work with **cached** itinerary, maps, static attraction information and last-known state.
- Guidance that depends on live Backend data (precise “leave now” timing, live wait times, animal activity) becomes best-effort or is clearly marked as potentially stale.
- Chat Service falls back to a local/smaller model (if deployed) or to a limited set of answers that do not require live Backend calls; otherwise it returns a graceful “limited connectivity” message.
- Core on-premises Backend functions (sensor ingestion, local alerts, staff tools) continue to operate independently.
- New ticket purchases and complex booking changes that require the SaaS may be queued or deferred until connectivity returns.
- The app surfaces a clear indicator to the user that some live features are temporarily limited.

**Design principles**
- Prefer graceful degradation over hard failure.
- Cache aggressively on the client for read-mostly data.
- Never leave the user with a completely blank or broken experience if local data exists.

## Consequences

**Positive**
- Visitors and staff retain useful functionality during network problems.
- Operational systems on-site remain available.
- Clear user communication reduces frustration.

**Negative / Risks**
- Caching and stale-data handling must be carefully designed and tested.
- Some features (live guidance, full chatbot power, new purchases) will be impaired.
- Reconciliation logic is needed when connectivity is restored (queued actions, cache invalidation).
