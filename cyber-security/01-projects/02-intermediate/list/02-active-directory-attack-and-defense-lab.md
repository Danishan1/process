# Project 02 — Active Directory Attack and Defense Lab

**Track:** Windows / Red + Blue  
**Target level:** Intermediate (project 02, 04, 13 and 20 may become advanced)  
**Environment:** Owned or explicitly authorized, isolated lab only

## 1. Executive summary
Build an isolated domain and demonstrate safe misconfiguration discovery, event collection, and remediation without attacking external systems.

## 2. Problem statement — what is broken?
Identity misconfigurations in Windows domains create exposure that beginners rarely understand beyond isolated commands.

### Why this matters
The underlying workflow either lacks visibility, repeatability, consistent evidence, or a reliable way to validate fixes. In a real organization, that creates delays, inconsistent decisions, and security blind spots. This project explores the problem in a controlled environment; it is not a claim of production readiness.

## 3. Proposed solution and value
Build an isolated domain and demonstrate safe misconfiguration discovery, event collection, and remediation without attacking external systems.

**Expected outcome:** A demonstrable workflow that turns raw observations into testable findings, actionable recommendations, and reproducible evidence.

## 4. Concepts used
- Ad ds
- Dns
- Ldap
- Kerberos
- Ntlm
- Gpo
- Permissions
- Windows event ids
- Least privilege

## 5. Revise these concepts before starting
- Windows users/groups, dns, ip networking, virtualization, powershell, authentication

**Readiness check:** Explain each prerequisite in your own words and demonstrate one small hands-on exercise before implementing the full project.

## 6. Technology stack
- Windows server evaluation vm
- Windows client
- Powershell
- Sysmon
- Wazuh
- Virtualbox/vmware

**Selection note:** These are suggested technologies, not mandatory dependencies. Choose the smallest stack that supports a reliable MVP.

## 7. Functional requirements / MVP scope
- Domain controller and clients
- Users/ous/groups
- Baseline gpos
- Sample share permissions
- Event forwarding
- Controlled weak-versus-hardened configuration comparison

## 8. Suggested architecture and workflow
1. Define the owned lab assets, data sources, users, and allowed operations.
2. Capture or ingest representative sample data.
3. Normalize and validate inputs; handle missing or malformed data.
4. Run the analysis, checks, or tests described in the MVP.
5. Store results with timestamps, source references, and reproducible evidence.
6. Produce a human-readable finding, dashboard, or report.
7. Apply a safe remediation or configuration change and repeat the test.

**Design decisions to document:** boundaries and assumptions; data schema; error handling; permissions; privacy; logging; resource limits; what the system does *not* detect.

## 9. Practical build phases
**Phase A — Foundation:** Draw architecture, define threat model and scope, prepare isolated test assets, and record baseline behavior.

**Phase B — Working prototype:** Implement the simplest end-to-end flow with one input, one analysis path, and one report.

**Phase C — Intermediate depth:** Add the remaining MVP features, modular components, validation, structured errors, and reproducible setup.

**Phase D — Security and quality:** Add unit/integration tests, negative tests, least-privilege defaults, sanitization, and clear failure behavior.

**Phase E — Portfolio:** Record a demo, publish safe synthetic data, write a technical case study, and explain limitations and improvements.

## 10. Real-world use cases
- Enterprise identity administration
- Access reviews
- Soc identity investigations

**Real-world adoption conditions:** Obtain written authorization and stakeholder approval; confirm legal and organizational policy requirements; assess data sensitivity; test at small scale; establish ownership, monitoring, and rollback before any production deployment.

## 11. Related problems and modifications
- Multi-domain trust simulation
- Automated policy audit
- Identity-risk dashboard

Additional transferable patterns:
- **Manual-to-repeatable workflow:** Turn a checklist into an auditable tool with consistent outputs.
- **Single-host to multi-host:** Add scoped inventory, concurrency limits, ownership, and result aggregation.
- **One-time assessment to continuous monitoring:** Add scheduling, historical baselines, and change alerts.
- **Technical result to business action:** Add prioritization, accountable owner, and remediation verification.

## 12. Testing and measurable success criteria
- Verify policy and share access before/after changes
- Confirm expected events arrive in siem

Also demonstrate: clean setup on a fresh machine; input validation; at least three negative/error tests; reproducible results; documented false positives/false negatives where applicable.

## 13. Risks, boundaries, and limitations
- Operate only on machines, applications, networks, or cloud accounts you own or have explicit permission to assess.
- Keep deliberately vulnerable targets isolated; never expose them to the public internet.
- Do not collect real credentials, private user data, or confidential organization information for your public portfolio.
- Prefer read-only checks and safe simulations; require explicit confirmation for any state-changing test.
- Avoid presenting prototype results as comprehensive security assurance.

## 14. GitHub portfolio deliverables
- Domain diagram, setup scripts, event timelines, configuration comparison, defensive checklist

Repository should also include: `README.md`, `docs/architecture.md`, `docs/threat-model.md`, `docs/findings.md`, `tests/`, `.gitignore`, `LICENSE` (if desired), setup and cleanup instructions, and a short demo script.

## 15. Interview discussion prompts
1. What exact problem does this solve, and who experiences it?
2. What assumptions did you make about the environment?
3. Why did you select this architecture and stack?
4. How did you verify the tool or detection was correct?
5. Which edge cases, false positives, or failures did you discover?
6. What would you change for a production environment?
7. What did you learn from fixing a defect or misconfiguration?

## 16. Definition of done
- [ ] Problem, scope, and threat model written
- [ ] Isolated lab and sample data documented
- [ ] MVP functions end-to-end
- [ ] Positive and negative tests pass
- [ ] Security boundaries verified
- [ ] Findings and remediation explained
- [ ] Reproducible demo and sanitized screenshots available
- [ ] README, architecture, limitations, and lessons learned complete
