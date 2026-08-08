Absolutely. **Active Directory — Level 1** is especially important for our roadmap because it sits directly between our **Red Team/VAPT** and **Blue Team/SOC** knowledge.

We are not going to learn AD as a list of commands. First, we need to understand the **architecture, identity model, authentication, authorization, objects, policies, DNS, domains, forests, and security boundaries**.

The Level 1 objective is:

> **Understand how an enterprise Active Directory environment is structured, how identities interact with it, and where security boundaries exist.**

Later, Level 2 will move into **enumeration, attack paths, privilege escalation, Kerberos, delegation, credential attacks, and lateral movement**, while Level 3 will focus on **advanced AD attacks, domain compromise, detection, hardening, and attack-path analysis**.

# ACTIVE DIRECTORY — LEVEL 1

## Foundational Question Bank — 150 Questions

---

# A. Active Directory Fundamentals

### 1.

**What is Active Directory, and what problem does it solve for organizations?**

### 2.

**Why do enterprises use centralized identity management instead of maintaining separate accounts on every computer?**

### 3.

**What is a directory service?**

### 4.

**How is Active Directory different from a simple database of usernames and passwords?**

### 5.

**What types of resources can Active Directory manage?**

### 6.

**What is an Active Directory domain?**

### 7.

**What is the purpose of a domain controller?**

### 8.

**What is the difference between a domain controller and an ordinary Windows server?**

### 9.

**What is an Active Directory object?**

### 10.

**Why is understanding Active Directory architecture important for cybersecurity professionals?**

---

# B. Active Directory Objects

### 11.

**What is a user object in Active Directory?**

### 12.

**What is a computer object?**

### 13.

**What is a group object?**

### 14.

**What is an Organizational Unit (OU)?**

### 15.

**What is a contact object, and how does it differ from a user object?**

### 16.

**What information can be stored as attributes of an Active Directory object?**

### 17.

**Why are object attributes important from a security perspective?**

### 18.

**How are users and computers represented within Active Directory?**

### 19.

**Why are object relationships important when analyzing an Active Directory environment?**

### 20.

**Given a fictional enterprise containing users, computers, servers, groups, and OUs, explain how you would organize these objects logically.**

---

# C. Domains

### 21.

**What is an Active Directory domain?**

### 22.

**Why is a domain considered an identity and administrative boundary?**

### 23.

**What is a domain name in Active Directory?**

### 24.

**How does a domain differ from a workgroup?**

### 25.

**Why are domains useful for centralized authentication?**

### 26.

**How does a domain controller participate in domain authentication?**

### 27.

**Can an organization have multiple Active Directory domains? Why might it do so?**

### 28.

**What security implications arise when multiple domains exist?**

### 29.

**How does domain membership affect a Windows computer?**

### 30.

**Explain the lifecycle of a Windows workstation joining an Active Directory domain.**

---

# D. Domain Controllers

### 31.

**What is a Domain Controller (DC)?**

### 32.

**What responsibilities does a domain controller perform?**

### 33.

**Why are domain controllers high-value security assets?**

### 34.

**What happens when a domain controller becomes unavailable?**

### 35.

**Why do organizations typically deploy multiple domain controllers?**

### 36.

**What is replication between domain controllers?**

### 37.

**Why is replication necessary in Active Directory?**

### 38.

**What security risks could result from poorly secured domain controllers?**

### 39.

**Why should administrative access to domain controllers be heavily restricted?**

### 40.

**Design a basic secure architecture for domain controllers in a medium-sized enterprise.**

---

# E. Active Directory Database & Directory Structure

### 41.

**What is the Active Directory database?**

### 42.

**What type of information is stored within the directory database?**

### 43.

**Why is the directory database security-sensitive?**

### 44.

**What is the logical structure of Active Directory?**

### 45.

**What is the relationship between domains, OUs, users, computers, and groups?**

### 46.

**Why does Active Directory use a hierarchical structure?**

### 47.

**How does the hierarchical structure help administrators manage large organizations?**

### 48.

**Why does understanding directory structure help security professionals identify attack paths?**

### 49.

**What would happen if an attacker obtained highly privileged access to a domain controller?**

