# ADR-005: Mobile App Scope

**Status:** Accepted

## Context

Visitors need a rich experience while they are inside the park: live guidance, real-time information next to attractions, an AI chatbot, gamification, notifications and the ability to save memories. Alternatives considered:

1. Rely primarily on the SaaS vendor’s white-label or web app.
2. Build a full custom app that also owns the complete commerce and booking flow.
3. Build a custom app focused on the during-visit experience while leaving commerce in the SaaS.

Option 1 would limit differentiation. Option 2 would duplicate complex and regulated commerce functionality.

## Decision

Build custom native (or high-quality cross-platform) iOS and Android applications whose primary responsibility is the during-visit experience.

- Discovery, complex planning, booking and payments remain in the SaaS.
- The app can still offer simple ticket purchases or upgrades by calling SaaS APIs or opening a secure checkout experience.
- The app authenticates against the SaaS identity and pulls the guest’s tickets and itinerary.

## Consequences

**Positive**
- Excellent, park-specific user experience for the differentiating features.
- Clear ownership boundaries.
- Ability to iterate quickly on chatbot, guidance and gamification.

**Negative / Risks**
- Need to maintain mobile applications (or a cross-platform codebase).
- Ticket and itinerary data must stay consistent between the app and the SaaS.
- Some users may still prefer the SaaS web experience for pre-visit planning.
