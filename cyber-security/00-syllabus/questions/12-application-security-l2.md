Absolutely. **Application Security — Level 2** is where we move from understanding vulnerabilities to **thinking like an application security tester**.

Level 1 established:

> **Application architecture → HTTP → Authentication → Authorization → Input → Sessions → Data → Secure Development**

Level 2 now asks:

> **“How do I systematically assess an application, identify weaknesses, understand exploitability, chain vulnerabilities, determine business impact, and communicate the finding so it can actually be fixed?”**

For our career priority, this level should be strongly connected to **Red Team / Pentesting**, while still developing the defensive mindset needed by **Blue Team/SOC**.

Our progression:

> **Level 1 — Application Architecture, Security Fundamentals & Common Vulnerabilities**
> **Level 2 — Application Security Testing, Vulnerability Analysis, APIs, Business Logic & Attack Chains**
> **Level 3 — Advanced Web/API Security, DevSecOps, Source-Code Analysis, Advanced Exploitation & AppSec Architecture**

# APPLICATION SECURITY — LEVEL 2

## Intermediate Question Bank — 150 Questions

---

# A. Application Security Testing Methodology

### 1.

**What is the purpose of an application security assessment, and how is it different from simply running a vulnerability scanner?**

### 2.

**What phases should a structured web-application security assessment contain?**

### 3.

**How should an application tester define the scope of an authorized assessment?**

### 4.

**Why is understanding the application's intended functionality important before testing its security?**

### 5.

**How can application functionality reveal potential attack surfaces?**

### 6.

**What information should be collected during application reconnaissance?**

### 7.

**How should an assessor map an application's attack surface before testing individual vulnerabilities?**

### 8.

**Why should authentication and authorization boundaries be identified early during an assessment?**

### 9.

**How should findings be prioritized when an application contains many vulnerabilities?**

### 10.

**Design a complete methodology for assessing a web application from reconnaissance through vulnerability validation, reporting, remediation, and retesting.**

---

# B. Application Reconnaissance

### 11.

**What information should a tester collect about an application before interacting with security-sensitive functionality?**

### 12.

**How can DNS information help identify application infrastructure?**

### 13.

**How can subdomains expand an application's attack surface?**

### 14.

**Why should forgotten development and staging environments be included in an authorized assessment when they are within scope?**

### 15.

**How can HTTP headers reveal information about application architecture?**

### 16.

**How can application error messages reveal backend technologies?**

### 17.

**How can JavaScript files reveal API endpoints or application functionality?**

### 18.

**How can robots.txt, sitemap files, documentation, or public metadata reveal application paths?**

### 19.

**Why should discovered technologies be treated as clues rather than proof of vulnerabilities?**

### 20.

**Design a passive and low-impact reconnaissance process for mapping a web application's technologies, endpoints, domains, APIs, authentication mechanisms, and public attack surface.**

---

# C. Attack Surface Mapping

### 21.

**What is an application attack-surface map?**

### 22.

**What types of endpoints should be included in an application attack-surface inventory?**

### 23.

**Why should API endpoints be mapped separately from traditional web pages?**

### 24.

**Why should authentication, administrative, upload, payment, and account-management functionality receive special attention?**

### 25.

**How can different user roles expose different parts of an application's attack surface?**

### 26.

**Why should HTTP methods be included when mapping endpoints?**

### 27.

**How can hidden or undocumented functionality affect application security?**

### 28.

**How can application features create relationships between otherwise separate endpoints?**

### 29.

**Why is an endpoint's business purpose important when determining its security risk?**

### 30.

**Given a fictional application, construct an attack-surface map covering users, roles, endpoints, APIs, files, databases, third-party services, and administrative functionality.**

---

# D. Authentication Testing

### 31.

**What security properties should a secure application authentication mechanism provide?**

### 32.

**How should an assessor evaluate an application's login process?**

### 33.

**What weaknesses can arise from poor password policies?**

### 34.

**Why can authentication rate limiting be important?**

### 35.

**How can account-enumeration vulnerabilities arise?**

### 36.

**Why should login error messages avoid unnecessarily revealing whether an account exists?**

### 37.

**How should password-reset functionality be assessed from a security perspective?**

### 38.

**What security properties should password-reset tokens have?**

### 39.

**How can weaknesses in account-recovery workflows bypass otherwise strong authentication?**

### 40.

