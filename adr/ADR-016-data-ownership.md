# ADR-016: Data Ownership / Systems of Record

**Status:** Accepted

## Context

Multiple systems hold related data about guests, tickets, itineraries, assets, sensors and gamification progress. Without clear ownership boundaries it becomes easy to create conflicting sources of truth, synchronisation bugs and unclear responsibilities for data quality and retention.

## Decision

Define explicit systems of record:

| Data Domain                        | System of Record          | Notes |
|------------------------------------|---------------------------|-------|
| Guest identity & profile           | Customer SaaS             | Master identity |
| Tickets, entitlements, memberships | Customer SaaS             | Commerce source of truth |
| Bookings & planned itinerary       | Customer SaaS             | Experience layer may cache |
| Asset master data & maintenance    | On-prem Backend           | |
| Sensor / IoT readings              | On-prem Backend           | |
| Real-time operational state (waits, closures, occupancy) | On-prem Backend | Published to experience layer |
| Gamification progress & collections| Custom (App + Backend)    | Can later sync summaries to SaaS |
| Chat conversation logs             | Chat Service / observability store | Subject to retention policy |
| Feedback scores & ratings          | Both (App captures, SaaS stores long-term) | |

Other systems may hold **copies or projections** of the data but must not become competing sources of truth. Updates flow from the system of record outward.

## Consequences

**Positive**
- Clear accountability for data quality and schema evolution.
- Reduced risk of inconsistent guest or operational state.
- Simpler reasoning about synchronisation and conflict resolution.

**Negative / Risks**
- Requires discipline; teams must resist the temptation to treat a local cache as authoritative.
- Some data (e.g. feedback, gamification) will still need well-defined synchronisation paths.
- Documentation of ownership must be kept up to date as the system evolves.
