Absolutely. **Security Fundamentals Level 2** is where we move from *"I understand the security concepts"* to *"I understand how security mechanisms actually work and how they fail."*

Level 1 established:

> **Asset → Threat → Vulnerability → Risk → Control → Detection → Response → Recovery**

Level 2 should now make you reason about the **mechanisms behind those concepts**.

The emphasis will be on:

* security architecture
* identity and access
* cryptography
* network security
* endpoint security
* vulnerability management
* secure configuration
* logging and monitoring
* malware
* security operations
* attack chains
* defense-in-depth
* offensive vs defensive thinking

The questions are still designed to make you **think**, not merely memorize definitions.

# SECURITY FUNDAMENTALS — LEVEL 2

## Intermediate Question Bank

**150 Questions**

---

# A. Security Architecture & Defense-in-Depth

### 1.

**What is security architecture, and how is it different from implementing individual security tools?**

### 2.

**Why should security controls be designed around threats, assets, trust boundaries, and business requirements?**

### 3.

**What is defense-in-depth, and why is relying on a single security control dangerous?**

### 4.

**How can multiple layers of controls compensate when one security mechanism fails?**

### 5.

**What is a security boundary, and how can improperly defined boundaries create vulnerabilities?**

### 6.

**How does network segmentation contribute to defense-in-depth?**

### 7.

**Why should sensitive systems generally have fewer pathways to reach them?**

### 8.

**What is the principle of least functionality, and how does it reduce attack surface?**

### 9.

**What is fail-safe design, and how can the default behavior of a failed security control affect security?**

### 10.

**Design a defense-in-depth architecture for a company's critical database, starting from the Internet edge and ending at the database itself.**

---

# B. Zero Trust & Security Principles

### 11.

**What is the basic philosophy behind Zero Trust security?**

### 12.

**Why does Zero Trust challenge the assumption that an internal network is automatically trustworthy?**

### 13.

**How are identity, device posture, application context, and resource sensitivity relevant to Zero Trust decisions?**

### 14.

**Why should authentication and authorization be continuously evaluated rather than treated as one-time decisions?**

### 15.

**How does least privilege contribute to a Zero Trust architecture?**

### 16.

**What is micro-segmentation, and how can it limit lateral movement?**

### 17.

**Why does Zero Trust not simply mean "use MFA everywhere"?**

### 18.

**How can Zero Trust principles be applied to cloud environments?**

### 19.

**What challenges might an organization face when migrating from a traditional perimeter model toward Zero Trust?**

### 20.

**Design a conceptual Zero Trust architecture for a company whose employees work from offices, homes, and public networks.**

---

# C. Identity & Access Management

### 21.

**What is Identity and Access Management (IAM), and what problems does it attempt to solve?**

### 22.

**What is the identity lifecycle from account creation to account termination?**

### 23.

**Why is employee offboarding an important security process?**

### 24.

**What can happen when an employee leaves an organization but their accounts remain active?**

### 25.

**What is Role-Based Access Control (RBAC)?**

### 26.

**What is Attribute-Based Access Control (ABAC), and how does it differ conceptually from RBAC?**

### 27.

**Why can excessive role complexity itself become a security problem?**

### 28.

**What is privilege creep, and how can organizations detect and reduce it?**

### 29.

**Why should privileged accounts be separated from ordinary user accounts?**

### 30.

**Design an IAM model for an organization containing employees, administrators, developers, security analysts, contractors, and service accounts.**

---

# D. Authentication Mechanisms

### 31.

**How does a typical password-based authentication process work from the user's input to the server's verification decision?**

### 32.

**Why should passwords never be stored directly in plaintext?**

### 33.

**What properties should a password-storage mechanism provide?**

### 34.

**Why are password hashing and encryption fundamentally different approaches?**

### 35.

**What are password salts, and what security problem do they address?**

### 36.

**What is password spraying, and how does it differ from brute-force attacks against a single account?**

### 37.

**What is credential stuffing, and why can breached credentials from another organization become a threat?**

### 38.

