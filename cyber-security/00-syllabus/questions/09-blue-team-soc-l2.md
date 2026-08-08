Absolutely. **Blue Team / SOC — Level 2** is where we move from **“I understand what a SOC does”** to **“I can investigate suspicious activity and build meaningful detections.”**

Level 1 established:

> **Telemetry → Alert → Triage → Investigation → Response**

Level 2 adds:

> **Correlation → Detection Engineering → Threat Hunting → Attack-Path Reconstruction → Endpoint/Network Investigation → Forensics → Improved Detection**

Because **Red Team is our primary career direction**, this level deliberately asks many questions from both sides:

> **“What would an attacker do?” → “What evidence would they leave?” → “How would I detect it?” → “How would I investigate it?”**

# BLUE TEAM / SOC — LEVEL 2

## Intermediate Question Bank

**150 Questions**

---

# A. SOC Operations & Advanced Triage

### 1.

**How should a SOC analyst prioritize alerts when multiple potentially serious incidents occur simultaneously?**

### 2.

**What factors should influence the severity assigned to a security alert?**

### 3.

**Why should asset criticality influence alert prioritization?**

### 4.

**How should an analyst distinguish between a suspicious event and evidence of an active compromise?**

### 5.

**Why is context often more valuable than the individual alert itself?**

### 6.

**How can an analyst determine whether an alert is related to another alert occurring nearby in time?**

### 7.

**Why should analysts investigate the user, host, network, and application context surrounding an alert?**

### 8.

**How can repetitive alerts indicate either an ongoing attack or a poorly designed detection rule?**

### 9.

**How should an analyst handle an alert involving a privileged account?**

### 10.

**Design an advanced alert-triage methodology that incorporates severity, asset criticality, user identity, attack stage, and available evidence.**

---

# B. SIEM Architecture & Operations

### 11.

**How does a SIEM transform raw logs from multiple systems into useful security information?**

### 12.

**Why are log parsing and normalization important when data comes from different vendors?**

### 13.

**What problems can occur when timestamps from different systems are inconsistent?**

### 14.

**How can missing or malformed log fields affect detection quality?**

### 15.

**Why should a SOC monitor the health of its own logging infrastructure?**

### 16.

**How can log ingestion failures create security blind spots?**

### 17.

**What is the difference between centralized logging and centralized security monitoring?**

### 18.

**How should an organization determine which logs deserve the highest priority for collection?**

### 19.

**Why should log retention policies consider both security requirements and operational constraints?**

### 20.

**Design a SIEM architecture capable of collecting and correlating endpoint, identity, network, application, and cloud telemetry.**

---

# C. SIEM Correlation & Detection

### 21.

**What is event correlation, and why is it important for detecting multi-stage attacks?**

### 22.

**How can multiple low-severity events combine into a high-confidence detection?**

### 23.

**Why can a single event rarely provide enough context to confirm a sophisticated attack?**

### 24.

**How can time windows be used in security-event correlation?**

### 25.

**Why should correlation rules account for normal organizational behavior?**

### 26.

**How can authentication, endpoint, and network events be correlated to detect suspicious activity?**

### 27.

**What makes a correlation rule reliable?**

### 28.

**Why can overly broad correlation rules generate excessive false positives?**

### 29.

**How should detection rules be tested before being deployed to production?**

### 30.

**Design a SIEM correlation strategy for detecting a hypothetical attack involving suspicious authentication, process execution, privilege escalation, and outbound network communication.**

---

# D. Detection Engineering

### 31.

**What is detection engineering?**

### 32.

**How is detection engineering different from simply creating alerts?**

### 33.

**What makes a detection actionable for a SOC analyst?**

### 34.

**Why should detections be based on attacker behavior rather than only known indicators?**

### 35.

**What is detection logic?**

### 36.

**How can detection rules be mapped to MITRE ATT&CK techniques?**

### 37.

**Why should every important detection have an investigation procedure associated with it?**

### 38.

**What is detection coverage?**

### 39.

**How can a SOC identify gaps in its detection coverage?**

### 40.

**Design a detection-engineering lifecycle from threat hypothesis through rule creation, testing, deployment, tuning, measurement, and retirement.**

---

# E. Windows Detection Engineering

### 41.

**Which categories of Windows events are particularly valuable for security detection?**

### 42.

**Why is process creation important for detecting suspicious behavior?**

### 43.

**How can parent-child process relationships reveal potentially malicious execution?**

### 44.

**Why can unusual command-line arguments provide valuable detection context?**

### 45.

**How can Windows authentication events help detect credential abuse?**

### 46.

**How can privilege-assignment events contribute to attack detection?**

### 47.

**Why are service creation and modification events security-relevant?**

### 48.

**How can scheduled-task activity be monitored for suspicious behavior?**

### 49.