**Design an authorized authentication-security test plan covering login, registration, password reset, MFA, account recovery, lockout, session creation, and authentication failure handling.**

---

# E. Session Security Testing

### 41.

**How should a tester evaluate the security of application sessions?**

### 42.

**What properties should secure session identifiers have?**

### 43.

**How can session fixation vulnerabilities occur?**

### 44.

**How can session invalidation failures create security risks?**

### 45.

**Why should session identifiers change appropriately after authentication events?**

### 46.

**How can insecure cookie attributes increase session risk?**

### 47.

**Why should applications use secure transport for session credentials?**

### 48.

**How can concurrent sessions affect application security?**

### 49.

**How should a tester evaluate logout functionality?**

### 50.

**Design a session-security test methodology covering session creation, renewal, expiration, logout, cookie attributes, fixation, invalidation, and concurrent sessions.**

---

# F. Authorization & IDOR Testing

### 51.

**How should an application tester determine whether authorization controls are consistently enforced?**

### 52.

**What is horizontal privilege escalation?**

### 53.

**What is vertical privilege escalation?**

### 54.

**How can an IDOR vulnerability be identified conceptually?**

### 55.

**Why should changing an object identifier never automatically grant access to another user's resource?**

### 56.

**How can API endpoints accidentally expose authorization weaknesses even when the web interface appears secure?**

### 57.

**Why must authorization checks occur server-side?**

### 58.

**How can hidden frontend controls create a false sense of security?**

### 59.

**How should testers compare access between different application roles?**

### 60.

**Given three fictional user roles and several API endpoints, construct a role-permission matrix and identify potential horizontal and vertical authorization failures.**

---

# G. Input Validation & Injection Testing

### 61.

**How should a tester identify user-controlled input throughout an application?**

### 62.

**What is the difference between reflected input and stored input?**

### 63.

**Why should every input point be considered independently during security testing?**

### 64.

**How can insufficient validation lead to injection vulnerabilities?**

### 65.

**How should an assessor determine the context in which application input is used?**

### 66.

**Why does the same input require different defenses depending on whether it reaches HTML, JavaScript, SQL, a shell, or another interpreter?**

### 67.

**What is SQL injection at the application-security testing level?**

### 68.

**What is command injection at the application-security testing level?**

### 69.

**What is template injection conceptually, and why can server-side templates become security-sensitive?**

### 70.

**Design a safe lab methodology for identifying and validating injection vulnerabilities while minimizing destructive or unintended effects.**

---

# H. Cross-Site Scripting Testing

### 71.

**How should a tester distinguish reflected, stored, and DOM-based XSS?**

### 72.

**How can a tester identify application inputs that reach HTML output?**

### 73.

**Why does the output context matter when analyzing XSS?**

### 74.

**How can JavaScript DOM manipulation introduce XSS vulnerabilities?**

### 75.

**Why can stored XSS have broader impact than reflected XSS?**

### 76.

**Why can XSS targeting privileged users be especially dangerous?**

### 77.

**How should Content Security Policy be evaluated as a defense against XSS?**

### 78.

**How can browser security controls reduce the impact of XSS without fixing the underlying vulnerability?**

### 79.

**How should an XSS vulnerability be validated safely in an authorized testing environment?**

### 80.

**Given a fictional vulnerable application, determine where untrusted data enters, where it is rendered, what context is involved, what defense is missing, and how the application should be fixed.**

---

# I. API Security Testing

### 81.

**Why have APIs become one of the most important application attack surfaces?**

### 82.

**How should an API inventory be constructed during an application assessment?**

### 83.

**What authentication mechanisms can APIs use?**

### 84.

**How should API authorization be tested independently from web-interface authorization?**

### 85.

**What is Broken Object Level Authorization (BOLA), and how is it related to IDOR?**

### 86.

**What is Broken Function Level Authorization?**

### 87.

**Why should APIs validate HTTP methods and content types correctly?**

### 88.

**How can excessive API data exposure create privacy or security risks?**

### 89.

**How can API rate limiting contribute to application security?**

### 90.

**Design a comprehensive API security assessment methodology covering authentication, authorization, input validation, object access, rate limiting, error handling, data exposure, and business logic.**

---

# J. JWT & Token Security

### 91.

**What is a JSON Web Token (JWT), and why is it commonly used by applications?**

### 92.

**What are the major components of a JWT?**

### 93.

**What security properties should applications enforce when validating tokens?**

### 94.

**Why should applications not blindly trust token claims?**

