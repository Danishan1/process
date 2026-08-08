# Our Cybersecurity Subject Tree

First, let's establish the complete syllabus.

```text
CYBERSECURITY
│
├── 1. NETWORKING
│
├── 2. LINUX
│
├── 3. WINDOWS
│
├── 4. PYTHON
│
├── 5. WEB FUNDAMENTALS
│
├── 6. SECURITY FUNDAMENTALS
│
├── 7. NETWORK SECURITY & ANALYSIS
│
├── 8. RED TEAM / VAPT
│
├── 9. BLUE TEAM / SOC
│
├── 10. ACTIVE DIRECTORY
│
├── 11. CLOUD SECURITY
│
└── 12. APPLICATION SECURITY
```

Now let's expand each one.

## 1. NETWORKING 🔴

This is our **foundation subject**.

Almost everything else depends on it.

### 1.1 Networking Basics

* What is a network?
* LAN / WAN / MAN
* Client-server model
* Peer-to-peer
* Internet vs intranet
* Network topologies
* Network devices

  * Hub
  * Switch
  * Router
  * Firewall
  * Access Point
  * Proxy

### 1.2 Network Models

* OSI model
* TCP/IP model
* Encapsulation
* Decapsulation
* PDU

  * Frame
  * Packet
  * Segment
  * Data

### 1.3 Ethernet

* MAC address
* Ethernet frame
* NIC
* Broadcast
* Unicast
* Multicast
* Broadcast domain
* Collision domain

### 1.4 IP

* IPv4
* IPv6 basics
* Public vs private IP
* Static vs dynamic IP
* Loopback
* APIPA
* Network address
* Broadcast address
* Subnet mask
* CIDR

### 1.5 Subnetting

* Binary
* Decimal ↔ binary
* Subnet masks
* CIDR notation
* Network/host portions
* Number of hosts
* Subnet calculation
* VLSM basics

**Subnetting deserves serious practice.**

### 1.6 ARP

* ARP request
* ARP reply
* ARP cache
* MAC/IP relationship
* Gratuitous ARP

### 1.7 TCP / UDP

#### TCP

* Connection-oriented communication
* Three-way handshake
* SYN
* SYN-ACK
* ACK
* Sequence numbers
* ACK numbers
* Flags
* Connection termination
* Retransmission
* Flow control
* Congestion control

#### UDP

* Connectionless communication
* Datagram
* Use cases
* TCP vs UDP

### 1.8 Ports & Sockets

* Port numbers
* Well-known ports
* Registered ports
* Dynamic ports
* Socket
* IP + port
* Listening ports
* Established connections

Know important services:

* SSH — 22
* DNS — 53
* HTTP — 80
* HTTPS — 443
* FTP — 21
* SMTP — 25
* SMB — 445
* RDP — 3389
* LDAP — 389
* LDAPS — 636
* Kerberos — 88

Don't just memorize them.

Understand **why they exist**.

### 1.9 DNS

* Domain names
* Resolver
* Recursive DNS
* Authoritative DNS
* Root servers
* TLD
* A
* AAAA
* CNAME
* MX
* NS
* TXT
* PTR
* DNS caching
* DNS lookup process

### 1.10 DHCP

* DHCP Discover
* DHCP Offer
* DHCP Request
* DHCP ACK
* IP allocation
* Lease
* DHCP server

### 1.11 ICMP

* Echo request
* Echo reply
* Ping
* TTL
* Traceroute concept
* ICMP error messages

### 1.12 Routing

* Routing table
* Default gateway
* Static routing
* Dynamic routing concept
* Default route
* Next hop

### 1.13 Switching

* MAC address table
* VLAN
* Access port
* Trunk port
* 802.1Q
* Inter-VLAN routing

### 1.14 NAT

* Static NAT
* Dynamic NAT
* PAT
* Private → public communication

### 1.15 Network Services

* SSH
* FTP
* SMTP
* DNS
* DHCP
* HTTP
* HTTPS
* SMB
* RDP
* LDAP

### 1.16 VPN

* Purpose
* Tunneling
* Encryption
* Remote access VPN
* Site-to-site VPN

### 1.17 HTTP/HTTPS

