# ADR-006: Chat Service Placement

**Status:** Accepted

## Context

The conversational AI component (intent routing, tool orchestration, LLM calls, guardrails) could be implemented in several places:

1. Inside the BFF.
2. Inside the on-premises backend.
3. As a dedicated Chat Service.

Putting it inside the BFF would make the BFF fat and harder to scale independently. Putting it inside the on-premises backend would mix a high-churn, user-facing conversational component with stable operational systems and would force guest-context data deeper into the asset domain.

## Decision

Implement the Chat Service as a dedicated service that runs next to the BFF in the cloud.

Responsibilities of the Chat Service:
- Intent routing
- Fast-path vs agentic-path decision
- Tool execution (calling Backend and SaaS as needed)
- LLM interaction
- Input and output guardrails
- Returning a clean answer plus optional UI hints to the BFF

## Consequences

**Positive**
- Clear separation of concerns.
- Independent deployment and scaling of conversational logic.
- Easier evolution of prompts, tools and routing without touching core systems.

**Negative / Risks**
- Additional service to operate and monitor.
- Extra network hop compared with embedding the logic directly in the BFF.
- The tool interface between Chat Service, Backend and SaaS must be carefully designed and versioned.
