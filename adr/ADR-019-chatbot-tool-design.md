# ADR-019: Chatbot Tool Design Principles

**Status:** Accepted

## Context

The agentic path of the Chat Service relies on tools (functions) that the LLM can call. Poorly designed tools lead to unreliable behaviour, security issues, excessive latency and difficult maintenance.

## Decision

All tools exposed to the LLM must follow these principles:

1. **Single responsibility** – each tool does one clear job (e.g. `get_wait_time`, `get_animal_status`, `get_itinerary_summary`).
2. **Explicit and stable contracts** – well-defined input schema, output schema and error model. Tools are versioned.
3. **Least privilege** – a tool can only access the data and actions it genuinely needs. Guest context is passed in; tools do not receive raw SaaS tokens.
4. **Idempotent where possible** – safe to retry.
5. **Fast and bounded** – strict timeouts; no long-running operations inside a tool call.
6. **Observable** – every invocation is logged with parameters (redacted where necessary), result status and latency.
7. **Fail clearly** – structured error responses that the LLM (and the orchestrator) can understand and recover from.
8. **No side effects unless explicitly intended** – read tools are side-effect free; write tools are clearly named and protected by additional authorisation.

The set of available tools is curated and reviewed; the LLM is not given unrestricted access to internal APIs.

## Consequences

**Positive**
- More reliable and predictable agent behaviour.
- Easier security review and auditing.
- Tools can be tested and evolved independently of prompts.

**Negative / Risks**
- Up-front design discipline is required.
- Adding a new capability means defining and reviewing a new tool rather than just changing a prompt.
- Overly fine-grained tools can increase the number of LLM round-trips; overly coarse tools reduce flexibility.
