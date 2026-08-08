Absolutely. **Active Directory — Level 2** is where we move from *understanding the AD environment* to **analyzing it from an attacker and defender perspective**.

Our progression is:

> **Level 1 — Understand the AD architecture**
> **Level 2 — Enumerate, analyze, attack paths & defend**
> **Level 3 — Advanced AD exploitation, domain compromise, detection & hardening**

Because **Red Team / Pentesting is our primary career**, Level 2 will deliberately make you think in both directions:

> **“What can I discover?” → “What can I abuse?” → “What evidence would that create?” → “How should the defender detect and prevent it?”**

# ACTIVE DIRECTORY — LEVEL 2

## Intermediate Question Bank — 150 Questions

---

# A. Active Directory Enumeration

### 1.

**What is Active Directory enumeration, and why is it an essential phase of an enterprise security assessment?**

### 2.

**What information should a penetration tester attempt to understand about an AD environment before assessing its security?**

### 3.

**How can a tester determine the domain name, domain controllers, and domain structure of an authorized environment?**

### 4.

**What types of information can be learned from user enumeration?**

### 5.

**What information can computer-object enumeration reveal about an organization's infrastructure?**

### 6.

**Why are group memberships particularly valuable during AD security assessments?**

### 7.

**How can Organizational Unit structure reveal information about an organization's administrative model?**

### 8.

**Why are service accounts important during AD enumeration?**

### 9.

**How can enumeration reveal relationships between users, computers, groups, and permissions?**

### 10.

**Design a methodology for systematically enumerating an authorized Active Directory environment while minimizing unnecessary noise.**

---

# B. LDAP Fundamentals & Enumeration

### 11.

**What is LDAP, and how does Active Directory use it?**

### 12.

**What is an LDAP directory tree?**

### 13.

**What is a Distinguished Name (DN)?**

### 14.

**What is an LDAP search filter?**

### 15.

**How can LDAP queries retrieve information about users and computers?**

### 16.

**Why can excessive LDAP information exposure become a security concern?**

### 17.

**What kinds of information should defenders consider sensitive when exposed through directory queries?**

### 18.

**How can LDAP enumeration help identify potentially interesting accounts?**

### 19.

**How can a defender monitor unusual LDAP enumeration behavior?**

### 20.

**Design an authorized LDAP-enumeration exercise that identifies useful directory information while demonstrating the security implications of excessive information exposure.**

---

# C. Users, Groups & Privilege Relationships

### 21.

**Why is understanding group membership more important than simply identifying individual users?**

### 22.

**How can nested groups create unexpected privilege relationships?**

### 23.

**How can a low-privileged user indirectly obtain access to a sensitive resource through group membership?**

### 24.

**Why should privileged groups receive additional monitoring?**

### 25.

**How can an analyst identify unnecessary privileged memberships?**

### 26.

**What is the security significance of users who can modify membership of privileged groups?**

### 27.

**How can service accounts create unexpected privilege relationships?**

### 28.

**Why can dormant accounts represent security risk?**

### 29.

**How should organizations identify and manage accounts with excessive privileges?**

### 30.

**Given a fictional AD group structure, determine which users have indirect access to a highly privileged group and explain why the relationship matters.**

---

# D. Service Accounts

### 31.

**What is a service account, and why is it used?**

### 32.

**Why are service accounts often more security-sensitive than ordinary user accounts?**

### 33.

**What problems can arise when service accounts have excessive privileges?**

### 34.

**Why are long-lived service credentials a security concern?**

### 35.

**How can password-management practices affect service-account security?**

### 36.

**What is a managed service account, and why can it improve security?**

### 37.

**How can defenders identify service accounts that have accumulated unnecessary privileges?**

### 38.

**How can unusual authentication activity involving service accounts indicate compromise?**

### 39.

**Why should service-account usage patterns be monitored?**

### 40.

**Design a secure service-account lifecycle covering creation, privilege assignment, credential management, monitoring, rotation, and retirement.**

---

# E. Kerberos Deep Dive

### 41.

**Explain the complete conceptual Kerberos authentication flow within an Active Directory domain.**

### 42.

**What is the KDC, and what are its major responsibilities?**

### 43.

**What is a TGT, and why is it important?**

### 44.

**What is a service ticket?**

### 45.

**How does a client obtain permission to access a particular service using Kerberos?**

### 46.

**What is a Service Principal Name (SPN)?**

### 47.

**Why are SPNs important for Kerberos service authentication?**

### 48.