**How can rate limiting and account protection mechanisms reduce automated authentication attacks?**

### 39.

**Why can poorly designed account lockout mechanisms create availability problems?**

### 40.

**Design a resilient authentication system that balances protection against credential attacks with usability and availability.**

---

# E. Multi-Factor Authentication

### 41.

**Why does MFA provide stronger security than a password alone?**

### 42.

**What makes two authentication methods genuinely independent factors?**

### 43.

**Why is receiving a code through a separate channel not automatically equivalent to using a phishing-resistant authentication mechanism?**

### 44.

**What is phishing-resistant authentication at a conceptual level?**

### 45.

**Why are hardware-backed authentication mechanisms valuable?**

### 46.

**What security risks remain after MFA is enabled?**

### 47.

**How can attackers target the recovery and enrollment processes of an MFA system?**

### 48.

**Why should organizations protect MFA enrollment and reset operations as carefully as login?**

### 49.

**How should emergency or backup authentication methods be secured?**

### 50.

**Design an MFA strategy for administrators of an organization's most sensitive systems. Explain factor choice, enrollment, recovery, monitoring, and emergency access.**

---

# F. Privileged Access

### 51.

**What makes a privileged account fundamentally different from a normal user account?**

### 52.

**Why should administrative privileges generally be granted only when necessary?**

### 53.

**What is Just-in-Time privileged access?**

### 54.

**How can temporary privilege reduce the attack window associated with administrator accounts?**

### 55.

**Why should administrators avoid using privileged accounts for routine activities such as email and web browsing?**

### 56.

**What is Privileged Access Management (PAM)?**

### 57.

**Why are privileged credentials attractive targets for attackers?**

### 58.

**How can compromised administrator credentials facilitate lateral movement?**

### 59.

**What types of activity should organizations monitor for privileged accounts?**

### 60.

**Design a privileged-access strategy for a large organization and explain how you would reduce the risk of administrator-account compromise.**

---

# G. Cryptography — Intermediate

### 61.

**Why does cryptography require both strong algorithms and secure key management?**

### 62.

**What is the difference between a cryptographic key, a password, and a secret?**

### 63.

**Why should encryption algorithms generally be public while cryptographic keys remain secret?**

### 64.

**What is the difference between encryption at rest and encryption in transit?**

### 65.

**What security risks remain even when sensitive data is encrypted at rest?**

### 66.

**What is authenticated encryption, and why is confidentiality alone sometimes insufficient?**

### 67.

**Why is integrity protection important when transmitting sensitive information?**

### 68.

**What is a nonce or initialization value, and why can incorrect reuse create security problems for some cryptographic schemes?**

### 69.

**Why does cryptographic randomness matter when generating keys, tokens, and other security-sensitive values?**

### 70.

**Design a cryptographic strategy for protecting sensitive customer data both in storage and while traveling across a network.**

---

# H. Hashing, Password Storage & Integrity

### 71.

**What properties make a cryptographic hash function useful for security applications?**

### 72.

**Why should ordinary cryptographic hashes not be used directly for password storage?**

### 73.

**What makes password-specific hashing mechanisms different from general-purpose hashing algorithms?**

### 74.

**What is computational cost in password hashing, and why can intentionally expensive operations improve password security?**

### 75.

**What is a dictionary attack against password hashes?**

### 76.

**Why does a salt make precomputed password-cracking tables less effective?**

### 77.

**Why does hashing help verify integrity without providing confidentiality?**

### 78.

**How can hashes be used to identify changes to files or software?**

### 79.

**Why does knowing the hash of a file not automatically prove that the file came from a trusted source?**

### 80.

**Design a secure password-storage system and explain the purpose of salts, password-specific hashing, computational cost, and credential-protection controls.**

---

# I. PKI, Certificates & TLS

### 81.

**What problem does Public Key Infrastructure attempt to solve?**

### 82.

**What is the relationship between a certificate, a public key, a private key, and a certificate authority?**

### 83.

**Why does a browser need to establish trust in the identity represented by an HTTPS certificate?**

### 84.

**What is a certificate chain?**

