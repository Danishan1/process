# Portfolio Quality Standard

## Minimum documentation for each serious project
1. **Problem:** What operational/security failure is being addressed?
2. **Audience:** Who needs the result and what decision will it support?
3. **Scope:** Authorized assets, data, assumptions, and exclusions.
4. **Architecture:** Components, trust boundaries, data flow, and permissions.
5. **Implementation:** Setup steps, commands, versions, configuration, and cleanup.
6. **Validation:** Positive and negative tests with expected and actual outcomes.
7. **Findings:** Evidence, severity rationale, limitations, and false positives.
8. **Remediation:** Fix or mitigation, then a repeat test.
9. **Demo:** Short walkthrough with synthetic data and no exposed secrets.
10. **Lessons:** What failed, what changed, and what would be needed for production.

## Suggested repository layout
```text
project-name/
  README.md
  docs/
    architecture.md
    threat-model.md
    findings.md
    test-plan.md
  src/
  tests/
  examples/            # synthetic and sanitized only
  scripts/
  .gitignore
```

## Scorecard (100 points)
| Criterion | Points |
|---|---:|
| Clear problem, scope, and architecture | 15 |
| Working end-to-end implementation | 25 |
| Testing, reproducibility, and error handling | 20 |
| Security controls and ethical boundaries | 15 |
| Evidence, analysis, and remediation | 15 |
| Clear documentation and demo | 10 |

A polished project should aim for 80+ points, with no missing safety requirements.

## What not to publish
Passwords, API tokens, private IP/network details that expose sensitive assets, personal data, third-party confidential logs, exploit material targeting unauthorized systems, or copyrighted datasets without permission.
