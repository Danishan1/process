Absolutely. **Network Security & Analysis — Level 2** is where we move from *understanding network-security concepts* to **actually analyzing network behavior and thinking like both an attacker and a defender**.

Level 1 established:

> **What is communicating with what, through which protocol, across which trust boundary, and should that communication exist?**

Level 2 now asks:

> **What does the traffic actually tell us, how do we recognize reconnaissance and attacks, how do we investigate suspicious communication, and how do we validate network-security controls?**

This level is particularly important for our **Red Team / Pentesting + Blue Team / SOC** priorities.

# NETWORK SECURITY & ANALYSIS — LEVEL 2

## Intermediate Question Bank

**150 Questions**

---

# A. Network Reconnaissance

### 1.

**What is network reconnaissance, and why is it usually one of the first activities performed during an attack?**

### 2.

**What is the difference between passive and active reconnaissance from a network-security perspective?**

### 3.

**What information can an attacker learn by identifying live hosts on a network?**

### 4.

**Why is discovering an organization's network topology valuable to an attacker?**

### 5.

**What is host discovery, and how is it different from service discovery?**

### 6.

**What is port scanning, and what security questions can it answer?**

### 7.

**Why does identifying an open port not necessarily mean that the service is vulnerable?**

### 8.

**How can service enumeration provide more information than simple port scanning?**

### 9.

**What types of network behavior can indicate that reconnaissance is taking place?**

### 10.

**How can defenders distinguish legitimate network administration and asset discovery from potentially malicious reconnaissance?**

---

# B. Scanning & Enumeration

### 11.

**What is the difference between host discovery, port scanning, service enumeration, and vulnerability scanning?**

### 12.

**Why should security teams understand the behavior of scanning tools even if they are not conducting offensive assessments?**

### 13.

**How can scanning a network generate detectable traffic patterns?**

### 14.

**Why might an attacker scan only a small number of ports rather than every available port?**

### 15.

**How can the timing of connection attempts help identify scanning behavior?**

### 16.

**What is the difference between horizontal and vertical scanning?**

### 17.

**How could a defender detect an attacker scanning many hosts for the same service?**

### 18.

**How could a defender detect an attacker scanning one host for many services?**

### 19.

**Why can distributed or slow scanning make detection more difficult?**

### 20.

**Design a detection strategy for identifying network reconnaissance without generating excessive false positives from legitimate vulnerability scanners.**

---

# C. TCP Analysis

### 21.

**How can TCP handshake behavior help an analyst determine whether a connection attempt was successful?**

### 22.

**What does a large number of SYN packets without corresponding completed connections potentially indicate?**

### 23.

**What can repeated RST responses tell an analyst about network behavior?**

### 24.

**Why are TCP flags useful when analyzing packet captures?**

### 25.

**How can unusual TCP connection patterns indicate scanning or malfunctioning applications?**

### 26.

**What is the security significance of connection resets occurring at unusual frequencies?**

### 27.

**How can TCP retransmissions affect security analysis?**

### 28.

**Why should an analyst avoid interpreting every unusual TCP pattern as malicious?**

### 29.

**How can TCP-level evidence be combined with application-layer information to improve investigation accuracy?**

### 30.

**Given a packet capture containing thousands of TCP packets, how would you systematically determine whether the traffic represents normal communication, scanning, application failure, or potential attack activity?**

---

# D. UDP Analysis

### 31.

**Why is UDP analysis different from TCP analysis?**

### 32.

**Why can UDP scanning be more difficult to interpret than TCP scanning?**

### 33.

**What types of services commonly use UDP, and why are they important from a security perspective?**

### 34.

**How can unexpected UDP traffic indicate potential security problems?**

### 35.

**Why should DNS, DHCP, NTP, and other UDP-based protocols receive security monitoring?**

### 36.

**How can high-volume UDP traffic affect network availability?**

### 37.

**What network evidence could indicate abnormal UDP behavior?**

### 38.

**How can defenders distinguish legitimate high-volume UDP traffic from potentially malicious activity?**