This overlaps heavily with **Web Fundamentals**, so we study it there too.

## 2. LINUX 🔴

Linux becomes our **security laboratory operating system**.

### 2.1 Linux Fundamentals

* Linux architecture
* Kernel
* Shell
* Terminal
* Distribution
* Filesystem hierarchy

### 2.2 Filesystem

* `/`
* `/home`
* `/etc`
* `/var`
* `/tmp`
* `/usr`
* `/opt`
* `/bin`
* `/sbin`

### 2.3 Commands

* `ls`
* `cd`
* `pwd`
* `cat`
* `less`
* `head`
* `tail`
* `cp`
* `mv`
* `rm`
* `mkdir`
* `touch`

### 2.4 Searching & Processing

* `grep`
* `find`
* `awk`
* `sed`
* `sort`
* `cut`
* `uniq`
* pipes
* redirection

### 2.5 Users & Groups

* Users
* Groups
* `/etc/passwd`
* `/etc/shadow`
* `sudo`
* User management
* Group management

### 2.6 Permissions

* Read
* Write
* Execute
* Owner
* Group
* Others
* `chmod`
* `chown`
* `umask`
* Special permissions

### 2.7 Processes

* Process
* PID
* Parent/child process
* `ps`
* `top`
* `htop`
* `kill`
* Signals

### 2.8 Services

* systemd
* `systemctl`
* Service management
* Startup services

### 2.9 Networking in Linux

* `ip`
* `ss`
* `ping`
* `traceroute`
* `curl`
* `wget`
* `dig`
* `nslookup`

### 2.10 SSH

* SSH architecture
* Authentication
* Password authentication
* Key authentication
* SSH keys
* `scp`
* SSH configuration
* SSH hardening

### 2.11 Logs

* `/var/log`
* Authentication logs
* System logs
* Application logs
* `journalctl`

### 2.12 Bash

* Variables
* Conditions
* Loops
* Functions
* Arguments
* Exit codes
* Pipes
* Shell scripting

### 2.13 Linux Security

* File permissions
* User privileges
* Sudo
* Firewall
* Services
* SSH hardening
* Logging
* Updates
* Process monitoring

## 3. WINDOWS 🔴

We need Windows because enterprise security heavily depends on it.

### 3.1 Windows Fundamentals

* Windows architecture
* Kernel
* User mode
* Processes
* Services
* Filesystem

### 3.2 Users & Groups

* Local users
* Local groups
* Administrators
* Standard users
* Privileges

### 3.3 NTFS

* Files
* Directories
* NTFS permissions
* Inheritance
* Ownership

### 3.4 Processes

* Processes
* Threads
* Services
* Task Manager
* Process relationships

### 3.5 Windows Networking

* IP configuration
* DNS
* SMB
* RDP
* WinRM
* Windows Firewall

### 3.6 PowerShell

* Cmdlets
* Variables
* Objects
* Pipelines
* Scripts
* Modules
* System administration

### 3.7 Registry

* Registry structure
* Keys
* Values
* Configuration

### 3.8 Event Logs

* Event Viewer
* Security logs
* System logs
* Application logs
* Important event IDs

### 3.9 Authentication

* Windows authentication
* NTLM concept
* Kerberos concept
* Credentials
* Tokens
* Sessions

### 3.10 Windows Security

* Defender
* Firewall
* UAC
* Security policies
* Local policies
* Logging
* Updates

## 4. PYTHON 🟠

Python is our **security automation language**.

### 4.1 Programming Fundamentals

* Variables
* Data types
* Operators
* Conditions
* Loops

### 4.2 Data Structures

* Lists
* Tuples
* Sets
* Dictionaries

### 4.3 Functions

* Functions
* Arguments
* Return values
* Scope
* Lambda basics

### 4.4 Files

* Read
* Write
* Append
* CSV
* JSON

### 4.5 Error Handling

* Exceptions
* `try`
* `except`
* `finally`

### 4.6 Modules

* Importing
* Standard library
* Virtual environments
* Packages

### 4.7 Networking

* Sockets
* TCP
* UDP
* IP addresses
* Ports

### 4.8 HTTP

* Requests
* Responses
* Headers
* Parameters
* JSON APIs

