Absolutely. Now we move to the **second pillar of our long-term career goal: Blue Team / SOC**.

Since Red Team is our **primary career direction**, we're not learning Blue Team merely to become a SOC analyst. We're learning it deeply enough to understand:

> **How defenders detect, investigate, contain, eradicate, recover from, and learn from attacks.**

That will make our Red Team thinking much stronger too.

# BLUE TEAM / SOC — LEVEL 1

## Foundational Question Bank

**150 Questions**

---

# A. Blue Team & SOC Fundamentals

### 1.

**What is the primary purpose of a Blue Team within an organization's cybersecurity program?**

### 2.

**What is a Security Operations Center (SOC), and what functions does it perform?**

### 3.

**How does a SOC differ from a traditional IT operations team?**

### 4.

**What are the major responsibilities of a SOC analyst?**

### 5.

**What is the difference between prevention, detection, response, and recovery?**

### 6.

**Why can no organization rely entirely on preventive security controls?**

### 7.

**What is a security event?**

### 8.

**What is a security alert?**

### 9.

**What is an incident?**

### 10.

**What is the difference between an event, alert, incident, and breach?**

---

# B. Security Monitoring

### 11.

**Why is continuous monitoring important for modern organizations?**

### 12.

**What types of systems should an organization monitor from a security perspective?**

### 13.

**What is telemetry, and why is it important to security monitoring?**

### 14.

**What is the difference between raw telemetry and a security alert?**

### 15.

**Why can collecting too much telemetry become a problem?**

### 16.

**Why can collecting too little telemetry make incident investigation difficult?**

### 17.

**What characteristics make security telemetry useful to an analyst?**

### 18.

**Why is accurate time synchronization important during security investigations?**

### 19.

**How can missing logs affect an organization's ability to investigate an incident?**

### 20.

**Design a basic security-monitoring strategy for a small organization containing endpoints, servers, network devices, and cloud services.**

---

# C. Logging Fundamentals

### 21.

**What is a log, and what information does it typically contain?**

### 22.

**Why are logs important during incident investigation?**

### 23.

**What types of events should Windows systems log?**

### 24.

**What types of events should Linux systems log?**

### 25.

**What types of events should network devices generate?**

### 26.

**Why are authentication logs particularly valuable to SOC analysts?**

### 27.

**What information should ideally be present in an authentication event?**

### 28.

**Why are process-creation logs useful during endpoint investigations?**

### 29.

**How can application logs help identify security incidents?**

### 30.

**Design a logging baseline for a workstation, Linux server, Windows server, firewall, and web application.**

---

# D. Network Monitoring

### 31.

**Why is network visibility important to a Blue Team?**

### 32.

**What information can network traffic reveal during an investigation?**

### 33.

**What is the difference between an IP address, port, protocol, and connection?**

### 34.

**Why is unusual outbound communication potentially security-relevant?**

### 35.

**How can abnormal internal network communication indicate lateral movement?**

### 36.

**Why is DNS traffic useful during security investigations?**

### 37.

**How can repeated connections to unusual destinations indicate suspicious activity?**

### 38.

**Why should network analysts understand normal traffic patterns before investigating anomalies?**

### 39.

**How can network segmentation improve both prevention and detection?**

### 40.

**Design a basic network-monitoring strategy for detecting reconnaissance, suspicious outbound communication, and unusual internal movement.**

---

# E. Endpoint Security

### 41.

**What is endpoint security?**

### 42.

**Why are endpoints frequently targeted by attackers?**

### 43.

**What types of endpoints should a security team monitor?**

### 44.

**What is the difference between antivirus and Endpoint Detection and Response (EDR)?**

### 45.

**Why is process behavior important when investigating endpoint activity?**

### 46.

**What is process creation, and why is it useful to defenders?**

### 47.

**Why can unusual parent-child process relationships be security-relevant?**

### 48.

**How can abnormal command-line activity help identify malicious behavior?**

### 49.

**Why should endpoint investigations consider both the process and the user responsible for it?**

### 50.

**Design a basic endpoint-investigation workflow beginning with a suspicious process alert.**

---

# F. Windows Security Monitoring

### 51.

**Why is Windows security logging important in enterprise environments?**

### 52.

**What is Windows Event Viewer, and how is it useful to analysts?**

### 53.

**What are Windows Security logs?**

### 54.

**Why are successful and failed authentication events important?**

### 55.

**What can account-management events reveal?**

### 56.

**Why are new service installations security-relevant?**

### 57.

**Why can unexpected scheduled tasks be suspicious?**

### 58.

**How can abnormal process creation help identify attacks on Windows systems?**

### 59.

**Why are privilege-related events important during investigation?**

### 60.

**Develop a Windows workstation monitoring checklist for a SOC analyst.**

---

# G. Linux Security Monitoring

### 61.

**Why is Linux security monitoring important even when Linux systems are traditionally considered server-focused?**

### 62.

**What types of authentication events should be monitored on Linux systems?**

### 63.

**Why are privileged command executions security-relevant?**

### 64.

