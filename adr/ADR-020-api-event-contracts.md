# ADR-020: API & Event Contract Management

**Status:** Accepted

## Context

The Customer SaaS, the on-premises Backend, the Experience API and the Chat Service exchange data through APIs and events. Without explicit contract rules, changes on one side easily break the others.

## Decision

Adopt the following contract management practices:

1. **Published contracts** – All cross-system APIs are described with OpenAPI (or equivalent). Events are described with AsyncAPI or a CloudEvents-based schema registry.
2. **Versioning** – Breaking changes require a new major version. Consumers are given a deprecation window before old versions are removed.
3. **Ownership** – Each contract has a clear owning team/system (usually the provider of the API or the publisher of the event).
4. **Compatibility** – Prefer additive, backward-compatible changes. Consumers must ignore unknown fields.
5. **Validation** – Providers validate incoming requests against the contract; consumers should treat responses defensively.
6. **Documentation & discovery** – Contracts live in a shared, version-controlled location accessible to all integrating teams.
7. **Idempotency & correlation** – Commands and events carry idempotency keys and correlation IDs to support safe retries and tracing.

Direct SaaS ↔ Backend integrations and the interfaces used by the Chat Service and BFF all fall under these rules.

## Consequences

**Positive**
- Independent evolution of systems becomes safer.
- Integration bugs caused by silent contract drift are reduced.
- New consumers can discover and understand interfaces more easily.

**Negative / Risks**
- Requires process discipline and tooling (schema registry, linting, contract tests).
- Version proliferation must be actively managed.
- Some vendor SaaS APIs may not fully support the desired versioning model; adapters or anti-corruption layers may be needed.