### 85.

**What is a root certificate authority, and why is it trusted differently from an ordinary website certificate?**

### 86.

**What happens conceptually when a browser connects to an HTTPS website?**

### 87.

**How does TLS establish a secure communication channel without simply sending a shared secret in plaintext?**

### 88.

**What security properties does modern TLS attempt to provide?**

### 89.

**What could happen if a server's private TLS key were compromised?**

### 90.

**Explain the major stages of a secure HTTPS connection from certificate validation through encrypted application traffic.**

---

# J. Network Security Controls

### 91.

**What is a firewall, and what security decision does it make?**

### 92.

**What is the conceptual difference between packet filtering and application-aware traffic inspection?**

### 93.

**What is a stateful firewall, and why does connection state matter?**

### 94.

**What is network segmentation, and why is it valuable even when perimeter defenses exist?**

### 95.

**What is an intrusion detection system, and what does it attempt to identify?**

### 96.

**How is an intrusion prevention system different from an intrusion detection system?**

### 97.

**What is network traffic analysis, and what kinds of security information can traffic reveal?**

### 98.

**Why can encrypted traffic still provide useful metadata for defenders?**

### 99.

**Why should network security controls be combined with endpoint and identity controls?**

### 100.

**Design a network security architecture for a company containing Internet-facing servers, employee systems, internal applications, administrative systems, and sensitive databases.**

---

# K. Endpoint Security

### 101.

**What is endpoint security, and which systems are considered endpoints?**

### 102.

**Why are employee laptops often attractive targets for attackers?**

### 103.

**What is the difference between antivirus and modern endpoint detection and response?**

### 104.

**What types of behavior can EDR systems monitor?**

### 105.

**Why is process behavior often more informative than simply matching known malware signatures?**

### 106.

**What is application allowlisting, and how can it reduce endpoint attack surface?**

### 107.

**Why are operating-system updates important to endpoint security?**

### 108.

**How can endpoint configuration affect the success or impact of malware?**

### 109.

**Why should administrator privileges on employee endpoints be restricted?**

### 110.

**Design a baseline security configuration for a corporate employee laptop and explain how each control reduces security risk.**

---

# L. Vulnerability Management

### 111.

**What is vulnerability management, and why is it a continuous process?**

### 112.

**What is the difference between vulnerability discovery, assessment, prioritization, remediation, and verification?**

### 113.

**Why is vulnerability scanning not equivalent to penetration testing?**

### 114.

**What is a false positive in vulnerability assessment?**

### 115.

**What is a false negative, and why can it be particularly dangerous?**

### 116.

**Why should vulnerability severity be combined with asset criticality and environmental context?**

### 117.

**What is a vulnerability remediation SLA, and why might different vulnerabilities have different deadlines?**

### 118.

**Why should organizations verify that vulnerabilities were actually remediated rather than simply marking them as fixed?**

### 119.

**How can vulnerability management help both Red Team and Blue Team professionals?**

### 120.

**Design a vulnerability-management lifecycle for a company containing thousands of endpoints, servers, applications, and cloud resources.**

---

# M. Secure Configuration & Hardening

### 121.

**What is system hardening?**

### 122.

**Why can default configurations create unnecessary security risk?**

### 123.

**What is the principle of least functionality in system hardening?**

### 124.

**Why should unnecessary services, ports, applications, and accounts be disabled?**

### 125.

**Why should security configurations be standardized across similar systems?**

### 126.

**What is configuration drift, and why can it weaken security over time?**

### 127.

**How can configuration management tools help maintain security baselines?**

### 128.

**Why should security baselines be regularly reviewed rather than treated as permanent?**

### 129.

**How can attackers benefit from weak configurations even when no software vulnerability exists?**

### 130.

**Create a hardening strategy for a Linux server exposed to the Internet. Identify the major categories of configuration you would review and explain why.**

---

# N. Logging, Monitoring & Detection

### 131.

**Why are logs important to cybersecurity?**

### 132.

**What is the difference between logging, monitoring, detection, and alerting?**

### 133.

