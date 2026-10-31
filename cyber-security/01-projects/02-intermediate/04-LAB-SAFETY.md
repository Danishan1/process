# Lab Safety and Ethical Boundaries

1. Test only owned systems or systems covered by explicit, documented authorization.
2. Keep intentionally vulnerable services on host-only or isolated virtual networks; do not expose them to the public internet.
3. Configure target allowlists, rate limits, timeouts, and an emergency stop for active tests.
4. Use synthetic identities, dummy credentials, fake email samples, and sanitized logs.
5. Use least-privilege credentials; prefer read-only auditing whenever possible.
6. Keep cloud spend controls and cleanup scripts; avoid unintentionally public cloud resources.
7. Back up VM snapshots before experiments and document restoration procedures.
8. Never publish secrets, customer data, or sensitive third-party findings.
9. Separate demonstration of a vulnerability from instructions for misuse; document remediation and detection.
10. Verify tool outputs; a scan finding is not proof of exploitability, and an absent alert is not proof of safety.

## Scope authorization template
- System owner:
- Authorized lab assets / CIDRs:
- Allowed test types:
- Explicit exclusions:
- Allowed dates and rate limits:
- Data handling and retention:
- Contact / stop procedure:

For a personal home lab, record your own ownership and the network isolation design.