**How can incorrectly configured SPNs create security weaknesses?**

### 49.

**How can defenders monitor unusual Kerberos authentication behavior?**

### 50.

**Construct a conceptual timeline showing how a user authenticates to the domain and then accesses a network service through Kerberos.**

---

# F. Kerberos Security Attacks

### 51.

**What is Kerberoasting, and what underlying Kerberos behavior makes it possible?**

### 52.

**Why are service accounts relevant to Kerberoasting?**

### 53.

**What characteristics make a service account more exposed to Kerberoasting risk?**

### 54.

**How can defenders reduce the risk associated with Kerberoasting?**

### 55.

**What is AS-REP roasting?**

### 56.

**Why can accounts that do not require Kerberos preauthentication create additional security risk?**

### 57.

**How can defenders identify suspicious Kerberos ticket-request behavior?**

### 58.

**How should a penetration tester safely validate Kerberos-related weaknesses in an authorized lab?**

### 59.

**How can Kerberos attack techniques be mapped to MITRE ATT&CK?**

### 60.

**Design a defensive strategy for detecting and mitigating common Kerberos credential-abuse techniques.**

---

# G. NTLM Security & Credential Abuse

### 61.

**Why does NTLM remain relevant in Active Directory security assessments?**

### 62.

**What security weaknesses can arise from continued use of legacy authentication?**

### 63.

**What is credential relay conceptually, and why is it dangerous?**

### 64.

**Why can authentication protocols become attack paths when combined with network positioning or weak configurations?**

### 65.

**What environmental conditions can increase the risk of credential-relay attacks?**

### 66.

**How can defenders reduce unnecessary NTLM usage?**

### 67.

**How can authentication-policy changes affect legacy applications?**

### 68.

**How should an organization safely identify systems still dependent on NTLM?**

### 69.

**How can a SOC detect suspicious NTLM authentication patterns?**

### 70.

**Design an assessment methodology for identifying and reducing unnecessary NTLM exposure in an enterprise environment.**

---

# H. Active Directory Permissions & ACLs

### 71.

**What are Access Control Lists (ACLs) in Active Directory?**

### 72.

**What is an Access Control Entry (ACE)?**

### 73.

**How do ACLs determine who can perform actions on directory objects?**

### 74.

**Why can seemingly minor permissions become dangerous when combined with other privileges?**

### 75.

**What is permission inheritance?**

### 76.

**How can inherited permissions create unexpected access relationships?**

### 77.

**Why are object-control permissions particularly important during AD security assessments?**

### 78.

**How can defenders identify dangerous delegated permissions?**

### 79.

**How can excessive permissions create privilege-escalation paths?**

### 80.

**Given a hypothetical permission graph, determine how a low-privileged user could indirectly obtain control over a sensitive object and explain how to remediate the relationship.**

---

# I. Attack Paths & BloodHound Concepts

### 81.

**What is an Active Directory attack path?**

### 82.

**Why is thinking in attack paths more useful than looking at permissions individually?**

### 83.

**How can relationships between users, groups, computers, and permissions form an attack graph?**

### 84.

**What is the conceptual purpose of tools such as BloodHound?**

### 85.

**What types of relationships can an AD attack-path analysis reveal?**

### 86.

**Why can an apparently low-risk permission become dangerous when chained with other relationships?**

### 87.

**How can defenders use attack-path analysis proactively?**

### 88.

**How can Red Teamers use attack-path analysis during authorized assessments?**

### 89.

**How should an organization prioritize remediation when hundreds of attack paths exist?**

### 90.

**Given a fictional AD graph, identify the shortest and most dangerous paths from a standard user to a highly privileged identity and explain each relationship.**

---

# J. Windows Privilege Escalation in AD

### 91.

**How can local privilege escalation contribute to an Active Directory attack path?**

### 92.

**Why can compromising a workstation administrator account become an enterprise security problem?**

### 93.

**How can local administrative privileges lead to access to domain resources?**

### 94.

**Why is credential exposure on workstations particularly dangerous in domain environments?**

### 95.

**How can shared administrative practices create privilege-escalation opportunities?**

### 96.

**Why should ordinary users and privileged administrators avoid sharing the same workstation?**

### 97.

**How can endpoint hardening reduce AD privilege-escalation opportunities?**

### 98.

**How can defenders identify unusual privilege escalation followed by domain authentication?**

### 99.

**How can workstation compromise become the starting point for broader domain compromise?**

### 100.

