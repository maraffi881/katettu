# ADR-008: Intent Routing Approach

**Status:** Accepted

## Context

The Chat Service must quickly decide whether a user message can be handled by a simple, deterministic tool path or whether it requires the full agentic LLM + tools loop. 
Alternatives considered:

1. Pure keyword / rule-based matching.
2. Full LLM-based classification on every message.
3. Embedding-based semantic similarity against example utterances.

Keyword matching is brittle. Calling a generative LLM for every routing decision adds latency and cost.

## Decision

Use an embedding-based Intent Router:

1. Define a focused set of high-value intents.
2. Maintain a set of example utterances for each intent.
3. Embed the examples (and the incoming user message) with the same embedding model.
4. Compute cosine similarity.
5. If the best match exceeds a confidence threshold → take the fast path for that intent.
6. Otherwise → fall back to the agentic path.

## Consequences

**Positive**
- Fast and inexpensive routing for the majority of common questions.
- Better semantic understanding than pure keywords.
- No generative LLM cost for the routing step itself.

**Negative / Risks**
- Quality depends on the quality and coverage of the example utterances.
- Thresholds must be tuned and periodically reviewed.
- Edge cases and novel phrasings will still fall through to the more expensive agentic path (which is intended).