**How can PowerShell activity be monitored from a defensive perspective?**

### 50.

**Design a Windows detection strategy for identifying suspicious process execution, authentication, privilege changes, service activity, and scheduled tasks.**

---

# F. Linux Detection Engineering

### 51.

**What Linux telemetry is most useful for detecting unauthorized access?**

### 52.

**How can authentication logs reveal password attacks or credential abuse?**

### 53.

**Why are privileged command executions important to monitor?**

### 54.

**How can unusual process execution indicate compromise?**

### 55.

**How can changes to privileged users or groups indicate malicious activity?**

### 56.

**Why should changes to critical configuration files be monitored?**

### 57.

**How can unexpected outbound connections from servers indicate compromise?**

### 58.

**How can scheduled-task activity be monitored on Linux systems?**

### 59.

**Why can shell history be useful but insufficient during an investigation?**

### 60.

**Design a Linux detection strategy covering authentication, privilege escalation, process execution, persistence, configuration changes, and network activity.**

---

# G. Network Detection & Analysis

### 61.

**What is network detection, and how does it complement endpoint monitoring?**

### 62.

**What characteristics of network traffic can indicate suspicious activity?**

### 63.

**How can unusual connection frequency indicate malicious behavior?**

### 64.

**How can DNS analysis help identify suspicious activity?**

### 65.

**What is beaconing, and why can periodic network communication be security-relevant?**

### 66.

**How can network flows help identify lateral movement?**

### 67.

**Why is encrypted traffic challenging for network defenders?**

### 68.

**How can defenders detect suspicious encrypted communication without necessarily inspecting its contents?**

### 69.

**How can network segmentation improve detection and investigation?**

### 70.

**Design a network-detection strategy capable of identifying reconnaissance, suspicious outbound traffic, lateral movement, and possible command-and-control behavior.**

---

# H. EDR & Endpoint Investigation

### 71.

**What is Endpoint Detection and Response (EDR), and how does it differ from traditional antivirus?**

### 72.

**What types of telemetry does an EDR platform typically provide?**

### 73.

**Why are process trees useful during endpoint investigations?**

### 74.

**How can command-line telemetry help an analyst understand attacker behavior?**

### 75.

**Why is parent-child process analysis important?**

### 76.

**How can file creation and modification events help reconstruct activity?**

### 77.

**How can network connections associated with processes improve endpoint investigations?**

### 78.

**Why should analysts investigate the user associated with a suspicious process?**

### 79.

**How can EDR evidence help distinguish legitimate administrative activity from malicious behavior?**

### 80.

**Design an endpoint-investigation workflow starting with a suspicious EDR alert and ending with a confirmed or dismissed security finding.**

---

# I. Threat Hunting Fundamentals

### 81.

**What is threat hunting, and how does it differ from alert-driven investigation?**

### 82.

**Why does threat hunting begin with a hypothesis rather than a random search through logs?**

### 83.

**What makes a good threat-hunting hypothesis?**

### 84.

**How can MITRE ATT&CK techniques be used to develop threat-hunting hypotheses?**

### 85.

**How can baseline behavior help threat hunters identify anomalies?**

### 86.

**Why should threat hunters search for behavior rather than only known malicious indicators?**

### 87.

**How can threat hunting identify attacks that existing detection rules missed?**

### 88.

**What data sources should a hunter consider when investigating suspicious endpoint behavior?**

### 89.

**How should a hunter document and validate a hunting hypothesis?**

### 90.

**Design a complete threat-hunting workflow from hypothesis creation through data collection, investigation, validation, and detection improvement.**

---

# J. MITRE ATT&CK & Adversary Behavior

### 91.

**What problem does MITRE ATT&CK solve for defenders?**

### 92.

**What is the difference between a tactic and a technique in ATT&CK?**

### 93.

**How can ATT&CK help analysts understand an attack sequence?**

### 94.

**Why should defenders map detections to attacker techniques?**

### 95.

**How can ATT&CK reveal gaps in a SOC's detection coverage?**

### 96.

**How can Red Team activity be mapped to ATT&CK techniques during an authorized exercise?**

### 97.

**Why is technique-level detection often more resilient than malware-specific detection?**

### 98.

**How can ATT&CK be used to organize threat-hunting activities?**

### 99.

**How can ATT&CK improve communication between Red Team and Blue Team?**

### 100.

**Design a detection-coverage matrix mapping an enterprise attack lifecycle to telemetry sources, detections, and investigation procedures.**

---

# K. Authentication Attack Detection

### 101.

**How can a SOC identify password-guessing behavior from authentication logs?**

### 102.

**Why can a successful login following numerous failed attempts be particularly important?**

### 103.

**How can password-spraying behavior differ from traditional brute-force behavior from a defender's perspective?**

### 104.

**How can abnormal authentication location or device information help identify credential compromise?**

