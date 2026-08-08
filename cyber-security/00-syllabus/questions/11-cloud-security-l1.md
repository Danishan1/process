Absolutely. **Cloud Security — Level 1** starts a new branch of our roadmap, but we should deliberately connect it to everything we've already learned.

Our progression will be:

> **Level 1 — Understand Cloud Architecture, IAM, Networking & Security Fundamentals**
> **Level 2 — Analyze Cloud Environments, Misconfigurations & Attack Paths**
> **Level 3 — Advanced Cloud Attacks, Detection, Incident Response & Cloud Security Architecture**

For our career priority of **Red Team / Pentesting > Blue Team / SOC > Cloud Security > AppSec**, Cloud Security should not be treated as a completely separate subject.

The core idea is:

> **Traditional security asks: “What machine or network can I compromise?”**
> **Cloud security increasingly asks: “What identity, permission, service, API, or trust relationship can I abuse?”**

So Level 1 will build the foundation we need before moving into cloud attack paths.

# CLOUD SECURITY — LEVEL 1

## Foundational Question Bank — 150 Questions

---

# A. Cloud Computing Fundamentals

### 1.

**What is cloud computing, and what problem does it solve for organizations?**

### 2.

**How is cloud computing different from traditional on-premises infrastructure?**

### 3.

**What are the major characteristics that distinguish cloud computing from conventional hosting?**

### 4.

**What are Infrastructure as a Service (IaaS), Platform as a Service (PaaS), and Software as a Service (SaaS)?**

### 5.

**How does the security responsibility change between IaaS, PaaS, and SaaS?**

### 6.

**What is the difference between public, private, hybrid, and multi-cloud environments?**

### 7.

**Why do organizations adopt cloud infrastructure instead of maintaining everything on-premises?**

### 8.

**What security advantages can cloud environments provide compared with traditional infrastructure?**

### 9.

**What new security risks can cloud adoption introduce?**

### 10.

**Compare the security responsibilities and attack surfaces of a traditional data center and a cloud environment.**

---

# B. Shared Responsibility Model

### 11.

**What is the cloud shared responsibility model?**

### 12.

**Why does moving infrastructure to the cloud not automatically transfer all security responsibilities to the cloud provider?**

### 13.

**Which security responsibilities typically remain with the customer in an IaaS environment?**

### 14.

**How does the customer's responsibility change when using PaaS?**

### 15.

**Why is SaaS security responsibility different from IaaS security responsibility?**

### 16.

**How can misunderstanding the shared responsibility model create security vulnerabilities?**

### 17.

**Who is responsible for protecting cloud credentials?**

### 18.

**Who is generally responsible for securing customer data stored in the cloud?**

### 19.

**Why should security teams document their responsibilities across cloud services?**

### 20.

**Given a hypothetical cloud deployment, divide the security responsibilities between the provider and customer and explain the consequences of getting the boundary wrong.**

---

# C. Cloud Providers & Global Architecture

### 21.

**What is a cloud provider?**

### 22.

**What are regions and availability zones?**

### 23.

**Why do cloud providers distribute infrastructure across multiple geographic regions?**

### 24.

**How can availability zones improve resilience?**

### 25.

**What security considerations arise when choosing a cloud region?**

### 26.

**What is the relationship between a cloud account, subscription, project, or tenant and the resources inside it?**

### 27.

**Why should organizations separate production and development cloud environments?**

### 28.

**How can multi-account or multi-subscription architectures improve security?**

### 29.

**How can poor cloud-environment organization create security problems?**

### 30.

**Design a basic multi-environment cloud structure separating development, testing, and production workloads.**

---

# D. Cloud Identity & IAM

### 31.

**What is Identity and Access Management (IAM) in cloud computing?**

### 32.

**Why is IAM one of the most important components of cloud security?**

### 33.

**How does cloud IAM differ conceptually from traditional Active Directory?**

### 34.

**What is a cloud identity?**

### 35.

**What is a user identity?**

### 36.

**What is a service identity or workload identity?**

### 37.

**What is a role in a cloud IAM system?**

### 38.

**What is a policy in cloud IAM?**

### 39.

**How do identities, roles, policies, and resources interact?**

### 40.

**Design a basic cloud IAM model for an organization containing developers, security analysts, administrators, applications, and automated workloads.**

---

# E. Authentication & Authorization

### 41.

**What is authentication in a cloud environment?**

### 42.

**What is authorization?**

### 43.

**What is the difference between authentication and authorization in cloud security?**

### 44.

**Why is successful authentication not equivalent to having unrestricted access?**

### 45.

**What is Multi-Factor Authentication (MFA)?**

### 46.

**Why is MFA particularly important for privileged cloud accounts?**

### 47.

**What is Single Sign-On (SSO)?**

### 48.

