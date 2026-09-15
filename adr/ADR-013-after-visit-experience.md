# ADR-013: After-Visit Experience Ownership

**Status:** Accepted

## Context

After a visit, guests should be able to see a personal summary, revisit memories, give feedback, and be encouraged to return. These capabilities could live entirely in the custom mobile app, entirely in the customer SaaS, or be split.

## Decision

Split ownership according to the strength of each system:

- **Custom mobile app** – immediate, personal after-visit experience: visit timeline/summary, collected animals/plants, photos/memories, quick contextual feedback.
- **Customer SaaS** – longer-term relationship: loyalty, membership status, re-booking, marketing automation, email/push campaigns, historical guest profile.

Key signals (feedback scores, completed challenges, highlight moments) are synchronised from the app to the SaaS so the guest profile remains complete.

## Consequences

**Positive**
- Best possible UX for the emotional “end of day” moment inside the app.
- Leverages the SaaS strengths in CRM, loyalty and commerce.
- Avoids forcing the SaaS to become a full experience platform.

**Negative / Risks**
- Requires a reliable data hand-off between the app and the SaaS.
- Risk of inconsistent history if synchronisation fails or is delayed.
- Two places where “after-visit” logic exists → documentation and ownership must stay clear.
