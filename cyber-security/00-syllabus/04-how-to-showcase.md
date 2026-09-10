In cybersecurity, **showcasing work is very different from a normal software-development portfolio**. You don't just show “I built X.” You need to show that you can **investigate, find weaknesses, understand evidence, and communicate risk safely**.

For the students we are designing the curriculum for, I would make the showcase system part of the syllabus itself.

## 1. The Cybersecurity Portfolio Stack

A student should ideally have:

```text
                 Cybersecurity Portfolio
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
       GitHub          Writeups       Resume
          │              │              │
      Projects         Labs/CTFs       Skills
      Scripts          Investigations   Experience
      Configs          Reports          Projects
          │              │
          └──────────────┼──────────────┘
                         ↓
                    LinkedIn
                         │
                         ↓
                  Internship / Job
```

The **GitHub repository is the evidence**, while the **resume and LinkedIn are the summary**.

---

# 2. What should students showcase?

I would divide cybersecurity work into **6 categories**.

### A. Security Projects

For example:

* Home Network Security Lab
* Python Network Recon Tool
* Web Security Lab
* SOC Log Analysis Lab
* Windows Security Lab
* Active Directory Security Lab
* Cloud Security Lab

But don't simply write:

> “Built a network security lab.”

Instead:

> **Built and analyzed a segmented home-network security lab, capturing and investigating ARP, DNS, DHCP and TCP traffic using Wireshark and Linux tools. Documented attack indicators, network exposure and defensive controls.**

That immediately sounds more professional.

---

# 3. Security Writeups

This is **extremely important** for beginners.

A student may not have professional cybersecurity experience yet, but they can demonstrate their thinking through writeups.

For every interesting lab/CTF/security exercise:

```text
Problem
   ↓
Environment
   ↓
Initial Observation
   ↓
Investigation
   ↓
Hypothesis
   ↓
Validation
   ↓
Finding
   ↓
Impact
   ↓
Remediation
   ↓
Retest
   ↓
Lessons Learned
```

For example:

### Writeup: Investigating Suspicious DNS Traffic

**Environment**

```text
Ubuntu VM
Wireshark
tcpdump
DNS server
Test client
```

**Observation**

> The client was generating an unusually high number of DNS requests to an unfamiliar domain.

**Investigation**

* inspected DNS packets
* checked query frequency
* identified requesting process
* compared with normal baseline
* examined destination IP
* checked system logs

**Finding**

> Suspicious DNS behavior was identified in the controlled lab environment.

**Impact**

Explain what could happen if the behavior were malicious.

**Remediation**

Explain defensive measures.

**Retest**

Demonstrate that the abnormal behavior was no longer present.

That is much stronger than:

> “I learned Wireshark.”

---

# 4. CTF / TryHackMe / HTB Writeups

Students can also showcase lab platforms such as:

* TryHackMe
* Hack The Box
* PortSwigger Web Security Academy
* OverTheWire
* picoCTF

But there is an important rule:

### Don't make the portfolio just a list of completed rooms.

Bad:

```text
TryHackMe:
✓ Room 1
✓ Room 2
✓ Room 3
✓ Room 4
✓ Room 5
```

Better:

```text
Lab: Web Authentication Investigation

Objective
Methodology
Evidence
Vulnerability
Root Cause
Impact
Remediation
What I learned
```

The goal is to demonstrate **understanding**, not platform completion.

---

# 5. GitHub Structure

I would ask every student to maintain a professional GitHub structure like:

```text
github.com/student
│
├── cybersecurity-home-lab
│
├── python-network-recon
│
├── web-security-lab
│
├── soc-log-analysis
│
├── windows-security-lab
│
├── active-directory-security-lab
│
├── security-scripts
│
└── security-writeups
```

Inside each repository:

```text
project/
│
├── README.md
├── docs/
│   ├── architecture.md
│   ├── investigation.md
│   └── findings.md
│
├── scripts/
│
├── configs/
│
├── screenshots/
│
├── reports/
│
└── LICENSE
```

Obviously, **never commit passwords, API keys, private IP information that shouldn't be public, personal data, employer data, or unauthorized assessment data.**

---

# 6. README should look like a Security Engineer's README

Every major project should answer:

### 1. What problem does this solve?

### 2. What environment was used?

```text
Ubuntu
Windows
Docker
VirtualBox
Wireshark
Nmap
Burp Suite
Elastic/Splunk/etc.
```

### 3. Architecture

Show:

```text
             Internet
                 │
             Firewall
                 │
        ┌────────┴────────┐
        │                 │
      Linux             Windows
        │                 │
      Web App             AD
        │
       DB
```

### 4. What did you investigate?

### 5. What tools were used?

### 6. What did you discover?

### 7. What was the security impact?

### 8. How was it fixed?

### 9. How was the fix validated?

### 10. What did you learn?

This turns the repository into **evidence of engineering ability**.

---

# 7. Screenshots are surprisingly important

Cybersecurity work is highly visual.

Students should capture evidence such as:

### Networking

* Wireshark packet capture
* Nmap results
* network topology
* routing configuration

### Linux

* processes
* services
* permissions
* logs
* SSH investigation

### Web Security

* Burp request/response
* vulnerable behavior in a lab
* corrected behavior
* application architecture

### SOC

* SIEM dashboard
* alert
* log correlation
* investigation timeline

### Active Directory

