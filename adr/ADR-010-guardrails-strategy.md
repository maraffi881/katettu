# ADR-010: Guardrails Strategy

**Status:** Accepted

## Context

A public-facing chatbot is exposed to prompt injection, jailbreak attempts, abusive content, attempts to extract system information, and the risk of generating harmful, biased or privacy-violating answers.

Relying only on the base LLM’s built-in safety is insufficient. A pure post-generation filter is also too late for many attacks.

## Decision

Implement defence-in-depth guardrails on both sides of the LLM:

1. **Input guardrails** (executed before the Intent Router)  
   Detect and block prompt injection, jailbreaks, clear policy violations and abusive content.

2. **Output guardrails** (executed after the LLM response)  
   Check for PII leakage, toxic language, policy violations and obvious low-quality or hallucinated answers.

Prefer a layered implementation that combines:
- Fast open-source scanners (e.g. LLM Guard, Presidio for PII)
- Stronger specialised classifiers (e.g. Llama Guard) where needed
- Optional LLM-as-judge checks for borderline cases

## Consequences

**Positive**
- Early rejection of malicious or out-of-policy input saves cost and reduces risk.
- Multiple independent checks improve overall safety.
- Open-source components allow self-hosting and data-residency control.

**Negative / Risks**
- Additional latency on every request.
- False positives can frustrate legitimate users → thresholds and allow-lists need ongoing tuning.
- Operational overhead of keeping scanners and models up to date.