### 4.9 Security Automation

* Log parsing
* Network utilities
* HTTP testing
* File analysis
* Automation

### 4.10 CLI Tools

* Arguments
* Flags
* Input validation
* Output formatting

### 4.11 Regex

* Pattern matching
* Searching
* Extraction
* Log analysis

## 5. WEB FUNDAMENTALS 🔴

This is the bridge toward **Pentesting + AppSec**.

### 5.1 Internet Architecture

* Client
* Server
* DNS
* Web server
* Application server
* Database

### 5.2 HTTP

* Request
* Response
* Methods

  * GET
  * POST
  * PUT
  * PATCH
  * DELETE
* Headers
* Status codes
* Body
* Parameters

### 5.3 HTTPS

* TLS concept
* Certificates
* Encryption
* Authentication
* HTTPS handshake concept

### 5.4 Browser

* URL
* DNS resolution
* HTTP request
* Rendering
* Cookies
* Cache
* Storage

### 5.5 Authentication

* Login
* Passwords
* Sessions
* Cookies
* Tokens
* MFA

### 5.6 Authorization

* Authentication vs authorization
* Roles
* Permissions
* Access control

### 5.7 Web Architecture

```text
Browser
   ↓
Web Server
   ↓
Application
   ↓
Database
```

Understand each layer.

### 5.8 Backend Fundamentals

* Server-side code
* APIs
* Routes
* Controllers
* Database interaction

### 5.9 Databases

* Tables
* Rows
* Columns
* Primary keys
* Foreign keys
* SQL basics
* CRUD

### 5.10 APIs

* REST
* JSON
* Endpoints
* Parameters
* Authentication
* Authorization

### 5.11 Basic JavaScript

We don't need to become frontend developers.

Learn enough to understand:

* Variables
* Functions
* DOM
* Requests
* Browser execution
* Client-side validation

## 6. SECURITY FUNDAMENTALS 🔴

This is our **security vocabulary and mental model**.

### 6.1 Core Concepts

* Asset
* Threat
* Vulnerability
* Exploit
* Risk
* Attack surface
* Exposure
* Impact

### 6.2 CIA Triad

* Confidentiality
* Integrity
* Availability

### 6.3 Security Principles

* Least privilege
* Defense in depth
* Separation of duties
* Fail secure
* Zero Trust
* Secure by design

### 6.4 Authentication

* Passwords
* MFA
* Biometrics
* Tokens
* Certificates

### 6.5 Authorization

* RBAC
* ACL
* Permissions
* Privileges

### 6.6 Cryptography

Understand the difference between:

* Encoding
* Encryption
* Hashing

Then:

* Symmetric encryption
* Asymmetric encryption
* Digital signatures
* Key exchange
* Certificates
* PKI

We focus on **understanding**, not cryptographic mathematics yet.

### 6.7 Security Controls

* Preventive
* Detective
* Corrective
* Technical
* Administrative
* Physical

### 6.8 Vulnerability Management

* Asset discovery
* Vulnerability discovery
* Risk assessment
* Prioritization
* Remediation
* Verification

### 6.9 Incident Response

* Preparation
* Detection
* Analysis
* Containment
* Eradication
* Recovery
* Lessons learned

### 6.10 Security Frameworks

At an introductory level:

* NIST Cybersecurity Framework
* MITRE ATT&CK
* OWASP
* CIS Controls

## 7. NETWORK SECURITY & ANALYSIS 🔴

Now networking becomes **security networking**.

### 7.1 Reconnaissance

* Host discovery
* Port scanning
* Service discovery
* Version detection
* Network enumeration

### 7.2 Packet Analysis

Using Wireshark/tcpdump:

* Ethernet
* ARP
* DNS
* TCP
* UDP
* ICMP
* HTTP
* TLS

### 7.3 Network Attacks — Conceptual + Lab

* ARP spoofing
* DNS attacks
* DoS concepts
* MITM concepts
* Rogue services
* Network reconnaissance

Only in controlled environments.

### 7.4 Network Defense

* Firewall
* IDS
* IPS
* Network segmentation
* VLAN
* ACL
* VPN
* Proxy

### 7.5 Traffic Analysis