### 39.

**Why can UDP-based communication make attribution more difficult in some investigations?**

### 40.

**Analyze a hypothetical network containing unexpected UDP traffic and describe the questions you would ask before deciding whether it represents an attack.**

---

# E. Wireshark & Packet Capture Analysis

### 41.

**What is a packet capture, and what security questions can it answer?**

### 42.

**What is Wireshark conceptually, and why is it useful for security analysis?**

### 43.

**What is the difference between examining individual packets and analyzing an entire conversation or flow?**

### 44.

**Why should an analyst first establish context before examining suspicious packets?**

### 45.

**How can packet-capture timestamps help reconstruct an incident?**

### 46.

**How can source and destination information help identify suspicious communication?**

### 47.

**How can protocol information help narrow down the cause of unusual traffic?**

### 48.

**Why are packet filters useful during large-scale packet analysis?**

### 49.

**How can an analyst follow a network conversation to understand the communication between two hosts?**

### 50.

**Describe a systematic workflow for analyzing an unfamiliar packet capture from initial overview through identification of suspicious traffic.**

---

# F. Protocol Analysis

### 51.

**Why is understanding protocol behavior essential for identifying malicious network activity?**

### 52.

**What can an HTTP request reveal about a client's interaction with a server?**

### 53.

**What information can HTTP headers provide during a security investigation?**

### 54.

**Why can unusual HTTP methods or request patterns be security-relevant?**

### 55.

**What can DNS request and response patterns reveal about endpoint behavior?**

### 56.

**How can TLS metadata provide useful information even when application content is encrypted?**

### 57.

**Why can protocol anomalies be useful indicators of compromise?**

### 58.

**How can malformed or unexpected protocol messages indicate either an attack or a broken application?**

### 59.

**Why is protocol knowledge more valuable than simply memorizing port numbers?**

### 60.

**Choose a common protocol and explain what normal communication looks like, what suspicious behavior might look like, and what telemetry could reveal it.**

---

# G. DNS Security Analysis

### 61.

**What characteristics of DNS traffic should a security analyst normally establish as a baseline?**

### 62.

**Why can unusually long or random-looking domain names deserve investigation?**

### 63.

**How can repeated DNS queries to newly observed domains become a security signal?**

### 64.

**Why can DNS query frequency be useful when investigating potentially compromised endpoints?**

### 65.

**What is DNS tunneling at a conceptual level, and why can it be difficult to detect?**

### 66.

**What characteristics might distinguish ordinary DNS traffic from potentially tunneled communication?**

### 67.

**Why can newly registered or previously unseen domains be useful investigation leads without automatically being malicious?**

### 68.

**How can DNS logs help identify which internal host contacted a suspicious domain?**

### 69.

**How can DNS information be correlated with endpoint and proxy telemetry?**

### 70.

**Design a DNS investigation workflow for a workstation suspected of communicating with malicious external infrastructure.**

---

# H. HTTP/HTTPS Traffic Analysis

### 71.

**What security information can be obtained from HTTP requests and responses?**

### 72.

**Why can unusual HTTP request frequency indicate automated or malicious activity?**

### 73.

**What can user-agent information reveal during network investigations?**

### 74.

**Why should user-agent strings not be treated as trustworthy identities?**

### 75.

**How can HTTP status codes help investigators understand suspicious communication?**

### 76.

**Why can repeated requests to unusual URLs become a useful security indicator?**

### 77.

**What information becomes hidden when communication moves from HTTP to HTTPS?**

### 78.

**What metadata can still be useful when HTTPS content is encrypted?**

### 79.

**Why does TLS inspection create both security benefits and privacy/security considerations?**

### 80.

**Analyze a hypothetical endpoint generating repeated HTTPS connections to an unfamiliar external service. What evidence would you gather before determining whether the behavior is malicious?**

---

# I. Network Baselines & Anomaly Detection

### 81.

**What is a network baseline, and why is it important for security monitoring?**

### 82.