### 105.

**Why are privileged-account authentication anomalies especially significant?**

### 106.

**How can authentication events be correlated across multiple systems to identify an attack?**

### 107.

**How can MFA-related events contribute to an investigation?**

### 108.

**Why should defenders monitor authentication failures and successes together rather than independently?**

### 109.

**How can stolen credentials be detected when the login itself appears technically valid?**

### 110.

**Design an identity-threat-detection strategy for identifying credential attacks, account compromise, abnormal authentication, and privilege abuse.**

---

# L. Lateral Movement Detection

### 111.

**What network and endpoint behaviors can indicate lateral movement?**

### 112.

**Why is unusual administrative activity between workstations suspicious?**

### 113.

**How can authentication logs reveal movement between systems?**

### 114.

**Why is source-and-destination analysis important when investigating internal traffic?**

### 115.

**How can privilege changes accompany lateral movement?**

### 116.

**How can unusual remote-service usage indicate suspicious activity?**

### 117.

**Why should defenders understand normal administrative behavior before creating lateral-movement detections?**

### 118.

**How can network segmentation make lateral movement easier to detect?**

### 119.

**What evidence should an analyst collect when investigating suspected lateral movement?**

### 120.

**Design a detection strategy for identifying lateral movement across an enterprise containing workstations, servers, and Active Directory infrastructure.**

---

# M. Incident Investigation & Attack Reconstruction

### 121.

**What is attack reconstruction?**

### 122.

**Why should an analyst construct a timeline during a complex incident?**

### 123.

**How can multiple telemetry sources be combined to reconstruct an attack?**

### 124.

**Why is determining the initial point of compromise important?**

### 125.

**How can an analyst identify the earliest suspicious activity in a large dataset?**

### 126.

**Why should analysts distinguish between attacker actions and normal administrative activity?**

### 127.

**How can process, authentication, and network evidence reinforce each other during an investigation?**

### 128.

**Why is identifying the attacker's objective important when reconstructing an incident?**

### 129.

**How can attack reconstruction reveal previously unknown detection gaps?**

### 130.

**Given endpoint, authentication, and network evidence from a hypothetical compromise, design a methodology for reconstructing the complete attack timeline.**

---

# N. Digital Forensics Fundamentals

### 131.

**What is digital forensics, and how does it support incident response?**

### 132.

**What is the difference between live response and offline forensic analysis?**

### 133.

**Why can volatile information be important during an investigation?**

### 134.

**What types of evidence may disappear when a compromised system is shut down?**

### 135.

**Why is evidence integrity important?**

### 136.

**What is chain of custody, and why can it matter during forensic investigations?**

### 137.

**How can filesystem artifacts help reconstruct attacker activity?**

### 138.

**How can memory analysis provide information that filesystem analysis may miss?**

### 139.

**Why should forensic collection minimize changes to the original evidence?**

### 140.

**Design a basic forensic-investigation workflow for a compromised workstation, including evidence preservation, collection, analysis, timeline reconstruction, and documentation.**

---

# O. Incident Response, Containment & SOC Improvement

### 141.

**How should a SOC determine whether an incident requires immediate containment?**

### 142.

**What factors should influence the choice between isolating an endpoint and allowing it to remain connected?**

### 143.

**Why can containment actions themselves create operational risk?**

### 144.

**What is eradication, and how does it differ from containment?**

### 145.

**Why should recovery include verification that the attacker no longer has access?**

### 146.

**Why is root-cause analysis important after an incident?**

### 147.

**How can an incident reveal weaknesses in existing security detections?**

### 148.

**How should new detections be developed from lessons learned during an incident?**

### 149.

**How can SOC performance be measured using metrics such as detection coverage, false-positive rate, investigation time, and response time?**

### 150.

**Design a complete Level 2 SOC operating model that connects SIEM monitoring, detection engineering, threat hunting, endpoint investigation, network analysis, incident response, forensics, and continuous improvement.**

---

# The Level 2 Mental Model

Level 1 was:

```text
Telemetry
   ↓
Alert
   ↓
Triage
   ↓
Investigation
   ↓
Response
```

Level 2 becomes:

```text
                       THREAT
                         │
                         ▼
                  ATTACK BEHAVIOR
                         │
                         ▼
              ┌────── TELEMETRY ──────┐
              │          │             │
              ▼          ▼             ▼
           Endpoint    Network       Identity
              │          │             │
              └──────────┼─────────────┘
                         ▼
                       SIEM
                         │
                         ▼
                    CORRELATION
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
                         ▼
                    INVESTIGATION
                         │
             ┌───────────┴───────────┐
             ▼                       ▼
         Threat Hunt            Forensics
             │                       │
             └───────────┬───────────┘
                         ▼
                  ATTACK RECONSTRUCTION
                         │
                         ▼
                    CONTAINMENT
                         │
                         ▼
                     RECOVERY
                         │
                         ▼
                DETECTION IMPROVEMENT
```