### 95.

**Why must token signatures be properly validated?**

### 96.

**How can excessive information inside tokens create security or privacy problems?**

### 97.

**What is token expiration, and why is it important?**

### 98.

**How should applications handle token revocation or invalidation requirements?**

### 99.

**How can insecure token storage increase application risk?**

### 100.

**Design a secure token-based authentication architecture covering issuance, validation, expiration, storage, rotation, revocation, and authorization.**

---

# K. Business Logic Security

### 101.

**What is a business-logic vulnerability?**

### 102.

**Why can an application be technically secure but still vulnerable because of flawed business logic?**

### 103.

**How can attackers abuse legitimate application functionality without exploiting a traditional software vulnerability?**

### 104.

**What is workflow abuse?**

### 105.

**How can missing state validation create business-logic vulnerabilities?**

### 106.

**How can price, quantity, balance, or transaction manipulation affect an application?**

### 107.

**Why should applications verify security-sensitive calculations on the server?**

### 108.

**How can race conditions affect business processes?**

### 109.

**How should testers identify assumptions embedded in application workflows?**

### 110.

**Given a fictional banking, e-commerce, or ticketing workflow, identify where a malicious user could abuse legitimate functionality and explain how the application should enforce the intended business rules.**

---

# L. File Upload & Path Handling

### 111.

**How should a tester evaluate the security of an application's file-upload functionality?**

### 112.

**Why is checking only the filename extension insufficient?**

### 113.

**Why should file content and expected file type be validated?**

### 114.

**How can unsafe file storage locations create security problems?**

### 115.

**What is path traversal at a conceptual level?**

### 116.

**How can user-controlled file paths affect application security?**

### 117.

**Why should applications avoid directly trusting user-supplied filenames or paths?**

### 118.

**How can archive-processing functionality create additional security risks?**

### 119.

**How can uploaded content interact with other application components?**

### 120.

**Design a safe methodology for assessing file-upload and file-download functionality, including validation, storage, path handling, permissions, and content processing.**

---

# M. SSRF & Server-Side Requests

### 121.

**What is Server-Side Request Forgery (SSRF) at a conceptual level?**

### 122.

**Why is SSRF fundamentally related to server-side trust of user-controlled URLs or network destinations?**

### 123.

**Why can SSRF be particularly dangerous in cloud environments?**

### 124.

**How can an application unintentionally become a network proxy for an attacker?**

### 125.

**Why should applications restrict which destinations server-side requests can reach?**

### 126.

**How can network segmentation reduce the impact of SSRF?**

### 127.

**How can cloud workload identity increase the impact of a server-side request vulnerability?**

### 128.

**What logging can help detect suspicious server-side outbound requests?**

### 129.

**How should an authorized tester validate SSRF safely without accessing unrelated internal systems?**

### 130.

**Design a secure architecture for a feature that legitimately fetches remote resources while minimizing SSRF risk.**

---

# N. Application Cryptography & Secrets

### 131.

**How should an application determine which data requires encryption?**

### 132.

**What is the difference between encryption, hashing, and encoding?**

### 133.

**Why should passwords be hashed rather than encrypted for normal password storage?**

### 134.

**Why should applications avoid implementing their own cryptographic algorithms?**

### 135.

**How should cryptographic keys be stored and protected?**

### 136.

**Why should application secrets be separated from source code?**

### 137.

**How can secrets leak through source repositories, logs, configuration files, or build systems?**

### 138.

**Why is key rotation important?**

### 139.

**How can compromised application secrets affect cloud infrastructure?**

### 140.

**Design a secure application secret and cryptographic-key management architecture covering generation, storage, access, rotation, revocation, logging, and incident response.**

---

# O. Vulnerability Analysis, Reporting & Remediation

### 141.

**How should an application-security tester determine whether a suspected weakness is actually exploitable?**

### 142.

**What is the difference between vulnerability severity and business risk?**

### 143.

**Why should application vulnerabilities be evaluated in their specific business context?**

### 144.

**How can multiple low-severity vulnerabilities combine into a high-impact attack chain?**

### 145.

**What information should a high-quality application-security finding contain?**

### 146.

**Why should a vulnerability report explain root cause rather than only describe the observed behavior?**

### 147.

**How should remediation recommendations address the underlying design or coding problem?**

### 148.

**Why should security testers perform retesting after remediation?**

### 149.

**How can penetration-testing findings be converted into secure coding requirements, detections, and automated security tests?**