**What characteristics might be included in a baseline for normal network behavior?**

### 83.

**Why can a baseline differ between servers, employee workstations, administrators, and network devices?**

### 84.

**Why is deviation from a baseline not automatically evidence of compromise?**

### 85.

**How can seasonal, business, and operational changes affect network baselines?**

### 86.

**Why should baselines be continuously updated rather than treated as permanent?**

### 87.

**How can behavioral anomalies reveal attacks that signatures fail to identify?**

### 88.

**What is the difference between a statistical anomaly and a security anomaly?**

### 89.

**How can analysts reduce false positives when using behavioral network detection?**

### 90.

**Design a baseline for an employee workstation and identify five network behaviors that would justify further investigation.**

---

# J. Network Flows & Traffic Analysis

### 91.

**What is network-flow data, and how does it differ from a full packet capture?**

### 92.

**Why can flow data be more scalable than storing every packet?**

### 93.

**What security information can source, destination, port, protocol, byte count, packet count, and duration provide?**

### 94.

**How can flow data reveal lateral movement?**

### 95.

**How can flow data reveal unusual outbound communication?**

### 96.

**Why can a host communicating with many previously unseen destinations be worth investigating?**

### 97.

**How can traffic volume help identify potential data-transfer events?**

### 98.

**Why does traffic volume alone not prove data exfiltration?**

### 99.

**How can flow data be correlated with authentication and endpoint telemetry?**

### 100.

**Design an investigation using network-flow data to determine whether a compromised workstation communicated with other internal systems after initial compromise.**

---

# K. IDS & IPS

### 101.

**What is an Intrusion Detection System, and what problem does it solve?**

### 102.

**What is the difference between IDS and IPS?**

### 103.

**What is signature-based network detection?**

### 104.

**What are the limitations of signature-based detection?**

### 105.

**What is anomaly-based detection, and what are its strengths and weaknesses?**

### 106.

**Why can IDS systems generate false positives?**

### 107.

**Why can an attacker attempt to evade network-based detection?**

### 108.

**How should security teams validate whether an IDS alert represents a genuine security event?**

### 109.

**Why should IDS/IPS alerts be correlated with endpoint and identity information?**

### 110.

**Design an IDS/IPS deployment for an enterprise network and explain where sensors should be placed and what security questions each location should answer.**

---

# L. Firewall Analysis

### 111.

**How can firewall logs help reconstruct an attempted attack?**

### 112.

**What information should a useful firewall log contain?**

### 113.

**How can repeated denied connections indicate reconnaissance?**

### 114.

**How can an unexpected allowed connection be more important than thousands of denied connections?**

### 115.

**Why should firewall rules be evaluated based on business requirements and attack paths?**

### 116.

**How can overly broad firewall rules enable lateral movement?**

### 117.

**What is the security significance of outbound firewall rules?**

### 118.

**How can firewall logs help identify compromised hosts communicating externally?**

### 119.

**How would you investigate a sudden increase in denied traffic toward an internal server?**

### 120.

**Design a firewall-log investigation process that moves from raw events to identifying the source, target, service, intent, and potential security impact.**

---

# M. Network Attacks

### 121.

**What makes a network attack different from an ordinary network error or outage?**

### 122.

**How does network reconnaissance support later stages of an attack?**

### 123.

**What is a denial-of-service attack conceptually, and what security property does it primarily target?**

### 124.

**How can excessive traffic affect network devices, servers, and applications differently?**

### 125.

**What is ARP spoofing, and what security assumptions does it abuse?**

### 126.

**What is DNS spoofing conceptually, and what risks can it create?**

### 127.

**What is a man-in-the-middle attack, and what trust relationship does it attempt to violate?**

### 128.

**Why is encrypted communication important for reducing certain man-in-the-middle risks?**

### 129.

**How can network segmentation limit the impact of a compromised internal host?**

### 130.

**Take a hypothetical network attack and explain the complete chain from reconnaissance to exploitation, communication, lateral movement, and potential impact, while identifying defensive opportunities at each stage.**