**Design a defensive architecture that limits the ability of a compromised workstation to become an entry point into privileged Active Directory infrastructure.**

---

# K. Lateral Movement

### 101.

**What is lateral movement in an Active Directory environment?**

### 102.

**Why is lateral movement one of the most important phases of enterprise attacks?**

### 103.

**What role do administrative privileges play in lateral movement?**

### 104.

**How can authentication relationships enable movement between systems?**

### 105.

**Why should administrative access between workstations be considered suspicious?**

### 106.

**How can Windows remote-management mechanisms become part of an attack path?**

### 107.

**What evidence might lateral movement leave in endpoint, authentication, and network telemetry?**

### 108.

**How can defenders distinguish normal administration from suspicious lateral movement?**

### 109.

**How can network segmentation limit lateral movement?**

### 110.

**Design a detection strategy for identifying suspicious lateral movement across domain-joined Windows systems.**

---

# L. Group Policy Security

### 111.

**Why is Group Policy security-critical in an Active Directory environment?**

### 112.

**How can excessive permissions over Group Policy create security risk?**

### 113.

**Why should modifications to security-sensitive GPOs be monitored?**

### 114.

**How can attackers abuse poorly designed administrative delegation involving Group Policy?**

### 115.

**How can defenders audit GPO permissions?**

### 116.

**Why should administrators minimize unnecessary ability to modify domain-wide policies?**

### 117.

**How can GPO configuration contribute to endpoint hardening?**

### 118.

**How can a malicious or unauthorized GPO change affect many systems simultaneously?**

### 119.

**How can SOC analysts detect suspicious changes to important Group Policies?**

### 120.

**Design a secure Group Policy management model using least privilege, change control, auditing, and monitoring.**

---

# M. Delegation & Privileged Relationships

### 121.

**What is delegation in Active Directory, and why is it necessary?**

### 122.

**What is unconstrained delegation conceptually?**

### 123.

**Why can excessive delegation create significant security risk?**

### 124.

**What is constrained delegation?**

### 125.

**How does resource-based constrained delegation differ conceptually from traditional delegation models?**

### 126.

**Why should delegation configurations be regularly reviewed?**

### 127.

**How can attackers abuse poorly configured delegation relationships?**

### 128.

**What telemetry can help defenders identify suspicious use of delegation?**

### 129.

**How should an organization identify and remediate unnecessary delegation?**

### 130.

**Compare the security implications of different Active Directory delegation models and explain how an organization should minimize delegation-related attack paths.**

---

# N. Trust Relationships

### 131.

**What is an Active Directory trust?**

### 132.

**Why would an organization create a trust between domains or forests?**

### 133.

**How can trusts expand an organization's attack surface?**

### 134.

**What is the security significance of transitive trust relationships?**

### 135.

**Why should organizations document all domain and forest trusts?**

### 136.

**How can excessive trust relationships create unexpected privilege paths?**

### 137.

**How can a compromised trusted environment affect another environment?**

### 138.

**How should defenders monitor changes to trust relationships?**

### 139.

**How can Red Teamers evaluate trust relationships during an authorized assessment?**

### 140.

**Design a trust-management strategy that allows legitimate business connectivity while minimizing unnecessary security exposure.**

---

# O. AD Attack Detection, Hardening & Incident Response

### 141.

**How can a SOC detect suspicious Active Directory enumeration?**

### 142.

**How can defenders detect abnormal privilege changes within the directory?**

### 143.

**How can authentication anomalies reveal possible credential compromise?**

### 144.

**How can defenders detect suspicious Kerberos activity?**

### 145.

**How can defenders identify potential lateral movement within a domain?**

### 146.

**What telemetry should be collected from domain controllers for effective security monitoring?**

### 147.

**How should a SOC investigate suspected compromise of a privileged AD account?**

### 148.

**How should an organization respond if it suspects that an attacker has obtained high-level domain privileges?**

### 149.

**How can attack-path analysis, detection engineering, and hardening work together to continuously reduce AD risk?**

### 150.

**Design a complete Active Directory security program connecting enumeration, attack-path analysis, identity protection, Kerberos security, privilege management, lateral-movement detection, monitoring, incident response, and continuous hardening.**

---

# The Level 2 AD Mental Model

Level 1:

```text
AD ARCHITECTURE
      ↓
Users
Groups
Computers
OUs
GPO
DNS
Domains
DCs
Authentication
```

Level 2:

```text
                ACTIVE DIRECTORY
                       │
                       ▼
                  ENUMERATION
                       │
                       ▼
             IDENTITIES + RESOURCES
                       │
                       ▼
             PERMISSIONS + RELATIONSHIPS
                       │
                       ▼
                  ATTACK PATHS
                       │
          ┌────────────┴────────────┐
          ▼                         ▼
      RED TEAM                  BLUE TEAM
          │                         │
          ▼                         ▼
      Exploit                    Detect
          │                         │
          ▼                         ▼
 Privilege Escalation          Investigate
          │                         │
          ▼                         ▼
 Lateral Movement              Contain
          │                         │
          └────────────┬────────────┘
                       ▼
                 HARDEN / IMPROVE
```

The critical shift is:

> **Don't think of Active Directory as individual accounts and computers. Think of it as a graph of identities, permissions, trust, authentication, and resources.**

---

# The AD Attack-Path Mindset

Suppose we have:

```text
Standard User
      │
      ▼
Workstation
      │
      ▼
Local Admin
      │
      ▼
Credential Exposure
      │
      ▼
Privileged Account
      │
      ▼
Server Access
      │
      ▼
Domain-Level Privilege
```

Level 1 asks:

> **What is a user?**

Level 2 asks:

> **What relationships allow this user to reach that privileged account?**

Level 3 will eventually ask:

> **How can the entire domain be compromised, how can we detect it, and how do we architect the environment so the attack path doesn't exist?**

---

# The Most Important Level 2 Questions

Whenever you encounter an AD environment, train yourself to automatically ask:

### Identity

> **Who am I?**

### Privilege

> **What can I control?**

### Relationships

> **Who can control me?**

### Access

> **What can I access?**

### Trust

> **What trusts what?**

### Authentication

> **How do identities authenticate?**

### Delegation

> **Where can identity authority be delegated?**

### Attack Path

> **Can these relationships be chained together?**

### Detection

> **What evidence would this activity create?**

### Defense

> **How do we break the chain?**

That last question is particularly important for us.

---

# Red Team ↔ Blue Team AD Loop

Our AD learning should eventually operate like this:

```text
              RED TEAM
                  │
                  ▼
          Enumerate AD
                  │
                  ▼
          Find Attack Path
                  │
                  ▼
         Validate Weakness
                  │
                  ▼
              BLUE TEAM
                  │
                  ▼
        What telemetry exists?
                  │
                  ▼
        Was the activity detected?
                  │
                  ▼
         Investigate the chain
                  │
                  ▼
       Identify detection gap
                  │
                  ▼
            Harden AD
                  │
                  ▼
         PURPLE TEAM TEST
                  │
                  ▼
         Validate the defense
                  │
                  └──────────► Repeat
```

This is exactly the kind of **learn-by-doing feedback loop** we want throughout our cybersecurity journey.

---

# Level 1 → Level 2

| Level 1                 | Level 2                                   |
| ----------------------- | ----------------------------------------- |
| What is AD?             | How do we enumerate AD?                   |
| What is a user?         | What can the user access?                 |
| What is a group?        | What privileges does membership provide?  |
| What is an OU?          | What security policies affect it?         |
| What is Kerberos?       | How can Kerberos be abused/detected?      |
| What is an SPN?         | Why does an SPN matter to security?       |
| What is an ACL?         | Can the ACL create an attack path?        |
| What is GPO?            | Can GPO permissions be abused?            |
| What is delegation?     | Can delegation create privilege paths?    |
| What is a trust?        | Does the trust expand the attack surface? |
| What is authentication? | Can authentication be abused?             |
| What is a DC?           | How do we protect and monitor it?         |

---

# What Level 2 Should Prepare You For

After Level 2, you should be able to look at something like:

```text
User A
  │
  ├── Member of Group X
  │        │
  │        ▼
  │     Permission Y
  │        │
  │        ▼
  │     Computer B
  │        │
  │        ▼
  │     Service Account
  │        │
  │        ▼
  │     Server C
  │        │
  │        ▼
  │    Privileged Group
  │        │
  │        ▼
  │   Domain Controller
```

and immediately think:

> **“This isn't six unrelated objects. This is an attack path.”**

Then from the Blue Team side:

> **“What logs would show each transition?”**

And from the security architecture side:

> **“Which relationship should we remove to break the path?”**

That is the real purpose of **Active Directory Level 2**.

Level 3 will take this much further into **advanced Kerberos abuse, delegation attacks, credential theft, domain compromise, persistence, advanced attack-path analysis, AD-specific detection engineering, incident response, and enterprise hardening**.