**What types of events should organizations prioritize for security logging?**

### 134.

**Why are authentication logs particularly valuable to security teams?**

### 135.

**Why can endpoint, network, application, cloud, and identity logs become more valuable when correlated together?**

### 136.

**What is a Security Information and Event Management system, and what problem does it attempt to solve?**

### 137.

**Why can excessive logging create operational and security problems?**

### 138.

**Why should security logs themselves be protected from unauthorized modification or deletion?**

### 139.

**What characteristics make a security alert useful to an analyst?**

### 140.

**Design a basic monitoring strategy for detecting a compromised employee account. Identify the logs you would collect, useful signals, possible alerts, and investigation steps.**

---

# O. Attack Chains, Incident Response & Security Thinking

### 141.

**What is an attack chain, and why is understanding the sequence of attacker actions useful to defenders?**

### 142.

**Why might an attacker perform reconnaissance, initial access, persistence, privilege escalation, lateral movement, and data access as separate stages?**

### 143.

**What is persistence, and why might an attacker attempt to establish it after gaining access?**

### 144.

**What is privilege escalation, and why does it frequently appear in enterprise attack chains?**

### 145.

**What is lateral movement, and why is it a major concern after initial compromise?**

### 146.

**Why should defenders attempt to detect attacker behavior at multiple stages rather than only detecting the initial compromise?**

### 147.

**What is containment, and how should defenders balance containment with preserving evidence?**

### 148.

**Why should incident response plans include predefined communication and decision-making procedures?**

### 149.

**How can an organization use lessons from an incident to improve preventive, detective, and corrective controls?**

### 150.

**Construct a complete hypothetical attack scenario against an organization. Start with reconnaissance, progress through initial access, privilege escalation and lateral movement, reach a sensitive asset, and then explain where security controls could prevent, detect, contain, and recover from each stage.**

---

# The Level 2 Mental Model

At Level 1, we built:

```text
ASSET
  ↓
THREAT
  ↓
VULNERABILITY
  ↓
RISK
  ↓
CONTROL
```

Now we need to make that much more concrete:

```text
                         ASSET
                           │
                           ▼
                    ATTACK SURFACE
                           │
                           ▼
                     THREAT ACTOR
                           │
                           ▼
                    ATTACK VECTOR
                           │
                           ▼
                     VULNERABILITY
                           │
                           ▼
                       EXPLOIT
                           │
                           ▼
                    INITIAL ACCESS
                           │
                           ▼
                    PERSISTENCE
                           │
                           ▼
                 PRIVILEGE ESCALATION
                           │
                           ▼
                   LATERAL MOVEMENT
                           │
                           ▼
                    OBJECTIVE / IMPACT
```

And on the defensive side:

```text
                      SECURITY
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
      PREVENT           DETECT          RESPOND
        │                │                │
     Firewall          Logs            Containment
     MFA               SIEM            Eradication
     Hardening         EDR             Recovery
     IAM               IDS             Lessons
     Segmentation      Monitoring
```

This is the bridge between **Security Fundamentals** and our future **Red Team / Blue Team** work.

---

# The Most Important Level 2 Shift

At Level 1, you should be able to say:

> "A compromised account is a security risk."

At Level 2, you should start thinking:

```text
How was the account compromised?
        ↓
What credentials were obtained?
        ↓
What permissions does it have?
        ↓
What systems can it access?
        ↓
Can MFA stop further access?
        ↓
Can the attacker escalate privileges?
        ↓
Can they move laterally?
        ↓
What logs reveal the activity?
        ↓
What controls could contain them?
        ↓
What happens if they reach sensitive data?
```

That's a **security professional's way of thinking**.

---

# Red Team ↔ Blue Team Connection

This is especially important for our career objective.

| Red Team asks                  | Blue Team asks                      |
| ------------------------------ | ----------------------------------- |
| What can I access?             | What was accessed?                  |
| What can I exploit?            | What was exploited?                 |
| Where can I move?              | Where did the attacker move?        |
| Can I escalate privileges?     | Was privilege escalation attempted? |
| Can I maintain access?         | Did persistence occur?              |
| Can I reach sensitive data?    | Was sensitive data accessed?        |
| How can I bypass this control? | Why did the control fail?           |
| What attack path exists?       | How can we break the attack path?   |