**How can federation allow users to authenticate to cloud services using an external identity provider?**

### 49.

**How can weak authentication controls contribute to cloud compromise?**

### 50.

**Design a secure authentication strategy for cloud administrators, employees, contractors, and automated workloads.**

---

# F. Least Privilege & IAM Security

### 51.

**What is the principle of least privilege?**

### 52.

**Why is least privilege particularly important in cloud environments?**

### 53.

**What is excessive permission, and why is it dangerous?**

### 54.

**How can a seemingly harmless permission become dangerous when combined with other permissions?**

### 55.

**Why should administrators avoid using highly privileged accounts for routine tasks?**

### 56.

**Why should application workloads receive only the permissions they actually require?**

### 57.

**How can organizations identify unused cloud permissions?**

### 58.

**Why should IAM policies be reviewed periodically?**

### 59.

**How can privilege accumulation occur in cloud environments?**

### 60.

**Given a hypothetical cloud IAM policy, identify excessive permissions and redesign it according to least privilege.**

---

# G. Cloud Networking Fundamentals

### 61.

**What is a Virtual Private Cloud (VPC) or equivalent virtual network?**

### 62.

**Why do cloud environments need virtual networking?**

### 63.

**What is a subnet?**

### 64.

**What is the difference between a public and private subnet?**

### 65.

**What is an Internet Gateway or equivalent internet connectivity mechanism?**

### 66.

**What is a route table?**

### 67.

**How does routing work within a cloud virtual network?**

### 68.

**What is network segmentation in the cloud?**

### 69.

**Why should sensitive workloads not automatically be placed on publicly accessible networks?**

### 70.

**Design a basic cloud network containing public-facing services, private application servers, databases, and administrative infrastructure.**

---

# H. Cloud Network Security

### 71.

**What is a cloud security group or virtual firewall?**

### 72.

**How does a security group control network access?**

### 73.

**What is the difference between inbound and outbound network rules?**

### 74.

**Why can unrestricted inbound access create serious security risk?**

### 75.

**Why can unrestricted outbound access also be dangerous?**

### 76.

**What is network ACL-based filtering?**

### 77.

**How do security groups and network ACLs differ conceptually?**

### 78.

**Why should cloud network rules follow least privilege?**

### 79.

**How can overly permissive network rules expose cloud workloads?**

### 80.

**Given a fictional cloud network, identify unnecessary exposure and redesign the network controls according to least privilege.**

---

# I. Cloud Compute

### 81.

**What is a cloud virtual machine?**

### 82.

**How does a cloud VM differ from a traditional physical server?**

### 83.

**What security responsibilities remain with the customer when running cloud virtual machines?**

### 84.

**Why should cloud operating systems still be patched and hardened?**

### 85.

**How can exposed management interfaces create security risks?**

### 86.

**Why should administrative access to cloud servers be restricted?**

### 87.

**How can compromised cloud credentials affect virtual machines?**

### 88.

**Why should production workloads use hardened machine images?**

### 89.

**How can security groups and IAM work together to protect cloud compute resources?**

### 90.

**Design a secure baseline for deploying a Linux or Windows virtual machine in the cloud.**

---

# J. Cloud Storage

### 91.

**What is object storage?**

### 92.

**How is object storage different from a traditional filesystem?**

### 93.

**What is a storage bucket or equivalent object-storage container?**

### 94.

**Why can cloud storage misconfiguration create serious security exposure?**

### 95.

**What does public access mean in the context of cloud object storage?**

### 96.

**Why should sensitive storage resources generally avoid unnecessary public exposure?**

### 97.

**How does IAM affect access to cloud storage?**

### 98.

**Why is encryption important for cloud storage?**

### 99.

**How can logging help detect suspicious access to sensitive storage?**

### 100.

**Design a secure cloud-storage architecture for confidential organizational documents, including access control, encryption, logging, and backup.**

---

# K. Cloud Databases

### 101.

**What is a managed cloud database?**

### 102.

**How does a managed database differ from running a database yourself on a virtual machine?**

### 103.

**What security responsibilities remain with the customer when using a managed database?**

### 104.

**Why should databases normally remain inaccessible directly from the public internet?**

### 105.

**How should applications authenticate to cloud databases securely?**

### 106.

**Why should database credentials not be hard-coded into application source code?**

### 107.

**What is encryption at rest?**

### 108.

**What is encryption in transit?**

### 109.

**How can database access logging help security teams?**

### 110.

**Design a secure architecture for a cloud-hosted web application and database where the database is isolated from direct public access.**

---

# L. Encryption & Key Management

### 111.

**Why is encryption important in cloud security?**

### 112.

**What is encryption at rest?**

### 113.

**What is encryption in transit?**

### 114.

**What is symmetric encryption?**