Learn to recognize:

* Normal traffic
* Suspicious traffic
* Scanning
* Repeated authentication attempts
* Unusual connections

### 7.6 Network Monitoring

* Logs
* Flow data
* Packet capture
* Alerts

## 8. RED TEAM / VAPT 🔴🔴

This is our **primary career subject**, so eventually it receives the deepest treatment.

### 8.1 Security Testing Methodology

```text
Scope
 ↓
Reconnaissance
 ↓
Enumeration
 ↓
Vulnerability Discovery
 ↓
Validation
 ↓
Exploitation
 ↓
Post-Exploitation
 ↓
Reporting
```

### 8.2 Reconnaissance

* Passive reconnaissance
* Active reconnaissance
* DNS enumeration
* Host discovery
* Technology identification

### 8.3 Enumeration

* Ports
* Services
* Versions
* Users
* Shares
* Applications

### 8.4 Vulnerability Assessment

* Misconfiguration
* Outdated software
* Weak authentication
* Exposed services
* Access-control issues

### 8.5 Web VAPT

Study:

* Injection
* XSS
* Broken access control
* Authentication failures
* Session problems
* Security misconfiguration
* File upload issues
* Path traversal
* SSRF concept
* API security

### 8.6 Linux Security Testing

* Permissions
* Services
* SUID/SGID concepts
* Credentials
* Misconfigurations
* Privilege escalation concepts

### 8.7 Windows Security Testing

* Services
* Permissions
* Credentials
* Misconfigurations
* Privilege escalation concepts

### 8.8 Active Directory Security

Later connect:

* AD enumeration
* Kerberos
* LDAP
* SMB
* Privilege relationships
* Misconfigurations

### 8.9 Reporting

This is extremely important.

Learn:

* Finding
* Evidence
* Impact
* Severity
* Reproduction
* Remediation

A pentester who cannot communicate findings is incomplete.

## 9. BLUE TEAM / SOC 🔴

Our secondary career path and an essential complement to Red Team.

### 9.1 SOC Fundamentals

* SOC role
* Analyst levels
* Alert
* Event
* Incident
* Case
* Escalation

### 9.2 Logging

Understand logs from:

* Linux
* Windows
* Firewalls
* Web servers
* Applications
* Network devices

### 9.3 SIEM

Understand:

```text
Log source
   ↓
Collection
   ↓
Normalization
   ↓
Search
   ↓
Detection
   ↓
Alert
```

### 9.4 Detection

* Rules
* Signatures
* Thresholds
* Correlation
* Baselines

### 9.5 Incident Investigation

* Timeline
* Evidence
* User
* Host
* IP
* Process
* Network connection

### 9.6 IOC

* IP
* Domain
* URL
* Hash
* File
* Username

### 9.7 MITRE ATT&CK

Understand:

* Tactics
* Techniques
* Procedures
* Mapping attacker behavior to detections

### 9.8 Incident Response

* Detection
* Triage
* Investigation
* Containment
* Eradication
* Recovery

### 9.9 Threat Hunting

Later:

* Hypothesis
* Data sources
* Query
* Investigation
* Detection

### 9.10 Basic Detection Engineering

Eventually:

* Detection logic
* False positives
* Alert quality
* Tuning

## 10. ACTIVE DIRECTORY 🟠

AD deserves its own subject because it connects **Windows + Networking + Red Team + Blue Team**.

### 10.1 AD Architecture

* Domain
* Forest
* Tree
* Domain Controller
* Organizational Unit

### 10.2 Objects

* Users
* Groups
* Computers
* OUs
* GPOs

### 10.3 Protocols

* LDAP
* Kerberos
* NTLM
* DNS
* SMB
* RPC

### 10.4 Authentication

* Kerberos authentication
* Tickets
* TGT
* Service tickets
* NTLM authentication

### 10.5 Group Policy

* GPO
* Policy application
* Security settings

### 10.6 Permissions

* ACL
* DACL
* SACL
* Object permissions
* Delegation

### 10.7 AD Security

* Weak passwords
* Excessive privileges
* Misconfigured permissions
* Unnecessary services
* Credential exposure

### 10.8 AD Red Team

Later:

* Enumeration
* Attack paths
* Privilege escalation
* Lateral movement
* Credential attacks

### 10.9 AD Blue Team

* Authentication monitoring
* Event logs
* Suspicious logons
* Privilege changes
* Lateral-movement detection

## 11. CLOUD SECURITY 🟡

We don't need deep cloud knowledge immediately.

First understand **how cloud systems are built**.

### 11.1 Cloud Fundamentals

* IaaS
* PaaS
* SaaS
* Public/private/hybrid cloud
* Shared responsibility model

### 11.2 IAM

* Users
* Groups
* Roles
* Policies
* Permissions
* Least privilege

### 11.3 Cloud Networking

* VPC/VNet
* Subnets
* Routing
* Security groups
* Network ACLs

### 11.4 Compute

* Virtual machines
* Containers
* Serverless concept

### 11.5 Storage

* Object storage
* Permissions
* Encryption
* Public exposure

### 11.6 Secrets

* API keys
* Credentials
* Secret management

### 11.7 Logging

* Cloud audit logs
* Access logs
* Monitoring
* Alerts

### 11.8 Cloud Security

* Misconfiguration
* Excessive permissions
* Exposed resources
* Insecure storage
* Credential exposure

## 12. APPLICATION SECURITY 🟡

This will build on **Web + Security + Red Team + Programming**.

### 12.1 Secure Development

* Secure coding
* Input validation
* Output encoding
* Authentication
* Authorization
* Error handling
* Secrets management

### 12.2 OWASP Concepts

Study the major categories of web application risks.

Especially:

* Broken access control
* Cryptographic failures
* Injection
* Security misconfiguration
* Authentication failures
* Vulnerable components
* Logging/monitoring failures
* SSRF

### 12.3 API Security

* Authentication
* Authorization
* Rate limiting
* Input validation
* API tokens
* Access control

### 12.4 Dependency Security

* Dependencies
* Vulnerabilities
* Software composition
* Updates

### 12.5 Security Testing

* SAST concept
* DAST concept
* Dependency scanning
* Manual testing
* Code review

### 12.6 DevSecOps

Eventually:

```text
Code
 ↓
Git
 ↓
Build
 ↓
Security Testing
 ↓
Deploy
 ↓
Monitor
```

## Now the important part: WE DON'T STUDY THESE IN ORDER

This is where our new approach becomes powerful.

Instead of:

```text
Networking
   ↓
finish
   ↓
Linux
   ↓
finish
   ↓
Windows
```

we create **learning clusters**.

## CLUSTER 1 — COMPUTING FOUNDATION

We study these together:

```text
NETWORKING
    +
LINUX
    +
WINDOWS
```

Example:

#### Topic: IP addresses

We learn:

**Networking**
→ IPv4, subnetting

**Linux**
→ `ip addr`

**Windows**
→ `ipconfig`

**Security**
→ exposed interfaces

This is much better than isolated learning.

## CLUSTER 2 — NETWORK COMMUNICATION

```text
NETWORKING
     +
LINUX
     +
PYTHON
     +
WEB
```

Example:

#### Topic: TCP

Learn:

**Networking**
→ TCP handshake

**Linux**
→ `ss`

**Python**
→ TCP socket

**Web**
→ HTTP over TCP

**Security**
→ port scanning

Now TCP becomes something we can actually **see and use**.

## CLUSTER 3 — OPERATING SYSTEMS

```text
LINUX
   +
WINDOWS
   +
SECURITY
   +
BLUE TEAM
```

Example:

#### Topic: Processes

Linux:

→ `ps`

Windows:

→ Task Manager / PowerShell

Security:

→ suspicious process

Blue Team:

→ process investigation

Red Team:

→ privilege escalation concepts

## CLUSTER 4 — WEB SECURITY

```text
NETWORKING
     +
PYTHON
     +
WEB
     +
SECURITY
     +
RED TEAM
     +
APPSEC
```

Example:

#### HTTP request

Networking:

→ TCP

Web:

→ HTTP

Python:

→ `requests`

Security:

→ authentication

Red Team:

→ request manipulation

AppSec:

→ secure implementation

This becomes one of our strongest learning clusters.

## CLUSTER 5 — NETWORK SECURITY