### 150.

**Design a complete application-security assessment and remediation lifecycle that connects reconnaissance, testing, vulnerability validation, attack-chain analysis, reporting, developer remediation, retesting, and continuous security improvement.**

---

# The Level 2 AppSec Mental Model

Level 1 taught us:

```text
Application
    ↓
Input
    ↓
Processing
    ↓
Decision
    ↓
Resource
```

Level 2 adds the **attacker's perspective**:

```text
                    APPLICATION
                         │
                         ▼
                  ATTACK SURFACE
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
        INPUT         IDENTITY        LOGIC
          │              │              │
          ▼              ▼              ▼
      Injection       Auth/Authz    Workflow
      XSS             Sessions       State
      Files           Tokens         Transactions
          │              │              │
          └──────────────┼──────────────┘
                         ▼
                    ATTACK PATH
                         │
                         ▼
                       IMPACT
```

But there's another side:

```text
                    ATTACK
                      │
                      ▼
                  TELEMETRY
                      │
                      ▼
                   DETECT
                      │
                      ▼
                INVESTIGATE
                      │
                      ▼
                 REMEDIATE
                      │
                      ▼
                   RE-TEST
```

This is where AppSec begins to connect directly with our **Blue Team/SOC** goal.

---

# The AppSec Attack-Chain Mindset

Don't stop at:

> **“There is an IDOR.”**

Instead ask:

```text
Low-privileged user
       ↓
Authorization weakness
       ↓
Access another user's resource
       ↓
Sensitive information
       ↓
Credential/token exposure
       ↓
Higher privilege
       ↓
Cloud/API access
       ↓
Larger impact
```

Or:

```text
XSS
 ↓
Privileged user interaction
 ↓
Session/account compromise
 ↓
Administrative functionality
 ↓
Application control
```

Or:

```text
SSRF
 ↓
Internal service access
 ↓
Cloud identity / internal API
 ↓
Additional permissions
 ↓
Cloud resources
```

The important skill is **chaining**.

A vulnerability's real importance often comes from what it enables next.

---

# AppSec Testing Flow

Our practical Level 2 methodology should eventually look like:

```text
1. Understand Application
          ↓
2. Map Attack Surface
          ↓
3. Identify Trust Boundaries
          ↓
4. Identify Roles
          ↓
5. Test Authentication
          ↓
6. Test Authorization
          ↓
7. Test Input Handling
          ↓
8. Test APIs
          ↓
9. Test Business Logic
          ↓
10. Test File/Network Interaction
          ↓
11. Analyze Attack Chains
          ↓
12. Determine Impact
          ↓
13. Document Root Cause
          ↓
14. Recommend Remediation
          ↓
15. Retest
```

The goal isn't to blindly test everything.

It's to develop a **hypothesis-driven assessment**.

---

# The Most Important Level 2 Questions

Whenever we encounter an application, these should become automatic.

### 1. What can I control?

```text
Parameters
Headers
Cookies
Files
Requests
Tokens
Workflow actions
```

### 2. What does the application trust?

```text
Identity
Role
Object ID
Client-side state
Token claims
Input
External services
```

### 3. What happens if I change it?

This is where authorization and business-logic testing begins.

### 4. Where does the data go?

```text
HTML
JavaScript
Database
Filesystem
OS
API
Cloud
Third-party service
```

### 5. What interpreter processes it?

```text
Browser
SQL engine
Shell
Template engine
Parser
XML/JSON processor
Application framework
```

This is critical for understanding injection vulnerabilities.

### 6. What happens after compromise?

```text
Account
   ↓
Application
   ↓
Server
   ↓
Credential
   ↓
Cloud Identity
   ↓
Cloud Resource
```

This connects AppSec directly to our **Red Team + Cloud Security** roadmap.

---

# AppSec ↔ Cloud Security

This connection becomes extremely important at Level 2.

Consider:

```text
Internet
   ↓
Web Application
   ↓
SSRF
   ↓
Cloud Workload
   ↓
Workload Identity
   ↓
Cloud Permissions
   ↓
Storage
   ↓
Sensitive Data
```

A vulnerability that initially looks like an **AppSec problem** can become a **Cloud Security problem**.

Similarly:

```text
Application
    ↓
Excessive Cloud IAM Permissions
    ↓
Application Compromise
    ↓
Cloud Privilege
    ↓
Cloud Attack Path
```