### 50.

**Explain how the logical structure of Active Directory represents an organization's identity environment.**

---

# F. Organizational Units

### 51.

**What is an Organizational Unit (OU)?**

### 52.

**Why are OUs useful for enterprise administration?**

### 53.

**How are OUs different from security groups?**

### 54.

**Why should OUs generally be designed around administrative requirements rather than simply organizational charts?**

### 55.

**How can OUs influence the application of Group Policy?**

### 56.

**Can an OU contain another OU? Why is this useful?**

### 57.

**How can poor OU design create security-management problems?**

### 58.

**Why might workstations and servers be placed in separate OUs?**

### 59.

**Why might privileged administrative accounts require separate organizational structures?**

### 60.

**Design a sensible OU structure for a company containing employees, workstations, servers, administrators, and branch offices.**

---

# G. Active Directory Groups

### 61.

**What is a security group?**

### 62.

**Why are groups important for access control?**

### 63.

**What is the difference between assigning permissions directly to users and assigning them through groups?**

### 64.

**Why is group-based access management generally easier to maintain?**

### 65.

**What is a group membership relationship?**

### 66.

**Why can nested group memberships become security-sensitive?**

### 67.

**How can excessive group membership create privilege-escalation opportunities?**

### 68.

**Why should privileged groups receive special monitoring?**

### 69.

**How can an analyst determine the effective privileges of a user who belongs to multiple groups?**

### 70.

**Design a group-management strategy following least privilege for a medium-sized enterprise.**

---

# H. Authentication Fundamentals

### 71.

**What is authentication?**

### 72.

**What is authorization?**

### 73.

**What is the difference between authentication and authorization?**

### 74.

**How does a domain user authenticate to Active Directory?**

### 75.

**What role does a domain controller play during authentication?**

### 76.

**Why is centralized authentication valuable to organizations?**

### 77.

**What happens when a user enters domain credentials on a Windows workstation?**

### 78.

**Why should authentication events be logged and monitored?**

### 79.

**What types of authentication failures might indicate security problems?**

### 80.

**Explain the complete conceptual flow of a user authenticating to a domain-joined Windows computer.**

---

# I. Kerberos Fundamentals

### 81.

**What is Kerberos?**

### 82.

**Why does Active Directory use Kerberos as an important authentication protocol?**

### 83.

**What is the basic security problem Kerberos is designed to solve?**

### 84.

**What is a ticket in the Kerberos authentication model?**

### 85.

**What is the purpose of the Ticket Granting Ticket (TGT)?**

### 86.

**What is a service ticket?**

### 87.

**What is the role of the Key Distribution Center (KDC)?**

### 88.

**What are the Authentication Service and Ticket Granting Service components conceptually responsible for?**

### 89.

**Why is time synchronization important for Kerberos authentication?**

### 90.

**Explain the conceptual flow from a user's initial authentication to accessing a service using Kerberos.**

---

# J. NTLM & Authentication Security

### 91.

**What is NTLM, and why does it still matter when Kerberos is widely used?**

### 92.

**How is NTLM conceptually different from Kerberos?**

### 93.

**Why can legacy authentication protocols create security risks?**

### 94.

**Why should organizations understand where NTLM is still being used?**

### 95.

**How can authentication-protocol weaknesses affect an organization's attack surface?**

### 96.

**Why is protocol downgrade behavior security-relevant?**

### 97.

**How can defenders identify legacy authentication activity?**

### 98.

**Why should organizations gradually reduce unnecessary dependence on legacy authentication?**

### 99.

**How can Red Teamers and Blue Teamers approach authentication protocols from different perspectives?**

### 100.

**Design a security strategy for evaluating and reducing unnecessary legacy authentication within an Active Directory environment.**

---

# K. Group Policy Fundamentals

### 101.

**What is Group Policy?**

### 102.

**Why is Group Policy important in enterprise Windows administration?**

### 103.

**What types of security settings can Group Policy control?**

### 104.

**How can Group Policy enforce password and authentication requirements?**

### 105.

**How can Group Policy affect workstation security?**

### 106.

**How can Group Policy affect server security?**

### 107.

**What is the relationship between Group Policy Objects and OUs?**

