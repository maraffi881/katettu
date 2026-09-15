# ADR-015: Chatbot Control Flow

**Status:** Accepted

## Context

Not every user message requires a full multi-step agentic reasoning loop. Many common park questions can be answered faster and more cheaply with a deterministic tool path followed by light natural-language formulation. At the same time the system must still handle open-ended and complex questions.

## Decision

Adopt a two-path control flow inside the Chat Service:

1. **Fast path**  
   Intent Router selects a known high-confidence intent → specific tools are called directly → LLM is used only to turn the structured results into a friendly natural-language answer.

2. **Agentic path**  
   Low-confidence or complex queries → LLM is given a set of tool definitions and may call tools in a bounded loop (strict maximum number of rounds) → final answer is generated from the accumulated observations.

Both paths are protected by the input and output guardrails defined in [ADR-010](./adr/ADR-010-guardrails-strategy.md). Rich context (itinerary, zone, etc.) is injected on every request (see  [ADR-009](./adr/ADR-009-context-provisioning.md).

## Consequences

**Positive**
- Good average latency and cost profile (most questions take the fast path).
- Still capable of handling novel or multi-step requests.
- Clear extension point: new intents and tools can be added incrementally.

**Negative / Risks**
- Requires ongoing maintenance of the intent catalogue, example utterances and tool definitions.
- Agent loops must be strictly limited and observed to prevent runaway cost or latency.
- The boundary between “fast” and “agentic” will need periodic review as real usage data arrives.
