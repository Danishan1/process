# SHORT-TERM CYBERSECURITY ROADMAP

### 1st Semester → First Cybersecurity Internship

#### Long-Term Priority

**RED TEAM / PENTESTING > BLUE TEAM / SOC > CLOUD SECURITY > APPSEC**

#### Immediate Objective

> **Become capable of applying for and performing meaningful work in a beginner cybersecurity internship by building strong fundamentals, practical labs, documented projects, and a professional portfolio.**

The first internship does not need to be prestigious.

The first internship needs to provide **real security exposure**.

---

## PART I — WHAT WE NEED TO ACHIEVE FIRST

Our first-semester skill stack:

```text
NETWORKING
    ↓
LINUX
    ↓
WINDOWS
    ↓
PYTHON
    ↓
WEB FUNDAMENTALS
    ↓
SECURITY FUNDAMENTALS
    ↓
NETWORK ANALYSIS
    ↓
BASIC VAPT
    ↓
BASIC SOC
    ↓
PROJECTS
    ↓
PORTFOLIO
    ↓
INTERNSHIP APPLICATIONS
```

We are not trying to become an advanced pentester in semester one.

We are trying to become:

> **A technically serious beginner who already understands how security work is performed.**

---

## PART II — THE TOPICS WE MUST COVER

### 1. Networking — Highest Priority

This is the first major foundation.

#### Theory

Learn:

* OSI model
* TCP/IP model
* Ethernet
* MAC addresses
* ARP
* IPv4
* IPv6 basics
* Subnetting
* TCP
* UDP
* Ports
* Three-way handshake
* DNS
* DHCP
* NAT
* ICMP
* Routing
* Switching
* VLANs
* VPN fundamentals
* Firewalls
* Proxies
* HTTP/HTTPS

#### Security connection

For every networking topic, ask:

> How can this be abused?

Examples:

**ARP → ARP spoofing**

**DNS → DNS attacks**

**HTTP → web attacks**

**TCP → scanning / connection analysis**

**DHCP → network attacks**

**VPN → secure remote access**

This prevents networking from becoming disconnected theory.

---

## 2. Linux

We need practical Linux skills.

#### Learn:

```text
Filesystem
Users
Groups
Permissions
Processes
Services
Networking
SSH
Logs
Package management
Bash
Cron
Environment variables
File ownership
System administration
```

#### Commands to become comfortable with:

```bash
ls
cd
pwd
cat
less
grep
find
awk
sed
chmod
chown
ps
top
kill
ss
ip
ping
curl
wget
ssh
scp
tar
systemctl
journalctl
```

Don't memorize commands mechanically.

Use them while solving problems.

---

## 3. Windows

Windows is essential for both Red Team and Blue Team.

Learn:

* Users
* Groups
* Permissions
* Processes
* Services
* Windows Firewall
* Event Viewer
* PowerShell
* Registry basics
* Windows networking
* Authentication
* NTFS
* Security logs

Then begin:

### Active Directory fundamentals

Understand:

* Domain
* Domain Controller
* Users
* Groups
* Organizational Units
* Group Policy
* Kerberos
* LDAP
* NTLM
* Shares
* Domain authentication

We don't need advanced AD exploitation yet.

We first need to understand how AD actually works.

---

## 4. Python

Our goal isn't software engineering.

Our goal is:

> **Security automation.**

Learn:

```text
Variables
Conditions
Loops
Functions
Lists
Dictionaries
Files
Exceptions
Modules
Requests
JSON
Regular expressions
Sockets
Command-line arguments
```

Then start writing small security utilities.

---

## 5. Web Fundamentals

This is extremely important for our Red Team + AppSec direction.

Understand:

```text
Browser
   ↓
HTTP Request
   ↓
Web Server
   ↓
Application
   ↓
Database
```

Learn:

* HTTP methods
* Headers
* Status codes
* Cookies
* Sessions
* Authentication
* Authorization
* Parameters
* Forms
* REST APIs
* JSON
* Basic JavaScript
* SQL basics

Then understand common vulnerabilities.

---

## 6. Security Fundamentals

Learn:

* CIA triad
* Authentication
* Authorization
* Accounting
* Least privilege
* Defense in depth
* Vulnerability
* Threat
* Risk
* Exploit
* Attack surface
* Security controls
* Encryption
* Hashing
* Digital signatures
* Certificates
* PKI
* MFA
* Zero Trust fundamentals

This provides the vocabulary needed to understand professional security discussions.

---

## PART III — TOOLS WE SHOULD LEARN

We don't need 50 tools.

We need to become good with a small number first.

### Networking

#### Wireshark

Goal:

> Capture traffic and understand what actually happened.

---

### Reconnaissance

#### Nmap

Learn:

* Host discovery
* Port scanning
* Service detection
* Version detection
* Basic scripting

Only against systems we own or are explicitly authorized to test.

---

### Web Security

#### Burp Suite

Learn:

* Proxy
* HTTP history
* Repeater
* Intruder basics
* Request modification
* Response analysis

This will eventually become one of our most important Red Team tools.

---

### Linux

Use the normal Linux command line extensively.

Eventually become comfortable with:

```text
tcpdump
curl
ss
netcat
grep
awk
sed
```

---

### Blue Team

Start with one SIEM rather than trying to learn everything.

Possible choices:

* Microsoft Sentinel
* Splunk
* Elastic

The specific platform can change later.

The important concept is:

> **Logs → collection → normalization → search → detection → alert → investigation**

---

## PART IV — OUR PROJECT PORTFOLIO

This is where we become different from students who only watch courses.

We will build projects progressively.

---

## PROJECT 1 — HOME NETWORK SECURITY LAB

#### Difficulty

⭐

#### Purpose

Understand networking practically.

#### Build

Create a small virtual network using virtual machines.

Example:

```text
                HOST MACHINE
                     │
              Virtual Network
                     │
        ┌────────────┼────────────┐
        │            │            │
      Linux        Windows      Security
      Server        VM            VM
```

#### Learn

* IP addressing
* Subnetting
* Ports
* Routing
* DNS
* Services
* Firewall rules

#### Demonstrate

Use Wireshark to capture:

* DNS request
* TCP handshake
* HTTP/HTTPS traffic
* ICMP traffic

#### Deliverable

Create a report:

**"Design and Security Analysis of a Virtual Network Lab"**

Include:

* Network diagram
* IP table
* Services
* Packet captures
* Security observations
* Hardening recommendations

---

## PROJECT 2 — PYTHON NETWORK RECON TOOL

#### Difficulty

⭐⭐

#### Purpose

Combine:

**Python + networking + security**

Build a small authorized network reconnaissance tool.

Features could include:

```text
Target
 ↓
Host availability
 ↓
Port scan
 ↓
Service identification
 ↓
Result report
```

Later add:

* Command-line arguments
* Output to JSON
* Logging
* Timeout handling
* Basic banner collection

#### Important

Build it yourself first.

Don't simply copy an existing scanner.

#### Deliverable

GitHub repository containing:

```text
README
Source code
Usage instructions
Screenshots
Sample output
Technical explanation
Limitations
```

---

## PROJECT 3 — WEB SECURITY LAB

#### Difficulty

⭐⭐⭐

This becomes one of our most important projects.

Build or deploy a deliberately vulnerable web application in our own lab.

Architecture:

```text
Browser
   ↓
Web Application
   ↓
Backend
   ↓
Database
```

Introduce controlled vulnerabilities such as:

* SQL Injection
* XSS
* Broken authentication
* Broken authorization
* Path traversal
* Insecure file upload

#### For every vulnerability:

We document:

```text
1. What is the vulnerability?
2. Why does it exist?
3. How can it be reproduced?
4. What is the impact?
5. How can it be fixed?
6. How could defenders detect it?
```

This one project gives us exposure to:

**Red Team + AppSec + Blue Team**

---

## PROJECT 4 — SOC / LOG ANALYSIS LAB

#### Difficulty

⭐⭐⭐

This is our first major Blue Team project.

Build:

```text
Windows/Linux machines
        ↓
Generate activity
        ↓
Collect logs
        ↓
SIEM
        ↓
Search
        ↓
Detection
        ↓
Alert
        ↓
Investigation
```

Generate legitimate lab activity such as:

* Failed logins
* Successful logins
* New user creation
* Suspicious process execution
* Network connections
* File activity

Then investigate those events.

#### Deliverable

Create several incident reports.

Example:

**Incident #001 — Suspicious Authentication Activity**

Include:

```text
Timeline
Source IP
Affected machine
User
Observed events
Evidence
Analysis
Severity
Containment recommendation
Prevention recommendation
```

This starts teaching us how SOC analysts actually think.

---

## PROJECT 5 — WINDOWS + ACTIVE DIRECTORY LAB

#### Difficulty

⭐⭐⭐⭐

This will become a major Red Team/Blue Team project.

Build a small Active Directory environment.

Example:

```text
             Domain Controller
                    │
          ┌─────────┴─────────┐
          │                   │
      Windows Client      Windows Client
          │
       User Accounts
          │
       Groups / GPO
```

Learn:

* Domain authentication
* Users
* Groups
* Permissions
* Group Policy
* Kerberos
* LDAP
* Windows event logs
* Network shares

Then study common attack and defense concepts **inside the lab**.

Eventually:

```text
Attacker
   ↓
Recon
   ↓
Initial Access
   ↓
Privilege Escalation
   ↓
Lateral Movement
   ↓
Detection
   ↓
Investigation
```

This project should evolve throughout Years 1–3.

---

## PROJECT 6 — MINI CLOUD SECURITY LAB

This comes later.

Don't start here.

Once networking and Linux are comfortable:

Deploy a small environment in AWS/Azure.

Learn:

* IAM
* Roles
* Permissions
* Network configuration
* Security groups
* Logging
* Storage security

Then intentionally create safe misconfigurations in your own environment.

Ask:

> What could go wrong?

Then:

> How would I detect it?

Then:

> How would I fix it?

This connects Cloud + Red Team + Blue Team.

---

## PART V — PROJECT DEVELOPMENT RULE

We will never consider a project finished just because:

> "It works."

Every serious project should have:

```text
Problem
 ↓
Architecture
 ↓
Implementation
 ↓
Testing
 ↓
Security testing
 ↓
Findings
 ↓
Fixes
 ↓
Documentation
```

Every project should ideally have:

#### 1. README

What is it?

#### 2. Architecture diagram

How does it work?

#### 3. Technical explanation

Why did we build it this way?

#### 4. Screenshots

Evidence that we actually did it.

#### 5. Security analysis

What could go wrong?

#### 6. Lessons learned

What did we discover?

#### 7. Future improvements

What would we do next?

This makes the project useful during interviews.

---

## PART VI — FIRST-SEMESTER PROJECT ORDER

Don't attempt six projects simultaneously.

Do them in this order:

#### Month 1

**Project 1: Network Security Lab**

↓

#### Month 2

**Project 2: Python Recon Tool**

↓

#### Month 3

**Project 3: Web Security Lab**

↓

#### Month 4

**Project 4: SOC / Log Analysis Lab**

↓

#### Month 5–6

**Project 5: Active Directory Lab**

The cloud lab can begin once the foundation is strong enough.

---

## PART VII — OUR LEARNING PLATFORMS

Use platforms as laboratories, not as substitutes for understanding.

We can use environments such as:

* TryHackMe
* Hack The Box
* PortSwigger Web Security Academy
* OverTheWire
* picoCTF

The important distinction:

> **Platform completion is not the goal. Skill acquisition is.**

For example, completing 100 rooms without understanding them is less useful than completing 20 rooms and documenting what we actually learned.

---

## PART VIII — WEEKLY STRUCTURE

During the semester, a realistic structure is:

#### 40% — Fundamentals

Networking / Linux / Windows / programming.

#### 25% — Hands-on labs

TryHackMe, HTB, local lab, etc.

#### 25% — Projects

Building our own environments and tools.

#### 10% — Documentation / career

GitHub, notes, resume, internship applications.

---

## PART IX — DAILY LEARNING METHOD

Instead of:

> Watch 2 hours of cybersecurity videos.

Use:

#### Learn

30–60 minutes.

↓

#### Reproduce

Do the same thing yourself.

↓

#### Experiment

Change something.

↓

#### Break it

See what happens.

↓

#### Investigate

Understand why.

↓

#### Document

Write it down.

This is our preferred learning loop.

---

## PART X — GITHUB

GitHub becomes our public technical notebook and portfolio.

We should eventually have repositories such as:

```text
cybersecurity-home-lab
python-network-recon
web-security-lab
soc-log-analysis
active-directory-security-lab
cloud-security-lab
```

But we should **not upload sensitive information**, credentials, private data, or anything obtained from an employer.

For professional work, we document only what we are permitted to disclose.

---

## PART XI — INTERNSHIP PREPARATION

We don't wait until we feel "ready."

We start preparing early.

#### Target roles

Priority:

1. VAPT / Pentesting Intern
2. Cybersecurity Intern — VAPT + SOC
3. SOC Intern
4. Security Analyst Intern
5. Network Security Intern
6. Security Research Intern
7. AppSec Intern
8. Cloud Security Intern

Our first internship does not need to match our final specialization perfectly.

It needs to give us **real security exposure**.

---

## PART XII — WHEN WE START APPLYING

We begin serious applications once we can demonstrate:

#### Knowledge

Networking + Linux + security fundamentals.

#### Practical ability

At least:

* 2–3 completed projects
* Several hands-on labs
* Basic Nmap
* Basic Wireshark
* Basic Burp
* Basic Linux
* Basic Python

#### Portfolio

GitHub + resume + project documentation.