### 108.

**Why can misconfigured Group Policy create security weaknesses?**

### 109.

**Why should changes to security-sensitive Group Policies be monitored?**

### 110.

**Design a baseline Group Policy strategy for securing enterprise Windows workstations and servers.**

---

# L. Active Directory Permissions

### 111.

**What does permission mean within Active Directory?**

### 112.

**How do permissions control access to directory objects?**

### 113.

**What is the difference between ownership and permission?**

### 114.

**Why can excessive permissions become an attack path?**

### 115.

**How can delegated administration be implemented safely?**

### 116.

**Why should administrators avoid granting broad permissions when narrow permissions are sufficient?**

### 117.

**How can permissions become dangerous when combined with group membership?**

### 118.

**Why should security teams periodically review delegated permissions?**

### 119.

**How can permission relationships be represented as an attack graph?**

### 120.

**Given a hypothetical AD environment, explain how you would identify excessive permissions and reduce them according to least privilege.**

---

# M. DNS & Active Directory

### 121.

**Why is DNS extremely important to Active Directory?**

### 122.

**How does Active Directory depend on DNS for service discovery?**

### 123.

**What happens if DNS is incorrectly configured in a domain environment?**

### 124.

**Why can DNS configuration affect authentication?**

### 125.

**What are SRV records, and why are they important to Active Directory?**

### 126.

**How can defenders use DNS information to understand an Active Directory environment?**

### 127.

**Why should domain DNS infrastructure be protected carefully?**

### 128.

**How can unauthorized DNS changes create security problems?**

### 129.

**How can attackers potentially use DNS information during reconnaissance, and what can defenders monitor?**

### 130.

**Design a secure DNS architecture supporting an enterprise Active Directory environment.**

---

# N. Active Directory Security Monitoring

### 131.

**Why should Active Directory activity receive dedicated security monitoring?**

### 132.

**Which categories of Active Directory events should a SOC monitor?**

### 133.

**Why are successful and failed authentication events important?**

### 134.

**Why should changes to privileged groups generate security visibility?**

### 135.

**Why are new user accounts potentially security-sensitive?**

### 136.

**Why should changes to existing user privileges be monitored?**

### 137.

**Why should domain-controller activity receive additional monitoring compared with ordinary endpoints?**

### 138.

**How can unusual authentication patterns reveal possible credential compromise?**

### 139.

**How can Active Directory telemetry be correlated with endpoint and network telemetry?**

### 140.

**Design a basic SOC monitoring strategy specifically for Active Directory.**

---

# O. AD Security Architecture & Cybersecurity Perspective

### 141.

**Why is Active Directory often considered one of the most critical identity infrastructures in an enterprise?**

### 142.

**What would happen to an organization if its central identity infrastructure were compromised?**

### 143.

**Why should privileged accounts be separated from ordinary user accounts?**

### 144.

**What is the principle of least privilege, and how should it apply to Active Directory?**

### 145.

**Why is administrative tiering important for protecting high-value identity infrastructure?**

### 146.

**Why should domain controllers be isolated from ordinary workstation activity?**

### 147.

**How can poor identity architecture make lateral movement easier for an attacker?**

### 148.

**How can Blue Teams use Active Directory relationships to identify potential attack paths?**

### 149.

**How can Red Team knowledge help a defender identify weaknesses in Active Directory architecture?**

### 150.

**Design a secure Active Directory architecture for a medium-to-large enterprise and explain how users, computers, groups, OUs, domains, domain controllers, DNS, authentication, permissions, Group Policy, and monitoring should work together to create a defensible identity environment.**

---

# The Level 1 AD Mental Model

The most important thing at this stage is to stop seeing AD as:

```text
Users
Passwords
Groups
Computers
```

Instead, see it as an **identity and trust system**:

```text
                    ACTIVE DIRECTORY
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
       IDENTITIES       RESOURCES        POLICIES
          │                │                │
      Users/Groups     Computers/Servers   GPO
          │                │                │
          └────────────────┼────────────────┘
                           │
                           ▼
                    AUTHENTICATION
                           │
                    ┌──────┴──────┐
                    ▼             ▼
                 Kerberos        NTLM
                    │
                    ▼
                 AUTHORIZATION
                    │
                    ▼
                  ACCESS
                    │
                    ▼
               TRUST RELATIONSHIPS
                    │
                    ▼
                ATTACK PATHS
```