Therefore:

> **The application is often one of the identities operating inside the cloud.**

That's a very important concept for our eventual Red Team career.

---

# AppSec ↔ Active Directory

The same principle applies on-premises.

```text
Web Application
      ↓
Compromised Server
      ↓
Service Account
      ↓
Windows Environment
      ↓
Active Directory
      ↓
Additional Privileges
```

So our knowledge isn't isolated:

```text
Web
 │
 ▼
Application
 │
 ▼
Server
 │
 ├──────────────► Active Directory
 │
 └──────────────► Cloud IAM
```

This is exactly why we built the earlier subjects together instead of studying them as isolated topics.

---

# Red Team Perspective

At Level 2, our Red Team thinking becomes:

> **Find the application's trust boundaries and determine whether they can be crossed.**

We should increasingly think in terms of:

```text
Recon
 ↓
Attack Surface
 ↓
Authentication
 ↓
Authorization
 ↓
Input
 ↓
Business Logic
 ↓
API
 ↓
Server
 ↓
Identity
 ↓
Cloud / AD
```

Not:

> **“Which vulnerability can I find?”**

But:

> **“What security boundary can I cross?”**

That is a much stronger pentesting mindset.

---

# Blue Team Perspective

The Blue Team question becomes:

> **“If this attack happens, what evidence will exist?”**

For example:

```text
Attacker
   ↓
Application Request
   ↓
Web Server Log
   ↓
Application Log
   ↓
Authentication Event
   ↓
Database Event
   ↓
Endpoint Event
   ↓
Cloud/API Event
   ↓
SOC
```

So eventually we should be able to take a Red Team finding and ask:

> **Can our SOC detect it?**

If not:

> **What telemetry is missing?**

Then:

> **What detection should we build?**

That is the beginning of **Purple Team application security**.

---

# Level 1 → Level 2

| Level 1                     | Level 2                                                     |
| --------------------------- | ----------------------------------------------------------- |
| What is authentication?     | How do we test authentication?                              |
| What is authorization?      | How do we find authorization failures?                      |
| What is XSS?                | How do we identify and validate XSS safely?                 |
| What is SQL injection?      | How do we determine whether input reaches a query unsafely? |
| What is an API?             | How do we assess API authorization and data exposure?       |
| What is a session?          | How do we assess session lifecycle and security?            |
| What is input validation?   | How do we trace input through an application?               |
| What is business logic?     | How can legitimate functionality be abused?                 |
| What is SSRF?               | How do we assess server-side network trust?                 |
| What are secrets?           | How can application secrets become an attack path?          |
| What is a vulnerability?    | What is its actual impact and attack chain?                 |
| What is secure development? | How do we convert findings into fixes and tests?            |

---

# What Level 2 Should Ultimately Give Us

By the end of Level 2, looking at an application should trigger something like:

```text
                    APPLICATION
                         │
                  ┌──────┴──────┐
                  ▼             ▼
              RECON         ARCHITECTURE
                  │             │
                  └──────┬──────┘
                         ▼
                   ATTACK SURFACE
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
   AUTHENTICATION    AUTHORIZATION      INPUT
        │                │                │
        ▼                ▼                ▼
      SESSION         IDOR/RBAC        INJECTION
        │                │                │
        └────────────────┼────────────────┘
                         ▼
                       APIs
                         │
                         ▼
                   BUSINESS LOGIC
                         │
                         ▼
                    ATTACK CHAIN
                         │
             ┌───────────┴───────────┐
             ▼                       ▼
         APPLICATION               CLOUD
             │                       │
             ▼                       ▼
          SERVER                 IAM/WORKLOAD
             │                       │
             └───────────┬───────────┘
                         ▼
                       IMPACT
```

And from the defensive side:

```text
                    ATTACK
                       ↓
                  APPLICATION
                       ↓
                    LOGGING
                       ↓
                   DETECTION
                       ↓
                  INVESTIGATION
                       ↓
                  CONTAINMENT
                       ↓
                  REMEDIATION
                       ↓
                     RETEST
```

That is the **Level 2 AppSec mindset** we want.

Level 3 will take this considerably deeper into **advanced web/API vulnerabilities, complex authorization, advanced business-logic attacks, SSRF and parser security, deserialization, advanced client-side security, source-code review, SAST/DAST/IAST, dependency and supply-chain security, DevSecOps, threat modeling, advanced attack chains, and enterprise AppSec architecture**.
