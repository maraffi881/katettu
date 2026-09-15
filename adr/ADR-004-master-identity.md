# ADR-004: Master Identity

**Status:** Accepted

## Context

Guests must have a consistent identity across ticket purchase, the mobile app, the chatbot and any post-visit services. Alternatives considered:

1. Build and operate a custom identity service.
2. Treat the customer SaaS as the master identity provider.
3. Introduce a separate identity broker / IdP in front of both the SaaS and internal systems.

A custom identity service would duplicate functionality the SaaS already provides and create synchronisation problems. A full broker adds complexity that is not justified at the current stage.

## Decision

The customer-facing SaaS is the master identity provider.

- The mobile app authenticates using OAuth 2.0 / OpenID Connect (Authorization Code flow with PKCE) against the SaaS.
- The BFF validates the SaaS-issued tokens and maps the guest identifier into an internal context.
- Backend services never receive the original SaaS credentials; they receive only a trusted internal token or mapped claims.

## Consequences

**Positive**
- Single source of truth for guest identity, tickets and entitlements.
- No duplicated login, password reset or MFA logic.
- Guests have a consistent account experience.

**Negative / Risks**
- Dependency on the SaaS identity features, token lifetimes and session behaviour.
- The BFF becomes a critical trust and mapping boundary that must be hardened.
- If the SaaS identity model is limited, workarounds may be required later.