This model will become extremely important later.

---

# The AD Security Questions We Should Automatically Ask

Whenever we encounter an Active Directory environment, train yourself to ask:

### 1. Who are the identities?

```text
Users
Groups
Service Accounts
Administrators
```

### 2. What can they access?

```text
Permissions
Resources
Shares
Applications
Servers
```

### 3. Who trusts whom?

```text
Group Membership
Delegation
Domain Trusts
Authentication
```

### 4. Who controls what?

```text
Ownership
Permissions
Administrative Rights
Group Policy
```

### 5. Where are the security boundaries?

```text
User
   ↓
Workstation
   ↓
Server
   ↓
Domain
   ↓
Forest
```

### 6. What happens if one identity is compromised?

This is the question that eventually takes us into **AD attack paths**.

---

# Level 1 → Level 2 → Level 3

Our AD progression will be:

|                    | Level 1          | Level 2               | Level 3                   |
| ------------------ | ---------------- | --------------------- | ------------------------- |
| Architecture       | Understand       | Analyze               | Design/Harden             |
| Users              | Understand       | Enumerate             | Attack/Defend             |
| Groups             | Understand       | Analyze memberships   | Attack paths              |
| OUs                | Understand       | Enumerate             | Secure architecture       |
| Permissions        | Understand       | Find excessive rights | Exploit/remediate         |
| Kerberos           | Understand       | Analyze               | Advanced attacks/defense  |
| NTLM               | Understand       | Assess                | Abuse/detection           |
| GPO                | Understand       | Analyze               | Attack/harden             |
| DNS                | Understand       | Enumerate             | Secure/attack             |
| Authentication     | Understand       | Analyze               | Advanced identity attacks |
| Domain Controllers | Understand       | Assess                | Protect/attack            |
| AD Security        | Basic            | Intermediate          | Advanced                  |
| SOC                | Basic monitoring | Detection             | Advanced AD detection     |
| Red Team           | Architecture     | Attack paths          | Domain compromise         |
| Blue Team          | Logging          | Detection             | Identity defense          |

---

# The Most Important Connection to Our Roadmap

Our learning path now starts connecting beautifully:

```text
NETWORKING
     │
     ▼
LINUX / WINDOWS
     │
     ▼
SECURITY FUNDAMENTALS
     │
     ▼
NETWORK SECURITY
     │
     ▼
RED TEAM / VAPT
     │
     ├───────────────┐
     ▼               ▼
BLUE TEAM / SOC   ACTIVE DIRECTORY
     │               │
     └───────┬───────┘
             ▼
       ATTACK + DEFENSE
             │
             ▼
        IDENTITY SECURITY
             │
             ▼
       ENTERPRISE SECURITY
```

And this is why **Active Directory is so important for our particular career goal**.

A large portion of enterprise security ultimately revolves around:

> **Identity → Authentication → Authorization → Trust → Privilege → Access**

And AD is one of the most important environments in which those concepts come together.

---

# What Level 1 Should Make You Capable Of

By the end of Level 1, when you see:

```text
Employee
   │
   ▼
Windows Workstation
   │
   ▼
Active Directory
   │
   ├── User
   ├── Groups
   ├── OUs
   ├── GPO
   ├── Domain
   ├── Domain Controller
   ├── DNS
   └── Authentication
```

you should understand **what each component is, why it exists, how they interact, and why each one matters from a security perspective**.

Then Level 2 can stop explaining *what Active Directory is* and start asking the much more interesting questions:

> **How do we enumerate it?**

> **Where are the weak relationships?**

> **How can privileges be chained?**

> **How does Kerberos actually work under attack?**

> **How can credentials be abused?**

> **How does an attacker move from one machine to another?**

> **How does a low-privileged identity become dangerous?**

> **And from the Blue Team side: how do we detect all of that?**

That is where **Active Directory becomes one of the most important technical subjects in our Red Team + Blue Team journey.**
