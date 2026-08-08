Absolutely. **Network Security & Analysis — Level 1** is where we take everything we learned in Networking + Security Fundamentals and start combining them.

The goal is **not yet advanced packet analysis or offensive exploitation**. Level 1 should make you comfortable looking at a network and asking:

> **What is communicating with what, why is it communicating, what should normally happen, what could go wrong, and what evidence would an attacker leave behind?**

This is particularly important for our path because **Red Team/Pentesting and Blue Team/SOC both depend heavily on understanding networks**.

# NETWORK SECURITY & ANALYSIS — LEVEL 1

## Foundation Question Bank

**150 Questions**

---

# A. Network Security Fundamentals

### 1.

**What is network security, and how is it different from general cybersecurity?**

### 2.

**Why is understanding normal network behavior essential before attempting to identify malicious activity?**

### 3.

**What are the primary security objectives when protecting a network?**

### 4.

**How do confidentiality, integrity, and availability apply specifically to network communications?**

### 5.

**What is a network attack surface?**

### 6.

**What components contribute to an organization's network attack surface?**

### 7.

**Why can an apparently harmless exposed service become a security risk?**

### 8.

**What is the difference between securing a network and securing the systems connected to that network?**

### 9.

**Why is network security best approached as a layered defense rather than a single security product?**

### 10.

**Design a basic security architecture for a small organization and explain where different network-security controls should exist.**

---

# B. Network Architecture & Trust

### 11.

**What is a network security boundary?**

### 12.

**Why should different levels of trust be separated into different network segments?**

### 13.

**What is the security difference between an Internet-facing network and an internal network?**

### 14.

**Why should an internal network not automatically be considered trusted?**

### 15.

**What is a DMZ, and what security problem does it address?**

### 16.

**Why are Internet-facing servers commonly placed in separate network segments?**

### 17.

**What is network segmentation, and how does it reduce security risk?**

### 18.

**What is network isolation, and how is it different from ordinary segmentation?**

### 19.

**How can poor network architecture create unnecessary attack paths between systems?**

### 20.

**Design a network containing users, public servers, internal servers, administrators, and sensitive databases. Explain how you would separate the trust zones.**

---

# C. OSI Model & Security Perspective

### 21.

**Why is the OSI model useful when analyzing network security problems?**

### 22.

**What security concerns exist at the physical layer?**

### 23.

**What types of security mechanisms operate around the data-link layer?**

### 24.

**How can attacks at the network layer affect systems at higher layers?**

### 25.

**What security information can be obtained from transport-layer communication?**

### 26.

**Why is application-layer visibility particularly valuable for security analysis?**

### 27.

**How does thinking in layers help troubleshoot whether a security problem is caused by connectivity, transport, or application behavior?**

### 28.

**Why can a security control at one layer fail to protect against an attack occurring at another layer?**

### 29.

**How can an attacker combine weaknesses across multiple network layers?**

### 30.

**Take a normal HTTPS connection and explain what happens at the major networking layers from the client to the server.**

---

# D. IP Addressing & Network Security

### 31.

**Why is IP addressing important from a security-analysis perspective?**

### 32.

**What is the difference between public and private IP addressing?**

### 33.

**Why does knowing whether an IP address is public or private matter during an investigation?**

### 34.

**What information can an IP address provide to a security analyst?**

### 35.

**What information can an IP address not reliably tell you?**

### 36.

**Why should IP addresses not automatically be treated as identities?**

### 37.

**How can NAT complicate security investigations?**

### 38.

**Why is subnetting useful for network security and segmentation?**

### 39.

**How can network ranges help security analysts understand the architecture of an organization?**

### 40.

**Given an organization's IP addressing structure, how would you identify which networks deserve stronger monitoring or access restrictions?**

---

# E. TCP & UDP Security

### 41.

**What is the security significance of TCP's connection-oriented behavior?**

### 42.

**What information can the TCP three-way handshake provide during network analysis?**

### 43.

**What is the purpose of TCP flags such as SYN, ACK, FIN, and RST?**

### 44.

**Why can unusual TCP flag combinations be useful during security analysis?**

### 45.

**What makes UDP fundamentally different from TCP from a security-monitoring perspective?**

### 46.

**Why can UDP-based services create particular challenges for network defenders?**

### 47.

**Why are open TCP or UDP ports not automatically vulnerabilities?**

### 48.

**How can an analyst determine whether a listening service represents legitimate business functionality or unnecessary exposure?**

### 49.

**How can port behavior provide clues about potentially malicious activity?**