---

# N. Network Security Investigation

### 131.

**What should an analyst do first after receiving an alert about suspicious network traffic?**

### 132.

**Why should investigators establish the affected host, source, destination, timeframe, and protocol before drawing conclusions?**

### 133.

**How can multiple data sources help validate suspicious network behavior?**

### 134.

**Why is establishing a timeline important during network investigations?**

### 135.

**How can DNS logs, firewall logs, flow data, packet captures, endpoint telemetry, and authentication logs complement each other?**

### 136.

**What is pivoting in a security investigation, and why is it useful?**

### 137.

**How can an analyst pivot from a suspicious IP address to the internal host responsible for the communication?**

### 138.

**How can an analyst pivot from a compromised host to identify other potentially affected systems?**

### 139.

**Why should investigators preserve relevant network evidence before making major changes to the environment?**

### 140.

**Develop a complete investigation methodology for a workstation suspected of communicating with malicious infrastructure.**

---

# O. Red Team ↔ Blue Team Network Thinking

### 141.

**From a Red Team perspective, why is understanding network reachability more important than simply knowing which systems exist?**

### 142.

**How would a penetration tester use network enumeration to identify potential attack paths?**

### 143.

**Why should a penetration tester validate whether discovered services are actually reachable and useful for achieving an objective?**

### 144.

**From a Blue Team perspective, what evidence could network reconnaissance leave behind?**

### 145.

**How can a SOC analyst distinguish vulnerability scanning performed by an internal security team from unauthorized reconnaissance?**

### 146.

**Why should offensive testing and defensive monitoring be evaluated together?**

### 147.

**How can a Red Team exercise reveal weaknesses in network segmentation that a configuration review might miss?**

### 148.

**How can a Blue Team use Red Team activity to improve network detections?**

### 149.

**Design a purple-team exercise in which a controlled network reconnaissance activity is performed, detected, investigated, and then used to improve the organization's security controls.**

### 150.

**Imagine an attacker has compromised one employee workstation. Starting from that single foothold, explain how you would analyze the network to determine what the attacker could potentially reach, what traffic would reveal lateral movement, which controls should prevent it, which telemetry should detect it, and how the organization should respond.**

---

# The Level 2 Mental Model

Level 1 taught us:

```text
WHO?
  ↓
WHAT?
  ↓
WHERE?
  ↓
WHEN?
  ↓
WHY?
```

Level 2 turns this into an investigation:

```text
                    NETWORK EVENT
                         │
                         ▼
                 ┌───────────────┐
                 │   OBSERVE     │
                 └───────┬───────┘
                         │
                         ▼
                  IDENTIFY HOSTS
                         │
                         ▼
                IDENTIFY PROTOCOL
                         │
                         ▼
                 IDENTIFY SERVICE
                         │
                         ▼
                 ESTABLISH BASELINE
                         │
                         ▼
                  FIND ANOMALY
                         │
                         ▼
                 CORRELATE EVIDENCE
                         │
                         ▼
                  FORM HYPOTHESIS
                         │
                         ▼
                  VALIDATE HYPOTHESIS
                         │
                         ▼
                 DETERMINE IMPACT
                         │
                         ▼
                 RESPOND / REMEDIATE
```

That's a major shift.

---

# The Three Perspectives

For every network-security concept we study from this point, I want you to look at it through **three lenses**.

## 1. Network Engineer

> **How should this network work?**

Example:

```text
User → Web Server
Web Server → Database
```

---

## 2. Red Team

> **What shouldn't I be able to reach, and can I find a path to it?**

```text
Compromised User
       │
       ├──→ Web Server
       │
       ├──→ Internal Server
       │
       └──→ Database?
```

---

## 3. Blue Team / SOC

> **If someone tries to make that movement, what evidence will I see?**

```text
Compromised User
       │
       ├──→ Web Server       ← Normal?
       │
       ├──→ Internal Server  ← Suspicious?
       │
       └──→ Database         ← HIGH PRIORITY
```