We don't wait for perfection.

---

## PART XIII — INTERNSHIP PROGRESSION

Our internship journey should look like:

```text
1st Internship
     ↓
Basic cybersecurity exposure
     ↓
VAPT / SOC
     ↓
More technical internship
     ↓
Enterprise security exposure
     ↓
Advanced security internship
     ↓
Top-tier security team
```

But the progression is based on:

**technical difficulty > responsibility > mentorship > company reputation**

Company size matters, but it is not the only metric.

---

## PART XIV — FIRST-SEMESTER CERTIFICATION

We don't need to rush multiple certifications.

Our primary certification target can be:

### CCNA

But only after we have actually learned networking.

After that:

### Security+

This becomes more relevant once our networking, Linux, Windows and security foundations are established.

Later:

### Offensive Security

Potential path:

**eJPT → PNPT / OSCP**

And for Blue Team:

**CySA+ → deeper SOC/detection specialization**

The exact timing will depend on our actual progress.

---

## PART XV — OUR FIRST-SEMESTER SUCCESS CRITERIA

At the end of this phase, we should be able to answer:

#### Networking

> What happens when I type a URL into a browser?

#### Linux

> Can I operate and troubleshoot a Linux machine from the command line?

#### Windows

> Can I understand basic Windows processes, services, permissions and logs?

#### Python

> Can I write a small security automation script myself?

#### Web

> Can I explain how a browser, server, backend and database communicate?

#### Red Team

> Can I perform basic reconnaissance and vulnerability analysis in an authorized lab?

#### Blue Team

> Can I read logs and investigate suspicious activity?

#### Security

> Can I explain why a vulnerability exists rather than merely naming it?

#### Professional

> Can I explain one of my projects to an interviewer from architecture → implementation → security → findings → fixes?

If the answer is yes, we are moving in the right direction.

---

## PART XVI — OUR FIRST MAJOR MILESTONE

### 🎯 MILESTONE 1

By the end of the first major learning cycle, we want:

```text
✓ Strong networking fundamentals
✓ Comfortable Linux usage
✓ Basic Windows administration
✓ Basic Python
✓ Web fundamentals
✓ Security fundamentals
✓ Wireshark
✓ Nmap
✓ Burp Suite
✓ Basic SOC concepts
✓ 3+ documented projects
✓ Active GitHub portfolio
✓ Hands-on lab experience
✓ Resume
✓ Internship applications
```

And most importantly:

> **We should be able to demonstrate our knowledge instead of merely claiming it.**

---

## THE SHORT-TERM NORTH STAR

Our immediate mission is NOT:

> "Become a pentester."

It is:

> **Become technically competent enough that a cybersecurity team can take a chance on us as a first-year intern.**

Then:

> **Use that first opportunity to learn how professional security actually works.**

Then:

> **Use that experience to qualify for a better opportunity.**

Then repeat.

```text
LEARN
  ↓
BUILD
  ↓
BREAK
  ↓
INVESTIGATE
  ↓
DOCUMENT
  ↓
APPLY
  ↓
INTERNSHIP
  ↓
LEARN FROM PROFESSIONALS
  ↓
UPGRADE SKILLS
  ↓
BETTER INTERNSHIP
  ↓
DEEPER SPECIALIZATION
```

That is our short-term engine.

---

## OUR CURRENT PRIORITY ORDER

#### RIGHT NOW

**Networking → Linux → Windows → Python → Web → Security**

#### THEN

**Nmap → Wireshark → Burp → VAPT → SOC**

#### THEN

**Projects → GitHub → Resume → Internship**

#### THEN

**Active Directory → Advanced Pentesting → Detection Engineering → Cloud**

#### LONG TERM

**Red Team / Pentesting mastery**

with strong:

**Blue Team + Cloud + AppSec**

supporting knowledge.

---

## FINAL RULE

We will never ask:

> **"What should I learn next because everyone else is learning it?"**

We will ask:

> **"What skill would make me more capable than I was yesterday, and what can I build to prove I learned it?"**

That is how we turn four years of B.Tech into four years of **compounding cybersecurity experience**.

#### One thing I'd do differently from most students

**Don't wait until you've finished this roadmap to start looking for internships.**

Once you have the first **2–3 projects + networking/Linux fundamentals**, we should start the application process and keep learning alongside it. If a company says, *"You don't know enough yet,"* that's useful feedback—we close that gap and apply again.

And as we progress, I would keep one living document for this journey: **a semester-by-semester roadmap with exact topics, projects, labs, certifications, internship targets, and exit criteria**. That way, we're never asking *"what should I learn now?"*—we simply execute the next milestone.