* domain structure
* users/groups
* GPO
* authentication events
* investigation evidence

But screenshots should be **sanitized**.

---

# 8. Security Reports

This is another major differentiator.

Students should learn to produce a professional report.

For example:

```text
Security Assessment Report
│
├── Executive Summary
├── Scope
├── Methodology
├── Environment
├── Findings
│
├── Finding #1
│   ├── Title
│   ├── Severity
│   ├── Description
│   ├── Evidence
│   ├── Impact
│   ├── Root Cause
│   ├── Recommendation
│   └── Retest
│
├── Finding #2
│
└── Conclusion
```

For a first-year student, even a **small 5–10 page professional report** can be excellent portfolio material.

---

# 9. Demonstrate both Red Team AND Blue Team thinking

This is where your curriculum can become much stronger.

Suppose a student discovers an authentication vulnerability in a controlled lab.

Don't stop at:

> “I exploited the vulnerability.”

Ask them to continue:

```text
Red Team
    ↓
Find vulnerability
    ↓
Demonstrate impact
    ↓
        ↓
Blue Team
        ↓
What logs were generated?
        ↓
Can SOC detect it?
        ↓
What indicator would appear?
        ↓
How should it be investigated?
        ↓
How should it be fixed?
        ↓
Retest
```

This creates a much more mature portfolio.

---

# 10. A particularly powerful format: Attack → Evidence → Defense

I'd actually make this a standard requirement.

For every significant security project:

```text
ATTACK
What could an attacker do?

        ↓

EVIDENCE
What would we observe?

        ↓

INVESTIGATION
How would we confirm it?

        ↓

IMPACT
What could happen?

        ↓

DEFENSE
How do we prevent/detect it?

        ↓

RETEST
Does the defense actually work?
```

This trains students to think like security professionals rather than tool operators.

---

# 11. LinkedIn

LinkedIn should **not** say:

> “Aspiring Cybersecurity Professional | Ethical Hacker | Bug Hunter | Kali Linux Expert”

especially for beginners.

Instead:

> **Cybersecurity Student | Network Security | Linux | Web Security | SOC | Python Automation**

Then posts can showcase actual work.

Example:

> Built a controlled Active Directory security lab to understand Windows authentication, privilege relationships and lateral-movement detection.
>
> I documented the environment, investigated authentication events, analyzed logs and implemented defensive controls.
>
> Key learning: understanding normal authentication behavior is essential before identifying anomalous activity.

That is much stronger.

---

# 12. Portfolio website

Eventually, students can have:

```text
studentname.dev
│
├── About
├── Skills
├── Projects
├── Security Labs
├── Writeups
├── Certifications
├── CTF / Labs
├── Reports
└── Contact
```

The homepage should immediately answer:

> **Who is this person and what security work have they actually done?**

Not:

> “Welcome to my website. I am passionate about cybersecurity.”

---

# 13. Resume

The resume should contain **evidence-based bullets**.

Instead of:

> Learned Nmap and Wireshark.

Use:

> Analyzed TCP, DNS, ARP and HTTP traffic using Wireshark and tcpdump across a virtualized network security lab.

Instead of:

> Made a Python scanning tool.

Use:

> Developed a Python-based network reconnaissance utility for authorized lab environments, supporting host discovery, port enumeration and structured result output.

Instead of:

> Practiced SOC.

Use:

> Investigated simulated authentication, malware and data-exfiltration alerts using centralized logs, correlating timestamps, users, hosts and network indicators to construct incident timelines.

---

# 14. The student's "Proof of Work" hierarchy

I'd teach students this hierarchy:

```text
                 PROFESSIONAL VALUE
                       ↑
                       │
              ┌─────────────────┐
              │ Real Experience  │
              ├─────────────────┤
              │ Security Projects│
              ├─────────────────┤
              │ Detailed Reports │
              ├─────────────────┤
              │ Investigation    │
              │ Writeups          │
              ├─────────────────┤
              │ CTF/Lab Writeups  │
              ├─────────────────┤
              │ Certifications    │
              ├─────────────────┤
              │ Course Completion │
              └─────────────────┘
```

Certification is useful, but **proof of work should dominate the portfolio**.

---

# 15. For your students, I would make this an actual requirement

By the end of Semester 1, each student should have:

### GitHub

**3–5 serious repositories**

```text
✓ Network Security Lab
✓ Python Security Tool
✓ Web Security Lab
✓ SOC Investigation Lab
✓ Windows/AD Lab
```

### Writeups

**10–20 meaningful investigations/labs**

### Security reports

**2–3 professional reports**

### CTF/Lab profile

Evidence of consistent hands-on practice.

### LinkedIn

Professional profile + technical posts.

### Resume

One-page beginner cybersecurity resume.

### Portfolio

Simple website linking everything together.

---

## The most important principle

Don't teach students:

> **"How do I show that I know cybersecurity?"**

Teach them:

> **"How do I prove that I can investigate a security problem?"**

That changes the entire portfolio.

A recruiter should be able to open a student's GitHub and see:

```text
I know networking
        ↓
I can work with Linux/Windows
        ↓
I can use security tools
        ↓
I can investigate something abnormal
        ↓
I can identify the root cause
        ↓
I understand the security impact
        ↓
I can recommend a fix
        ↓
I can validate the fix
        ↓
I can document everything professionally
```

**That is the cybersecurity portfolio we should build into the semester syllabus.**