The important new concept is:

> **The SOC doesn't just react to alerts. It continuously learns how attacks behave and improves its ability to detect them.**

---

# The Level 2 SOC Analyst's Core Questions

For every meaningful investigation, train yourself to ask:

### 1. What happened?

**Event analysis**

### 2. Who performed it?

**Identity**

### 3. From where?

**Source**

### 4. Against what?

**Target**

### 5. What happened immediately before and after?

**Timeline**

### 6. What other systems are involved?

**Correlation**

### 7. What attacker technique does this resemble?

**ATT&CK / behavior**

### 8. How far did the attacker get?

**Scope**

### 9. What evidence proves it?

**Investigation**

### 10. How do we prevent or detect it next time?

**Detection engineering**

---

# The Biggest Level 2 Upgrade

At Level 1, you might see:

```text
Failed Login
```

At Level 2, you think:

```text
Failed Login
     │
     ▼
Which account?
     │
     ▼
Which source?
     │
     ▼
How many attempts?
     │
     ▼
Were there successful logins?
     │
     ▼
From which devices?
     │
     ▼
What happened after authentication?
     │
     ▼
Were new processes created?
     │
     ▼
Were privileged actions performed?
     │
     ▼
Was there unusual network communication?
```

You're no longer analyzing **an alert**.

You're reconstructing **a potential attack**.

---

# Red Team ↔ Blue Team Connection

This is particularly important for our journey.

We learned in Red Team Level 2/3:

```text
Initial Access
      ↓
Privilege Escalation
      ↓
Credential Access
      ↓
Lateral Movement
      ↓
Objective
```

Now Blue Team Level 2 asks:

```text
Can we detect Initial Access?
      ↓
Can we detect Privilege Escalation?
      ↓
Can we detect Credential Abuse?
      ↓
Can we detect Lateral Movement?
      ↓
Can we detect Objective Access?
```

This creates a much more powerful learning loop:

```text
          RED TEAM
             │
       "How would I attack?"
             │
             ▼
          BLUE TEAM
             │
       "What evidence?"
             │
             ▼
       DETECTION ENGINE
             │
       "Can we detect it?"
             │
             ▼
        THREAT HUNT
             │
       "Did we miss it?"
             │
             ▼
       INCIDENT RESPONSE
             │
       "How far did it go?"
             │
             ▼
        IMPROVE DEFENSE
             │
             ▼
          RED TEAM
             │
        "Test again."
```

---

# What Level 2 Should Prepare You For

By the end of this level, you should be able to understand a scenario such as:

```text
Employee Account
       │
       ▼
Suspicious Authentication
       │
       ▼
Endpoint Compromise
       │
       ▼
Suspicious Process
       │
       ▼
Privilege Escalation
       │
       ▼
Credential Abuse
       │
       ▼
Lateral Movement
       │
       ▼
Server Access
       │
       ▼
Sensitive Resource
```

And instead of asking:

> **"Which alert fired?"**

you ask:

> **"Can I reconstruct the entire attack?"**

Then:

> **"Which stage did we detect?"**

> **"Which stage did we miss?"**

> **"What telemetry should have existed?"**

> **"What detection should we build?"**

> **"Could a Red Team reproduce this?"**

That is the bridge between **SOC Analyst → Detection Engineer → Threat Hunter → Incident Responder**.

---

# Level 1 → Level 2

| Level 1                  | Level 2                           |
| ------------------------ | --------------------------------- |
| Understand logs          | Correlate logs                    |
| Understand alerts        | Engineer detections               |
| Basic SIEM               | SIEM correlation                  |
| Basic monitoring         | Detection coverage                |
| Basic investigation      | Attack reconstruction             |
| Basic endpoint security  | EDR investigation                 |
| Basic network monitoring | Network threat detection          |
| Basic incident response  | Advanced investigation            |
| Understand IOCs          | Hunt for attacker behavior        |
| Basic ATT&CK             | ATT&CK-based detection            |
| Respond to alerts        | Proactively hunt                  |
| Investigate incidents    | Improve detections from incidents |

---

# The Core Skill We Are Building

The ultimate Level 2 skill is:

> **Evidence correlation.**

A single piece of evidence rarely tells the whole story.

Instead:

```text
Authentication
      +
Endpoint
      +
Process
      +
Network
      +
Identity
      +
Timeline
      ↓
ATTACK STORY
```

And that's exactly where our **Red Team + Blue Team combination** starts becoming powerful.

A Red Teamer asks:

> **"How can I move through this environment?"**

A Blue Teamer asks:

> **"What evidence would I leave while doing it?"**

A strong security professional asks **both questions simultaneously**.