**What is the purpose of Linux system logs?**

### 65.

**How can unexpected process execution indicate compromise?**

### 66.

**Why should changes to system configuration be monitored?**

### 67.

**How can unusual network connections from a Linux server indicate malicious activity?**

### 68.

**Why are new users and changes to privileged groups important to monitor?**

### 69.

**How can scheduled-task activity become relevant during Linux investigations?**

### 70.

**Develop a Linux server monitoring checklist for a SOC analyst.**

---

# H. Authentication & Identity Monitoring

### 71.

**Why is identity one of the most important security-monitoring domains?**

### 72.

**What authentication events should a SOC monitor?**

### 73.

**Why can repeated failed logins be security-relevant?**

### 74.

**Why can successful login events also be suspicious?**

### 75.

**What is an unusual login location, and why can it be useful during investigations?**

### 76.

**Why should authentication events be correlated with device information?**

### 77.

**How can abnormal login times contribute to an investigation?**

### 78.

**Why are privileged-account logins especially important?**

### 79.

**How can impossible or highly unusual authentication patterns indicate compromised credentials?**

### 80.

**Design an identity-monitoring strategy capable of detecting suspicious authentication behavior.**

---

# I. Security Alerts & Alert Triage

### 81.

**What is alert triage?**

### 82.

**Why can't every security alert be treated as a confirmed incident?**

### 83.

**What is a false positive?**

### 84.

**What is a false negative?**

### 85.

**Why can excessive false positives reduce SOC effectiveness?**

### 86.

**What information should an analyst gather when triaging an alert?**

### 87.

**Why is understanding the affected asset important during alert triage?**

### 88.

**Why should the user involved in an alert be investigated?**

### 89.

**How should an analyst prioritize multiple simultaneous alerts?**

### 90.

**Design a repeatable SOC alert-triage process from initial alert receipt to escalation or closure.**

---

# J. SIEM Fundamentals

### 91.

**What is a SIEM, and what problem does it solve?**

### 92.

**Why is centralized log collection useful to security teams?**

### 93.

**What is log normalization?**

### 94.

**Why is parsing important when ingesting logs into a SIEM?**

### 95.

**What is correlation in a SIEM?**

### 96.

**How can multiple low-severity events become a high-value security alert when correlated?**

### 97.

**Why does a SIEM require properly configured data sources to be useful?**

### 98.

**What is a detection rule?**

### 99.

**Why should SIEM detections be continuously reviewed and improved?**

### 100.

**Design a basic SIEM architecture for an organization containing endpoints, firewalls, servers, identity systems, and cloud services.**

---

# K. Threat Detection Fundamentals

### 101.

**What is threat detection?**

### 102.

**How does signature-based detection differ from behavior-based detection?**

### 103.

**Why can attackers evade simple signature-based detections?**

### 104.

**What is anomaly detection?**

### 105.

**Why is understanding normal behavior important for anomaly detection?**

### 106.

**What is an Indicator of Compromise (IOC)?**

### 107.

**What is an Indicator of Attack (IOA)?**

### 108.

**How do IOCs and behavioral indicators differ?**

### 109.

**Why should detection engineering focus on attacker behavior rather than only known malware?**

### 110.

**Design a detection strategy for identifying suspicious authentication, process execution, network communication, and privilege changes.**

---

# L. Incident Response Fundamentals

### 111.

**What is incident response?**

### 112.

**What are the major phases of an incident-response lifecycle?**

### 113.

**Why is preparation important before an incident occurs?**

### 114.

**What is incident identification?**

### 115.

**What is incident containment?**

### 116.

**What is the difference between containment and eradication?**

### 117.

**What is recovery?**

### 118.

**Why is post-incident analysis important?**

### 119.

**Why should incident-response procedures be documented and tested before a real incident?**

### 120.

**Design a basic incident-response workflow for a suspected compromised workstation.**

---

# M. Malware & Suspicious Activity Fundamentals

### 121.

**What is malware, and why is behavioral analysis important to defenders?**

### 122.

**What is the difference between a virus, worm, trojan, ransomware, and spyware?**

### 123.

**Why can malware detection based solely on file signatures be insufficient?**

### 124.

**What endpoint behaviors might indicate malware execution?**

### 125.

**Why can unusual process trees indicate malicious activity?**

### 126.

**Why is unexpected persistence behavior security-relevant?**

### 127.

**How can malware establish communication with external infrastructure?**

### 128.

**Why should suspicious files be analyzed in controlled environments?**

### 129.

**How can a SOC investigate suspected malware without executing it directly on production systems?**

### 130.

**Design a basic SOC investigation workflow for a suspected malware alert.**

---

# N. Threat Intelligence & Attack Frameworks

### 131.

**What is threat intelligence?**

### 132.

**How can threat intelligence improve security monitoring?**

### 133.

**What is the difference between tactical, operational, and strategic threat intelligence?**

### 134.

**What is the MITRE ATT&CK framework, and why is it useful to Blue Teams?**

### 135.

**How can attacker techniques be mapped to security detections?**

### 136.

