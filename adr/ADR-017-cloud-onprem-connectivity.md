# ADR-017: Cloud ↔ On-Prem Connectivity & Security

**Status:** Accepted

## Context

The cloud-hosted Experience API and Chat Service must communicate securely and reliably with the on-premises Backend and its API Gateway. Alternatives include public internet with mutual TLS, site-to-site VPN, cloud private links, reverse tunnels, or a combination.

## Decision

Establish a private, encrypted connectivity path between the cloud environment and the on-premises network. Preferred options (in order of preference where available):

1. Cloud provider private link / Direct Connect / ExpressRoute equivalent, or
2. Site-to-site VPN, or
3. Secure reverse tunnel (e.g. Cloudflare Tunnel, Tailscale, or similar) originating from on-premises.

Additional mandatory controls:

- Mutual TLS (mTLS) between services wherever practical.
- Short-lived credentials / workload identities rather than long-lived static keys.
- Least-privilege service accounts and network policies.
- The on-premises API Gateway remains the single controlled entry point into the Backend.
- Prefer outbound-initiated connections from on-premises when the technology allows it.

## Consequences

**Positive**
- Traffic does not traverse the public internet in clear text.
- Strong authentication and authorisation between the two environments.
- On-premises systems are not directly exposed to the public internet.

**Negative / Risks**
- Connectivity becomes a critical dependency; redundancy and monitoring are required.
- Certificate and identity management adds operational overhead.
- Network partitions must be designed for (see also ADR-018).