### 50.

**Analyze a hypothetical host exposing several TCP and UDP services. Explain how you would determine which services deserve further security investigation.**

---

# F. Ports & Network Services

### 51.

**What is a network port, and why is it useful for identifying services?**

### 52.

**Why should security analysts understand common service-to-port relationships without assuming that a port always identifies the service?**

### 53.

**Why can attackers intentionally run services on unusual ports?**

### 54.

**How can port scanning help security teams understand their attack surface?**

### 55.

**Why should organizations periodically identify unexpected listening services?**

### 56.

**What is the difference between an exposed service and an exposed vulnerable service?**

### 57.

**Why is service version information security-sensitive?**

### 58.

**How can unnecessary services increase an organization's attack surface?**

### 59.

**How would you investigate an unexpected open port on a production server?**

### 60.

**Design a process for continuously identifying unauthorized or unexpected network services across an organization's infrastructure.**

---

# G. DNS Security

### 61.

**What role does DNS play in normal network communication?**

### 62.

**Why is DNS important from a cybersecurity perspective?**

### 63.

**What information can DNS queries reveal about user and system behavior?**

### 64.

**Why can unusual DNS queries be useful indicators during security investigations?**

### 65.

**What is DNS caching, and how does it affect security analysis?**

### 66.

**Why should organizations monitor DNS infrastructure rather than treating it as purely operational infrastructure?**

### 67.

**How can attackers abuse legitimate DNS functionality?**

### 68.

**Why can DNS be involved in command-and-control communication?**

### 69.

**How can defenders distinguish unusual DNS activity from legitimate applications that generate many DNS queries?**

### 70.

**Design a basic DNS-monitoring strategy for detecting suspicious domain-resolution behavior inside an organization.**

---

# H. DHCP, ARP & Local Network Security

### 71.

**What role does DHCP play in an IP network?**

### 72.

**Why is DHCP security important in enterprise environments?**

### 73.

**What is ARP, and why does it exist?**

### 74.

**Why does ARP operate differently from IP routing?**

### 75.

**What is ARP spoofing conceptually, and why can it be dangerous on a local network?**

### 76.

**How could an attacker use manipulation of local network address resolution to intercept traffic?**

### 77.

**What security mechanisms can help protect switched networks from address-resolution attacks?**

### 78.

**Why are local-network attacks different from attacks originating across the Internet?**

### 79.

**How could a defender detect suspicious changes in local address mappings?**

### 80.

**Describe a hypothetical local-network attack involving address-resolution manipulation and explain how a defender could detect and prevent it.**

---

# I. Routing & Network Security

### 81.

**Why is routing important to network security?**

### 82.

**How can incorrect routing expose systems that should not be reachable?**

### 83.

**What is the security significance of a default gateway?**

### 84.

**How can routing tables help an analyst understand the path between systems?**

### 85.

**Why should sensitive network segments have carefully controlled routing paths?**

### 86.

**How can attackers benefit from excessive network reachability?**

### 87.

**Why is "can communicate with" often a more important security question than simply "exists on the network"?**

### 88.

**How can routing and firewall rules work together to restrict attack paths?**

### 89.

**How would you investigate a situation where a workstation suddenly becomes able to reach a previously inaccessible server?**

### 90.

**Design routing boundaries for an enterprise containing public servers, employee systems, administrative systems, and sensitive databases.**

---

# J. Firewalls

### 91.

**What problem does a firewall solve?**

### 92.

**How does a firewall determine whether network traffic should be allowed or blocked?**

### 93.

**What is the difference between inbound and outbound firewall filtering?**

### 94.

**Why is controlling outbound traffic important even when inbound traffic is strongly restricted?**

### 95.

**What is a stateful firewall, and why is connection state useful?**

### 96.

**Why can an overly permissive firewall rule create a significant security risk?**

### 97.

**Why can an overly restrictive firewall configuration also create operational problems?**

### 98.

**What does a firewall rule such as "allow from network A to service B" conceptually mean?**

### 99.

**Why should firewall rules be reviewed and cleaned up regularly?**

### 100.

**Design a firewall policy for a small enterprise and explain how you would control Internet access, internal communication, administrative access, and access to sensitive systems.**

---

# K. Network Address Translation & Security

### 101.

**What problem does NAT solve from a networking perspective?**

### 102.

**Why does NAT sometimes provide a limited security benefit without being a complete security control?**

### 103.

**Why should NAT not be confused with a firewall?**

### 104.