This is why we deliberately want exposure to **both Red and Blue Team thinking**.

A good pentester should understand detection.

A good SOC analyst should understand attacker behavior.

And eventually, when we combine both:

```text
              RED TEAM
                  │
          Attack / Validate
                  │
                  ▼
             SECURITY
              CONTROL
                  │
          Detect / Analyze
                  │
                  ▼
             BLUE TEAM
                  │
          Improve Control
                  │
                  └──────────► RED TEAM
```

That creates the continuous security loop.

---

# Level 2 Core Knowledge Map

By the end of this level, your conceptual map should look something like:

```text
SECURITY FUNDAMENTALS
│
├── Security Architecture
│   ├── Defense in Depth
│   ├── Security Boundaries
│   ├── Least Functionality
│   └── Zero Trust
│
├── Identity
│   ├── IAM
│   ├── Authentication
│   ├── MFA
│   ├── Authorization
│   ├── RBAC / ABAC
│   └── Privileged Access
│
├── Cryptography
│   ├── Encryption
│   ├── Hashing
│   ├── Password Security
│   ├── Digital Signatures
│   ├── PKI
│   └── TLS
│
├── Infrastructure Security
│   ├── Firewalls
│   ├── IDS / IPS
│   ├── Segmentation
│   ├── Endpoint Security
│   └── Hardening
│
├── Vulnerability Management
│   ├── Discovery
│   ├── Assessment
│   ├── Prioritization
│   ├── Remediation
│   └── Verification
│
├── Security Operations
│   ├── Logging
│   ├── Monitoring
│   ├── Detection
│   ├── Alerting
│   └── SIEM
│
└── Attack / Defense
    ├── Attack Chains
    ├── Initial Access
    ├── Persistence
    ├── Privilege Escalation
    ├── Lateral Movement
    ├── Containment
    ├── Eradication
    └── Recovery
```

---

# The Four Questions We Should Now Ask About Everything

Level 1 gave us:

> **What is it?**

Level 2 adds three more dimensions:

### 1. How does it work?

### 2. How can it fail?

### 3. How can an attacker abuse that failure?

### 4. How can a defender prevent, detect, and respond to it?

For example, don't merely learn **MFA**:

```text
MFA
│
├── How does it work?
│
├── What threats does it address?
│
├── What threats does it NOT address?
│
├── How could its implementation fail?
│
├── How might an attacker target the MFA process?
│
├── What logs would reveal suspicious activity?
│
└── How should the organization respond?
```

Do the same with:

* firewalls
* encryption
* IAM
* EDR
* SIEM
* network segmentation
* vulnerability management
* authentication
* hardening
* logging

This is what will make the later subjects dramatically easier.

---

# Where We Are Now

Our roadmap is becoming:

```text
1. Networking
   ├── Level 1
   ├── Level 2
   └── Level 3

2. Linux
   ├── Level 1
   ├── Level 2
   └── Level 3

3. Windows
   ├── Level 1
   ├── Level 2
   └── Level 3

4. Python
   ├── Level 1
   ├── Level 2
   └── Level 3

5. Web Fundamentals
   ├── Level 1
   ├── Level 2
   └── Level 3

6. Security Fundamentals
   ├── Level 1  ✓
   ├── Level 2  ← NOW
   └── Level 3

7. Network Security & Analysis

8. Basic Red Team / VAPT

9. Basic Blue Team / SOC

10. Active Directory

11. Basic Cloud Security

12. Application Security
```

The important milestone after **Security Fundamentals Level 2** is that security should stop feeling like a separate subject.

It should start becoming a **lens through which we look at Networking, Linux, Windows, Python, and Web**.

And then **Security Fundamentals Level 3** will take the final step: from understanding individual mechanisms to reasoning about **real attack scenarios, defensive architecture, detection, incident response, and adversarial thinking**.