This three-way perspective is exactly what we need for our long-term goal.

---

# Level 1 → Level 2 Progression

| Level 1                     | Level 2                                        |
| --------------------------- | ---------------------------------------------- |
| What is TCP?                | What does TCP behavior tell an analyst?        |
| What is DNS?                | What does suspicious DNS behavior look like?   |
| What is a firewall?         | How do we investigate firewall events?         |
| What is segmentation?       | How do we validate segmentation?               |
| What is a port?             | How can port behavior reveal reconnaissance?   |
| What is a packet?           | How do we investigate packets?                 |
| What is a network flow?     | How do we use flows during an investigation?   |
| What is IDS?                | How do we validate an IDS alert?               |
| What is VPN?                | How do we investigate suspicious VPN activity? |
| What is network monitoring? | How do we detect abnormal behavior?            |
| What is reconnaissance?     | How do we detect and analyze reconnaissance?   |
| What is an attack?          | How do we reconstruct the attack path?         |

---

# The Skill We Are Building

The ultimate skill at this stage isn't:

> **"I know Wireshark."**

or

> **"I know Nmap."**

Those are tools.

The real skill is:

> **"I can look at network evidence and form a technically defensible explanation of what is happening."**

For example:

```text
10:32:11
Employee-PC
     │
     ├── DNS → unusual-domain.example
     │
     ├── HTTPS → external IP
     │
     ├── Internal connection → Server-01
     │
     ├── Internal connection → Server-02
     │
     └── Administrative service → Server-03
```

A beginner sees:

> "Some connections."

A Level 2 analyst should start asking:

```text
Why did Employee-PC contact this domain?

Why did it contact the external IP?

Why did it suddenly communicate with Server-01?

Has this workstation contacted Server-02 before?

Why is it attempting an administrative connection to Server-03?

Which user was logged in?

What process generated the traffic?

Was this normal behavior?

What happened immediately before this?

What happened afterward?

Is this reconnaissance?

Credential compromise?

Lateral movement?

Malware?

Legitimate administration?

```

**That analytical habit is what we're building.**

---

# Level 2 Knowledge Map

```text
NETWORK SECURITY & ANALYSIS
│
├── Reconnaissance
│   ├── Passive Recon
│   ├── Active Recon
│   ├── Host Discovery
│   ├── Port Scanning
│   └── Enumeration
│
├── Traffic Analysis
│   ├── TCP
│   ├── UDP
│   ├── DNS
│   ├── HTTP/HTTPS
│   ├── TLS Metadata
│   └── Protocol Behavior
│
├── Packet Analysis
│   ├── Packet Captures
│   ├── Filtering
│   ├── Conversations
│   ├── Timelines
│   └── Anomalies
│
├── Network Visibility
│   ├── Flow Data
│   ├── Firewall Logs
│   ├── IDS/IPS
│   ├── DNS Logs
│   └── Network Baselines
│
├── Network Attacks
│   ├── Reconnaissance
│   ├── Scanning
│   ├── DoS
│   ├── ARP Attacks
│   ├── DNS Attacks
│   └── MITM Concepts
│
└── Investigation
    ├── Triage
    ├── Correlation
    ├── Pivoting
    ├── Timeline
    ├── Attack Path
    └── Response
```

---

# The Five Questions I Want You to Internalize

At **Level 1**, the central question was:

> **"Should this communication exist?"**

At **Level 2**, we add:

### 1. What does normal look like?

### 2. What changed?

### 3. What evidence proves the change?

### 4. What could explain the change?

### 5. What security consequence could follow?

That progression takes us from **networking → network security → network analysis → SOC investigation → Red Team attack-path analysis**.

And that's exactly where we want this subject to take us before we move into **Network Security & Analysis Level 3**, where the questions will become substantially more adversarial and investigative: **advanced traffic analysis, IDS/IPS evasion concepts, network attack chains, lateral movement analysis, C2 detection, exfiltration analysis, segmentation bypass, advanced packet investigation, and Red/Blue/Purple-team network scenarios.**