**How can NAT complicate attribution during incident investigations?**

### 105.

**How can port translation affect the way security analysts interpret network traffic?**

### 106.

**Why is understanding NAT important when investigating outbound connections from internal hosts?**

### 107.

**How can logs from NAT devices help correlate internal systems with external activity?**

### 108.

**What problems occur when NAT logging is incomplete or unavailable?**

### 109.

**How could an analyst determine which internal host generated an external connection when multiple hosts share one public IP?**

### 110.

**Design a logging strategy that allows security analysts to reconstruct internal-to-external connections across a NAT boundary.**

---

# L. VPN & Secure Remote Access

### 111.

**What problem does a VPN attempt to solve?**

### 112.

**What security properties should a properly designed VPN provide?**

### 113.

**Why does encrypting a VPN connection not automatically make the endpoint trustworthy?**

### 114.

**What risks arise when employees connect to corporate resources from untrusted networks?**

### 115.

**Why should remote-access authentication receive strong security controls?**

### 116.

**How can compromised VPN credentials provide attackers with access to internal resources?**

### 117.

**Why should VPN access be restricted according to user identity, device security, and required resources?**

### 118.

**What logs should security teams monitor for suspicious VPN activity?**

### 119.

**How might a security analyst identify a potentially compromised VPN account?**

### 120.

**Design a secure remote-access architecture for employees who need access to internal corporate applications from home and public networks.**

---

# M. Network Segmentation & Access Control

### 121.

**Why is segmentation one of the most important network-security concepts for limiting attacker movement?**

### 122.

**What is the difference between VLAN-based segmentation, subnet segmentation, and security-policy enforcement?**

### 123.

**Why does segmentation become ineffective if all segments can freely communicate?**

### 124.

**How should access between security zones be determined?**

### 125.

**Why should administrative networks receive stronger restrictions than ordinary user networks?**

### 126.

**Why should sensitive database networks generally have limited direct access from user endpoints?**

### 127.

**How can segmentation reduce the blast radius of a compromised endpoint?**

### 128.

**How can attackers exploit poorly designed segmentation?**

### 129.

**How would you test whether a segmentation design actually prevents unauthorized communication?**

### 130.

**Design a segmented enterprise network and explain the permitted communication paths between each security zone.**

---

# N. Network Monitoring & Visibility

### 131.

**Why is network visibility essential to effective security monitoring?**

### 132.

**What is the difference between observing network metadata and inspecting network content?**

### 133.

**What types of network telemetry are useful to security analysts?**

### 134.

**Why are source IP, destination IP, ports, protocols, timestamps, and connection direction useful during investigations?**

### 135.

**What is a network flow record, and what security information can it provide?**

### 136.

**How can network monitoring establish a baseline of normal organizational behavior?**

### 137.

**Why is unusual behavior not automatically malicious behavior?**

### 138.

**How can analysts combine network telemetry with endpoint and identity information?**

### 139.

**Why is centralized network visibility valuable during an incident?**

### 140.

**Design a basic network-monitoring architecture for an enterprise and explain what traffic and metadata you would collect and why.**

---

# O. Packet Analysis & Security Investigation

### 141.

**What is packet analysis, and why is it useful for cybersecurity?**

### 142.

**What information can a packet reveal about communication between two systems?**

### 143.

**Why is understanding normal protocol behavior essential before interpreting suspicious packets?**

### 144.

**How can packet captures help determine whether a connection was successfully established?**

### 145.

**How can packet analysis help distinguish connection attempts from completed communication?**

### 146.

**What can repeated connection attempts to many ports suggest during an investigation?**

### 147.

**How can packet timing, frequency, direction, and volume provide security clues?**

### 148.

**Why can packet captures contain extremely useful forensic evidence during an incident?**

### 149.

**How would you approach a packet capture containing suspicious traffic without immediately assuming that the traffic is malicious?**

### 150.

**Given a hypothetical packet capture from a compromised workstation, describe how you would move from basic connection identification to determining the communicating hosts, protocols, services, unusual behavior, possible attack activity, and evidence that should be investigated further.**

---

# The Level 1 Mental Model

Networking gave us the foundation:

```text
HOST
 │
 ├── IP
 ├── MAC
 ├── PORT
 └── SERVICE
```

Now Network Security & Analysis adds:

```text
HOST
 │
 ▼
COMMUNICATION
 │
 ├── WHO?
 │     ├── Source
 │     └── Destination
 │
 ├── HOW?
 │     ├── Protocol
 │     ├── Port
 │     └── Service
 │
 ├── WHERE?
 │     ├── Internal
 │     ├── External
 │     └── Security Zone
 │
 ├── WHEN?
 │     └── Timestamp / Frequency
 │
 └── WHY?
       └── Expected vs Unexpected
```

And **that "WHY?" is where security analysis begins.**

---

# The Core Network-Security Questions

Whenever you see network traffic, train yourself to ask:

### 1. Who is talking?

```text
Source → Destination
```

### 2. What are they using?

```text
Protocol → Port → Service
```

### 3. Where are they?

```text
Internal → Internal
Internal → External
External → Internal
```

### 4. Is this expected?

```text
Normal behavior?
Expected application?
Expected user?
Expected destination?
Expected time?
```

### 5. If it isn't expected, why?

```text
Misconfiguration?
New application?
Administrator activity?
Compromise?
Reconnaissance?
Malware?
Data transfer?
```

This five-question model will become extremely useful later in **SOC investigations**.

---

# The Red Team ↔ Blue Team Connection

Network Security & Analysis is one of the first subjects where our two primary career directions begin directly overlapping.

| Red Team                     | Blue Team                       |
| ---------------------------- | ------------------------------- |
| Discover exposed services    | Identify exposed services       |
| Map network topology         | Monitor network topology        |
| Identify reachable systems   | Analyze unexpected reachability |
| Scan ports                   | Detect scanning                 |
| Understand protocols         | Detect protocol anomalies       |
| Find segmentation weaknesses | Validate segmentation           |
| Identify attack paths        | Detect lateral movement         |
| Understand VPN exposure      | Monitor VPN activity            |
| Understand DNS               | Detect suspicious DNS           |
| Understand firewall rules    | Investigate firewall events     |
| Understand traffic flows     | Analyze traffic flows           |
| Validate network controls    | Improve network controls        |

So we aren't learning networking twice.

We're learning to **look at the same network from two perspectives**.

---

# The Most Important Transition

At ordinary Networking:

> **"How does a packet get from A to B?"**

At Network Security:

> **"Should A be able to communicate with B?"**

At Network Analysis:

> **"Why did A communicate with B at this particular time using this particular protocol, and what does that tell me?"**

And eventually in SOC:

> **"Does this communication represent part of an attack?"**

And in Red Team:

> **"Can I use this communication path to reach something I'm not supposed to reach?"**

That progression is exactly what we want.

---

# Level 1 Knowledge Map

By the end of this level, the map should look like:

```text
NETWORK SECURITY & ANALYSIS
│
├── Network Security Fundamentals
│
├── Network Architecture
│   ├── Trust Boundaries
│   ├── DMZ
│   ├── Segmentation
│   └── Isolation
│
├── Network Protocols
│   ├── IP
│   ├── TCP
│   ├── UDP
│   ├── DNS
│   ├── DHCP
│   └── ARP
│
├── Network Infrastructure
│   ├── Routing
│   ├── NAT
│   ├── Firewalls
│   ├── VPN
│   └── VLANs
│
├── Network Exposure
│   ├── Ports
│   ├── Services
│   ├── Attack Surface
│   └── Reachability
│
└── Network Visibility
    ├── Logs
    ├── Flow Data
    ├── Packet Captures
    ├── Monitoring
    └── Baselines
```

---

# What Level 1 Should Give You

By the end of these 150 questions, you should be able to look at a basic network diagram and understand:

```text
Internet
    │
    ▼
[Firewall]
    │
    ▼
   [DMZ]
   /   \
 Web    Mail
 Server  Server
    │
    ▼
[Internal Firewall]
    │
 ┌──┴───────────┐
 ▼              ▼
Users         Servers
 │              │
 └──────┬───────┘
        ▼
   [Sensitive DB]
```

And immediately start asking:

```text
Who can reach the Web Server?
Who can reach the Database?
Can Users reach the Database directly?
Can the Database reach the Internet?
Can the Web Server reach internal systems?
What happens if the Web Server is compromised?
Can an attacker move from Web → Internal?
Can Users communicate with each other?
Where are logs generated?
Where can we capture traffic?
What should normal traffic look like?
What traffic would indicate reconnaissance?
What traffic would indicate lateral movement?
```

**That is the foundation we need before Level 2.**

Level 2 will then move from *understanding network security components* toward **actual security analysis**: traffic patterns, packet captures, protocol behavior, scanning, reconnaissance detection, network attacks, IDS/IPS, network monitoring, and beginning to think like a SOC analyst examining a real network.