**Why is understanding attacker behavior important when creating SOC detections?**

### 137.

**How can threat intelligence help prioritize security alerts?**

### 138.

**Why should threat intelligence be evaluated for reliability and relevance?**

### 139.

**How can Red Team activity be mapped to ATT&CK techniques to evaluate defensive visibility?**

### 140.

**Design a basic threat-intelligence workflow that converts an external threat report into actionable detection opportunities.**

---

# O. Investigation, Documentation & SOC Improvement

### 141.

**What is a security investigation, and how does it differ from simple alert triage?**

### 142.

**What questions should an analyst ask when investigating a potentially compromised endpoint?**

### 143.

**Why is establishing a timeline important during incident investigation?**

### 144.

**How can analysts correlate identity, endpoint, and network evidence?**

### 145.

**Why should analysts distinguish facts from assumptions during an investigation?**

### 146.

**What makes good incident documentation useful for another analyst?**

### 147.

**Why should incident-response actions be recorded chronologically?**

### 148.

**How can lessons learned from incidents improve future detections?**

### 149.

**How can a SOC measure whether its detection capability is improving over time?**

### 150.

**Design the complete Level 1 SOC workflow—from telemetry collection and alert generation through triage, investigation, escalation, incident response, documentation, and lessons learned.**

---

# The Level 1 Blue Team Mental Model

Our first objective isn't to become experts in individual security products.

We want to understand this:

```text
                 ORGANIZATION
                      │
                      ▼
              SECURITY TELEMETRY
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
     Endpoint       Network       Identity
        │             │             │
        └─────────────┼─────────────┘
                      ▼
                     SIEM
                      │
                      ▼
                 DETECTION
                      │
                      ▼
                    ALERT
                      │
                      ▼
                    TRIAGE
                      │
              ┌───────┴───────┐
              ▼               ▼
           Benign          Suspicious
              │               │
              ▼               ▼
            Close          Investigate
                              │
                              ▼
                           Incident
                              │
                              ▼
                         Containment
                              │
                              ▼
                         Eradication
                              │
                              ▼
                           Recovery
                              │
                              ▼
                       Lessons Learned
```

The key transition is:

> **Telemetry → Evidence → Detection → Investigation → Response**

---

# The SOC Analyst's Five Questions

Whenever an alert appears, we want our brain to automatically ask:

### 1. What happened?

```text
EVENT
```

### 2. Is it actually suspicious?

```text
TRIAGE
```

### 3. What else happened around it?

```text
CORRELATION
```

### 4. What is the attacker trying to accomplish?

```text
ATTACKER BEHAVIOR
```

### 5. What should we do now?

```text
RESPONSE
```

For example:

```text
Failed Login
     ↓
Successful Login
     ↓
Unusual Device
     ↓
New Process
     ↓
Suspicious Network Connection
     ↓
Privilege Change
```

A beginner sees **six separate events**.

A good SOC analyst sees:

> **Potentially connected activity representing one attack sequence.**

---

# Blue Team Level 1 → Red Team Connection

This is particularly important for **our journey**.

Remember our Red Team progression:

```text
Recon
 ↓
Initial Access
 ↓
Privilege Escalation
 ↓
Lateral Movement
 ↓
Objective
```

Now Blue Team asks:

```text
Can we detect Recon?
        ↓
Can we detect Initial Access?
        ↓
Can we detect Privilege Escalation?
        ↓
Can we detect Lateral Movement?
        ↓
Can we detect Objective Access?
```

So we're building this loop:

```text
                 RED TEAM
                    │
                 ATTACK
                    ▼
                BLUE TEAM
                    │
                 DETECT
                    ▼
              INVESTIGATE
                    │
                 RESPOND
                    ▼
             IMPROVE CONTROLS
                    │
                    ▼
                 RED TEAM
                    │
              TEST AGAIN
```

This is exactly the kind of relationship we want between our **primary Red Team path** and our **secondary Blue Team/SOC specialization**.

---

# What Level 1 Should Make You Capable Of

By the end of these questions, you should be able to look at:

```text
User
 ↓
Endpoint
 ↓
Network
 ↓
Server
 ↓
Identity
 ↓
Application
 ↓
Cloud
```

and understand:

**What should normally happen?**

**What evidence would exist if something abnormal happened?**

**Where would that evidence be collected?**

**How would a SOC detect it?**

**How would an analyst investigate it?**

**What action should follow?**

That's the foundation we need before moving into **Blue Team Level 2**, where we'll go substantially deeper into:

* SIEM operations
* Detection engineering
* Windows event analysis
* Linux telemetry
* Network detection
* EDR
* Threat hunting
* Incident investigation
* IOC/IOA analysis
* MITRE ATT&CK
* Authentication attacks from the defender's perspective
* Malware investigation
* SOC workflows
* Alert correlation
* Basic digital forensics
* Detection gaps

And eventually Level 3 will connect this with our Red Team knowledge:

> **"If I were the attacker, what would I do—and exactly what evidence would I leave behind?"**

That is the Blue Team mindset we want to develop.