### 115.

**What is asymmetric encryption?**

### 116.

**What is a cryptographic key?**

### 117.

**What is a Key Management Service (KMS)?**

### 118.

**Why should cryptographic keys be protected separately from the data they protect?**

### 119.

**What security risks arise from poor key management?**

### 120.

**Design a basic cloud key-management strategy covering key creation, access control, rotation, monitoring, backup, and revocation.**

---

# M. Secrets & Credential Management

### 121.

**What is a secret in a cloud environment?**

### 122.

**What kinds of information should be treated as secrets?**

### 123.

**Why are API keys and access tokens security-sensitive?**

### 124.

**Why should secrets not be stored directly inside source-code repositories?**

### 125.

**What is a secrets-management service?**

### 126.

**How can applications retrieve secrets without exposing them unnecessarily?**

### 127.

**Why should cloud credentials be rotated?**

### 128.

**How can exposed credentials lead to cloud-account compromise?**

### 129.

**How can defenders detect potentially compromised cloud credentials?**

### 130.

**Design a secure secrets-management lifecycle for developers and cloud workloads.**

---

# N. Cloud Logging & Monitoring

### 131.

**Why is logging essential for cloud security?**

### 132.

**What is an audit log?**

### 133.

**What types of cloud activities should security teams monitor?**

### 134.

**Why should IAM changes receive special monitoring?**

### 135.

**Why should creation or deletion of cloud resources be logged?**

### 136.

**Why should changes to network security controls be monitored?**

### 137.

**How can authentication logs reveal compromised cloud identities?**

### 138.

**Why should cloud logs be protected against unauthorized modification or deletion?**

### 139.

**How can cloud logs be integrated with a SIEM or SOC?**

### 140.

**Design a basic cloud-security monitoring strategy covering identity, compute, networking, storage, databases, and administrative activity.**

---

# O. Cloud Security Architecture & Cybersecurity Perspective

### 141.

**Why is identity often considered the primary security boundary in modern cloud environments?**

### 142.

**How can a compromised cloud identity be more dangerous than a compromised individual server?**

### 143.

**How can cloud IAM permissions create attack paths similar to Active Directory privilege relationships?**

### 144.

**How can publicly exposed cloud resources become initial access points for attackers?**

### 145.

**How can cloud misconfigurations create security vulnerabilities without requiring exploitation of a software bug?**

### 146.

**How can attackers move from a compromised cloud workload toward higher privileges?**

### 147.

**How can Blue Teams detect suspicious cloud activity using identity, API, network, and resource telemetry?**

### 148.

**How can Red Teamers safely assess cloud environments without causing unintended outages or data exposure?**

### 149.

**How should an organization continuously assess its cloud environment for excessive permissions, exposed resources, weak configurations, and suspicious activity?**

### 150.

**Design a secure cloud architecture combining IAM, MFA, least privilege, virtual networking, compute security, storage security, encryption, secrets management, logging, monitoring, and incident response.**

---

# The Cloud Security Mental Model

At Level 1, don't memorize cloud services.

Build this mental model:

```text
                         CLOUD
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
     IDENTITY          NETWORK             DATA
        │                  │                  │
    Users/Roles        VPC/Subnets       Storage/DB
    Policies           Routes            Encryption
    MFA                Firewalls         Keys
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
                           ▼
                       COMPUTE
                           │
                    VMs / Containers
                    / Serverless
                           │
                           ▼
                      APPLICATIONS
                           │
                           ▼
                     LOGGING / API
                           │
                           ▼
                       MONITORING
```

And above everything:

```text
                SHARED RESPONSIBILITY
                        │
             ┌──────────┴──────────┐
             ▼                     ▼
          PROVIDER              CUSTOMER
             │                     │
       Cloud infrastructure    Identity
       Physical security       Configuration
       Core services           Data
                               Workloads
                               Access
```

---

# The Most Important Cloud Security Shift

This is where Cloud Security starts connecting directly with our **Active Directory** knowledge.

In AD, we learned:

```text
User
  ↓
Group
  ↓
Permission
  ↓
Resource
  ↓
Privilege
  ↓
Attack Path
```

Cloud:

```text
Identity
   ↓
Role / Policy
   ↓
Permission
   ↓
Cloud Resource
   ↓
Privilege
   ↓
Attack Path
```

The concepts are remarkably similar.

The major difference is that cloud environments dramatically expand the number of:

* identities
* APIs
* services
* temporary credentials
* machine identities
* trust relationships
* remotely accessible resources
* automated workloads

So eventually our question becomes:

> **“Who can do what, to which resource, through which identity or API?”**

That is the foundation of **Cloud Attack Path Analysis**.

---

# Red Team Perspective

At Level 1, you should start looking at cloud infrastructure and asking:

