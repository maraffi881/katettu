# ADR-011: LLM Hosting Preference

**Status:** Accepted

## Context

The chatbot requires a capable large language model. Alternatives considered:

1. Fully self-hosted / on-premises LLM.
2. External LLM hosted outside the EU (typical US providers).
3. EU-hosted managed LLM services (Mistral La Plateforme with EU endpoint, Scaleway Managed Inference, etc.).

Fully on-premises gives maximum control and offline capability but requires significant GPU capacity, MLOps effort and model-update processes. Non-EU hosting raises data-residency and regulatory concerns.

## Decision

Prefer a strong EU-hosted managed LLM (Mistral with the EU regional endpoint or Scaleway Managed Inference) as the primary inference path.

Keep the architectural option of a smaller local model as a degraded-mode fallback so basic functionality remains available if the external service or the internet link is unavailable.

## Consequences

**Positive**
- Strong GDPR and data-residency posture.
- Access to high-quality models without operating large GPU clusters day-to-day.
- Managed scaling and model updates.

**Negative / Risks**
- Primary path still depends on network connectivity and the external provider’s availability.
- Local fallback requires additional capacity planning and model management if activated.
- Contractual and DPA review of the chosen provider remains necessary.
