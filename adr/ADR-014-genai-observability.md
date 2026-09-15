# ADR-014: GenAI Observability Approach

**Status:** Accepted

## Context

Traditional software can be verified with deterministic tests. Generative AI outputs are non-deterministic, so classic pass/fail checks are insufficient. We need a way to detect when the chatbot starts misbehaving in production (quality regressions, increased hallucinations, safety issues, tool failures, etc.).

## Decision

Implement a multi-layered observability and evaluation strategy:

1. **Structured logging** of every interaction (user message, chosen intent + confidence, tools called and their results, prompts, raw and final responses, guardrail triggers, latency, guest context used).
2. **Runtime metrics and alerts** on leading indicators: guardrail trigger rate, fallback / “I don’t know” rate, tool-call error rate, latency percentiles, thumbs-down rate, intent-confidence distribution.
3. **Continuous evaluation** – maintain a golden set of real (anonymised) questions; periodically re-run the current system and score with automated metrics + LLM-as-judge + human sampling.
4. **User feedback loop** – simple 👍/👎 (and optional comment) in the app, feeding a review queue.
5. **Distributed tracing** (OpenTelemetry or equivalent) so any conversation can be fully reconstructed.

## Consequences

**Positive**
- Ability to detect quality or safety regressions quickly.
- Data-driven improvement of prompts, tools, routing examples and guardrails.
- Audit trail for investigations.

**Negative / Risks**
- Non-trivial engineering and process investment.
- Privacy and retention policies for conversation logs must be defined and enforced.
- Alert fatigue is possible if thresholds are not tuned carefully.