```text
What is exposed?
      ↓
Who can access it?
      ↓
What permissions exist?
      ↓
Are those permissions excessive?
      ↓
Can identities be chained?
      ↓
Can a compromised workload obtain more privileges?
      ↓
Can the attacker reach sensitive data?
```

Not:

> *“Which cloud hacking tool should I learn?”*

The architecture comes first.

---

# Blue Team Perspective

At the same time:

```text
Who authenticated?
       ↓
What API did they call?
       ↓
From where?
       ↓
What resource did they access?
       ↓
What permissions changed?
       ↓
Was a new identity created?
       ↓
Was a security control modified?
       ↓
Does this behavior make sense?
```

This will eventually lead us naturally into **Cloud SOC / Cloud Detection Engineering**.

---

# Cloud Level 1 → Level 2 → Level 3

| Area              | Level 1        | Level 2                    | Level 3                     |
| ----------------- | -------------- | -------------------------- | --------------------------- |
| Cloud             | Understand     | Assess                     | Architect                   |
| IAM               | Understand     | Find excessive permissions | Advanced identity security  |
| Authentication    | MFA/SSO        | Analyze                    | Identity attack defense     |
| Networking        | VPC/subnets    | Analyze exposure           | Advanced segmentation       |
| Compute           | Understand     | Harden/assess              | Advanced workload security  |
| Storage           | Understand     | Find exposure              | Data-security architecture  |
| Databases         | Understand     | Assess                     | Advanced protection         |
| Encryption        | Understand     | Implement                  | Key-management architecture |
| Secrets           | Understand     | Secure                     | Secret-abuse detection      |
| Logging           | Understand     | Analyze                    | Detection engineering       |
| APIs              | Understand     | Assess                     | API attack paths            |
| Cloud Identity    | Basic          | Attack paths               | Advanced identity security  |
| Red Team          | Recon concepts | Cloud assessment           | Adversary simulation        |
| Blue Team         | Logging        | Detection                  | Threat hunting              |
| Incident Response | Basic          | Investigate                | Cloud compromise response   |
| Architecture      | Basic          | Harden                     | Enterprise cloud security   |

---

# The Questions We Should Automatically Ask

Whenever we encounter a cloud environment, train yourself to ask these **10 questions**:

### 1. Who are the identities?

```text
Users
Roles
Service Accounts
Workload Identities
```

### 2. What can they access?

```text
VMs
Storage
Databases
APIs
Secrets
Networks
```

### 3. What can they change?

```text
IAM
Security Groups
Routes
Resources
Logging
```

### 4. What is publicly exposed?

```text
Internet
Public IPs
Public Storage
Public APIs
Management Interfaces
```

### 5. Where are the trust relationships?

```text
Identity → Role
Role → Resource
Service → Service
Account → Account
Cloud → Cloud
```

### 6. Where are the credentials?

```text
Passwords
API Keys
Tokens
Secrets
Certificates
Workload Credentials
```

### 7. What happens if one identity is compromised?

**This is our cloud attack-path question.**

### 8. What happens if one workload is compromised?

**Can it obtain credentials or metadata that lead elsewhere?**

### 9. What evidence would the attack create?

**API logs + identity logs + network telemetry + endpoint telemetry.**

### 10. How do we break the attack path?

**Least privilege + segmentation + strong authentication + monitoring + secure architecture.**

---

# The Connection to Our Overall Roadmap

Cloud Security now sits naturally after AD:

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
     ├───────────────┐
     ▼               ▼
RED TEAM         BLUE TEAM
     │               │
     └───────┬───────┘
             ▼
     ACTIVE DIRECTORY
             │
             ▼
       IDENTITY SECURITY
             │
             ▼
       CLOUD SECURITY
             │
       ┌─────┼─────┐
       ▼     ▼     ▼
      IAM  Network  Data
       │     │      │
       └─────┼──────┘
             ▼
       CLOUD ATTACK PATHS
             │
             ▼
      CLOUD RED + BLUE
```

The big idea we're building toward is:

> **On-premise security taught us machines, networks, Windows, and Active Directory. Cloud Security adds identity, APIs, managed services, automation, and distributed infrastructure to that same security model.**

And eventually, when we reach **Cloud Security Level 2**, we'll move from:

**“What is IAM?”**

to:

**“How do I analyze an IAM policy?”**

**“Can these permissions be chained?”**

**“What happens if this workload identity is compromised?”**

**“Can this exposed storage resource become an initial-access point?”**

**“Can a low-privileged identity escalate to a privileged role?”**

**“What logs would the attacker generate?”**

And Level 3 will take that into **advanced cloud attack paths, cloud privilege escalation, identity abuse, workload compromise, persistence, cloud detection engineering, threat hunting, incident response, and enterprise cloud security architecture.**