```text
NETWORKING
      +
LINUX
      +
WIRESHARK
      +
NMAP
      +
RED TEAM
      +
BLUE TEAM
```

Example:

#### Port scanning

Networking:

→ ports

Linux:

→ listening services

Red Team:

→ reconnaissance

Blue Team:

→ detect scanning

Network Security:

→ analyze packets

Now we're thinking from **both sides**.

## CLUSTER 6 — WINDOWS ENTERPRISE SECURITY

```text
WINDOWS
    +
NETWORKING
    +
SECURITY
    +
ACTIVE DIRECTORY
    +
RED TEAM
    +
BLUE TEAM
```

This eventually becomes one of the most important clusters for our career.

## CLUSTER 7 — CLOUD

```text
NETWORKING
     +
LINUX
     +
SECURITY
     +
WEB
     +
CLOUD
     +
RED TEAM
     +
BLUE TEAM
```

Cloud should come **after the underlying concepts are comfortable**.

## The dependency tree

This is the structure I want us to keep in mind:

```text
                         CYBERSECURITY
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
       NETWORK              SYSTEMS             WEB
          │               ┌────┴────┐             │
          │               │         │             │
          │             LINUX    WINDOWS          │
          │               │         │             │
          └───────────────┼─────────┼─────────────┘
                          │         │
                          └────┬────┘
                               │
                        SECURITY FUNDAMENTALS
                               │
              ┌────────────────┼────────────────┐
              │                │                │
           RED TEAM         BLUE TEAM        APPSEC
              │                │                │
              └──────────┬─────┴────────────────┘
                         │
                  ACTIVE DIRECTORY
                         │
                    CLOUD SECURITY
```

But there's another important layer:

```text
PYTHON
  │
  ├── Networking automation
  ├── Web automation
  ├── Log analysis
  ├── Security tooling
  └── SOC automation
```

So Python cuts **across the entire tree** rather than sitting underneath it.

## Our actual study philosophy

For every major concept, we should try to answer **five questions**:

#### 1. How does it work?

**Theory**

#### 2. How do I use it?

**Administration**

#### 3. How can it fail?

**Security**

#### 4. How can I attack/test it?

**Red Team**

#### 5. How would I detect/defend it?

**Blue Team**

For example:

### SSH

**Networking**
→ TCP / port 22

**Linux**
→ SSH server

**Security**
→ authentication

**Red Team**
→ SSH exposure / credential security

**Blue Team**
→ SSH logs / suspicious authentication

**Python**
→ automate SSH-related administration

That is exactly the type of **interconnected understanding** we're trying to build.

## Priority of the subjects vs priority of topics

One subtle but important distinction:

Your original list is our **subject priority**:

```text
1. Networking
2. Linux
3. Windows
4. Python
5. Web
6. Security
7. Network Security
8. Red Team
9. Blue Team
10. AD
11. Cloud
12. AppSec
```

But within our actual learning, we'll constantly jump between them.

For example:

> **Networking is Priority #1**

doesn't mean we spend three months studying only networking.

It means:

> **Whenever another subject touches networking, we make sure the networking part is understood deeply.**

That's the model I recommend.

## The first major learning cycle

Before we even think about projects, our first cycle should therefore cover:

#### Networking

**IP → subnetting → ARP → TCP/UDP → ports → DNS → DHCP → routing**

*

#### Linux

**CLI → filesystem → permissions → processes → networking**

*

#### Windows

**users → permissions → processes → networking → logs**

*

#### Python

**fundamentals → files → networking → HTTP**

*

#### Security

**CIA → authentication → authorization → vulnerabilities → risk**

And then those concepts naturally lead into:

**Wireshark + Nmap + web security + basic SOC thinking.**

That's our first **integrated foundation**, rather than five disconnected courses.

### The key principle

From now on, whenever we add a topic to the roadmap, we should ask:

> **"Which other subjects does this topic connect to?"**

That's how we'll turn the 12 subjects into **one cybersecurity curriculum**, rather than 12 separate courses.

And when we move to projects next, each project should deliberately pull from **3–6 subjects at once**. That will make the projects much more valuable for our Red Team + Blue Team goal.
