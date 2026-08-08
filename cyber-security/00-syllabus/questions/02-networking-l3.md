# Networking — Level 3

### Advanced Question Bank

## A. Enterprise Network Architecture

1. **How would you design a secure enterprise network for an organization with thousands of users, multiple offices, data centers, remote workers, cloud workloads, third-party partners, and Internet-facing applications? Explain the major trust zones and the reasoning behind them.**

2. **What is the difference between north-south and east-west traffic? Why is this distinction important when designing security monitoring and segmentation?**

3. **How should an organization design network segmentation around high-value assets such as domain controllers, databases, certificate authorities, backup systems, and security infrastructure?**

4. **What is microsegmentation? How does it differ from traditional VLAN-based segmentation, and why can microsegmentation be particularly valuable against lateral movement?**

5. **What is a security zone? How should communication between different security zones be controlled and monitored?**

6. **How would you design a network where a compromised employee workstation has as little access as possible to critical infrastructure while still allowing the employee to perform normal work?**

7. **What is a zero-trust network architecture? How does its security philosophy differ from the traditional idea of trusting devices because they are inside the corporate network?**

8. **Why should network location alone not be considered sufficient evidence that a connection is trustworthy? Explain the relationship between identity, device posture, network location, and authorization.**

9. **How would you design network boundaries for an organization that has both traditional on-premises infrastructure and cloud environments?**

10. **What architectural decisions can reduce the blast radius of a successful network compromise?**

## B. Advanced Switching

11. **Explain how STP prevents Layer 2 switching loops. Why are switching loops potentially catastrophic for network availability?**

12. **What is a broadcast storm? How can a Layer 2 problem spread across a large network?**

13. **What are STP root bridges and root ports? Why does their placement matter in enterprise network design?**

14. **What is BPDU Guard? Why is it useful on access ports, and what security problem is it intended to reduce?**

15. **What is Port Security? How can it restrict which devices are allowed to communicate through a switch port?**

16. **What are DHCP Snooping and Dynamic ARP Inspection? Explain how they can work together to reduce certain Layer 2 attacks.**

17. **How can an attacker abuse weaknesses in Layer 2 trust relationships? Discuss ARP, DHCP, MAC addresses, and VLAN boundaries conceptually.**

18. **What is a private VLAN? How can private VLANs provide additional isolation within a subnet?**

19. **How would you investigate a suspected Layer 2 attack when you only have access to switch logs, ARP information, and network traffic?**

20. **Why can Layer 2 security controls be particularly important in environments where an attacker has already obtained physical or logical access to an internal network?**

## C. Advanced Routing

21. **Explain the conceptual differences between distance-vector, link-state, and path-vector routing protocols. What kinds of environments are they designed for?**

22. **How does OSPF build a view of the network topology? Why does this allow routers to calculate routes dynamically?**

23. **What are OSPF areas, and why might an enterprise divide a large OSPF environment into multiple areas?**

24. **What is route summarization? How can summarization improve scalability while potentially affecting troubleshooting and security visibility?**

25. **What is BGP, and why is it fundamental to communication between large autonomous systems on the Internet?**

26. **Why is BGP security important? Explain conceptually how incorrect route advertisements could affect Internet traffic.**

27. **What is route hijacking? How could incorrect or malicious routing information redirect traffic away from its intended destination?**

28. **What is asymmetric routing, and why can it cause problems for stateful firewalls and security monitoring systems?**

29. **A network monitoring system sees only one direction of a TCP conversation. What networking architectures or routing behaviors could explain this?**

30. **How would you troubleshoot a situation where packets reach a destination but return traffic follows a completely different path?**

## D. IPv6 Security

31. **Why was IPv6 designed, and what fundamental differences exist between IPv4 and IPv6 addressing?**

32. **How does IPv6 Neighbor Discovery differ from IPv4 ARP? Why is understanding this difference important for security professionals?**

33. **What are IPv6 Router Advertisements? How could unauthorized router advertisements affect clients on a network?**

34. **What are IPv6 link-local addresses? Why can they matter during network reconnaissance and troubleshooting?**

35. **What security problems can arise when an organization enables IPv6 without implementing equivalent IPv6 security controls?**

36. **Why can IPv6 create unexpected attack paths in an environment where administrators primarily monitor IPv4?**

37. **How should firewalls and monitoring systems account for both IPv4 and IPv6 traffic?**

38. **What is dual-stack networking? What additional complexity does it create for security architecture and incident response?**

39. **How would you investigate a suspicious IPv6 connection in an environment where most administrators are accustomed to analyzing IPv4 addresses?**

40. **Why should disabling or ignoring IPv6 be considered carefully rather than assumed to be a security solution?**

## E. Advanced DNS Security

41. **How does DNS resolution work from a client request through recursive resolution to an authoritative answer? Explain the security implications at each stage.**

42. **What is DNSSEC? What security problem does it address, and what problem does it not solve?**

43. **How does DNSSEC establish authenticity and integrity for DNS responses?**

44. **What is DNS tunneling? Explain conceptually how DNS can be abused as a communication channel and why defenders might look for unusual DNS behavior.**

45. **What DNS characteristics could indicate potential command-and-control activity? Consider query frequency, domain patterns, response sizes, entropy, and unusual record types.**

46. **How can attackers use DNS during reconnaissance? What information can DNS records unintentionally reveal about an organization?**

47. **Why should internal DNS architecture be carefully separated from public DNS infrastructure?**

48. **How would you investigate a workstation that suddenly generates thousands of DNS queries to unusual domains?**

49. **What is DNS over HTTPS and DNS over TLS? Why can encrypted DNS improve privacy while simultaneously creating challenges for enterprise monitoring?**

50. **How should an enterprise balance DNS privacy, availability, performance, and security monitoring?**

## F. Advanced Network Traffic Analysis

51. **How would you establish a baseline for normal network behavior in an enterprise environment? What dimensions of traffic would you measure?**

52. **Why is baseline behavior more useful than simply maintaining a list of "known malicious IP addresses"?**

53. **How can flow data such as NetFlow or IPFIX complement full packet capture? What information does each provide?**

54. **What is the difference between packet-level visibility and flow-level visibility? When would an analyst prefer one over the other?**

55. **How would you identify possible lateral movement using network telemetry alone? What traffic patterns might be suspicious?**

56. **How could unusual SMB, RDP, SSH, WinRM, or database traffic indicate lateral movement? What additional evidence would you seek?**

57. **How can periodic outbound connections help identify possible command-and-control behavior? Why is periodicity alone insufficient to classify traffic as malicious?**

58. **How can beaconing behavior appear in network traffic? What statistical or behavioral characteristics might an analyst examine?**

59. **How can encrypted traffic still reveal useful metadata even when its contents cannot be inspected?**

60. **How would you investigate a potentially compromised workstation when you have firewall logs, DNS logs, flow data, and packet captures but no endpoint telemetry?**

## G. IDS / IPS / Network Detection

61. **How does an intrusion detection system determine that network traffic may represent an attack? Explain the difference between signatures, heuristics, and behavioral detection.**

62. **What is the difference between false positives and false negatives in network detection? Why is reducing one sometimes difficult without increasing the other?**

63. **How should a SOC analyst prioritize a network alert rather than treating every IDS alert as equally important?**

64. **What information should be correlated with a network alert before escalating it into a potential incident?**

65. **How can network segmentation improve the quality of security alerts as well as reduce attack impact?**

66. **What is network detection engineering? How would you design a detection for suspicious lateral movement while minimizing unnecessary alerts?**

67. **How can attackers attempt to evade network-based detection? Discuss concepts such as encryption, fragmentation, protocol abuse, traffic variation, and living-off-the-network behavior.**

68. **Why should network detection not depend entirely on signatures for known attacks?**

69. **How can defenders identify suspicious behavior when the exact malware or attack technique is unknown?**

70. **Design a conceptual detection strategy for a workstation that suddenly begins scanning internal systems, authenticating to unusual servers, and making new outbound connections.**

## H. Network Forensics

71. **What is network forensics? How does it differ from general network troubleshooting?**

72. **What evidence should be preserved when investigating a suspected network intrusion?**

73. **How can packet captures help establish a timeline during a security incident?**

74. **How would you determine which internal host initiated a suspicious communication when multiple NAT devices are involved?**

75. **Why are accurate timestamps and time synchronization important for network forensics?**

76. **What is the relationship between source IP, source port, destination IP, destination port, protocol, timestamp, and session when reconstructing network activity?**

77. **How can DHCP logs help identify which device was using a particular IP address at a specific time?**

78. **How can DNS logs help connect an IP address observed in network traffic to the domain name that the client attempted to access?**

79. **How would you investigate whether a suspicious external connection originated from malware, a legitimate application, or an administrator's activity?**

80. **What limitations should an investigator consider when reconstructing an incident from network evidence alone?**

## I. VPN & Remote Access

81. **What problem does a VPN solve? Explain the difference between site-to-site VPNs and remote-access VPNs.**

82. **How does encryption protect traffic traveling through an untrusted network such as the public Internet?**

83. **What security risks can arise when VPN users receive broad access to internal networks after authentication?**

84. **How should network segmentation apply to remote VPN users? Why should successful VPN authentication not automatically mean unrestricted internal access?**

85. **What is split tunneling? What are its advantages and security concerns?**

86. **How could a compromised remote endpoint create a security risk even when the VPN itself uses strong encryption and authentication?**

87. **How would you design remote-access networking for administrators who need access to highly sensitive infrastructure?**

88. **What network evidence might indicate suspicious use of a VPN account during a security incident?**

## J. Proxy, Load Balancer & Reverse Proxy

89. **What is a forward proxy? How can it provide security, visibility, filtering, and access control for outbound traffic?**

90. **What is a reverse proxy? Why might an organization place one in front of Internet-facing web applications?**

91. **How can a load balancer improve availability while also affecting security monitoring and source-IP visibility?**

92. **Why is accurate client-IP attribution important when traffic passes through multiple proxies, load balancers, NAT devices, or security gateways?**

93. **How can reverse proxies and Web Application Firewalls contribute to a defense-in-depth architecture?**

94. **What security problems could occur if a backend application trusts client-supplied headers that are intended to identify the original source IP?**

95. **How would you investigate suspicious web traffic when the application servers only see connections originating from a reverse proxy?**

## K. Network Security Architecture

96. **Design a defense-in-depth architecture for a public-facing web application. Explain how DNS, CDN, firewall, WAF, reverse proxy, application servers, database servers, monitoring, and logging could work together.**

97. **How would you architect a network so that compromising an Internet-facing web server does not provide a direct path to the organization's internal Active Directory environment?**

98. **How should security monitoring be positioned within an enterprise network so that an attacker cannot easily bypass all monitoring by moving between network segments?**

99. **Imagine an attacker has compromised a workstation and is attempting to discover and move toward domain controllers, file servers, databases, and backup infrastructure. Design a network architecture that makes this movement difficult and highly visible.**

100. **Design an enterprise network from the perspective of both the Red Team and Blue Team. Explain where an attacker would likely look for weaknesses, which trust boundaries would matter most, what telemetry defenders should collect, and how the architecture could contain a compromise.**

## What Networking Level 3 Should Give You

If Level 1 was:

> **"I understand networking."**

and Level 2 was:

> **"I can work with and analyze a real network."**

then Level 3 should move toward:

> **"I can think about an enterprise network as an attacker, defender, architect, and investigator."**

The progression now looks like:

```text
                    NETWORKING
                         │
          ┌──────────────┴──────────────┐
          │                             │
      FOUNDATION                    SECURITY
          │                             │
     IP / TCP / UDP                Attack Surface
     DNS / DHCP                    Segmentation
     Routing                       Monitoring
     Switching                     Detection
          │                             │
          └──────────────┬──────────────┘
                         │
                  ENTERPRISE NETWORK
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
   Architecture       Red Team          Blue Team
       │                 │                 │
   Segmentation       Recon             Detection
   Zero Trust         Enumeration       Monitoring
   DMZ                Network Paths     Hunting
   VPN                Attack Paths      Response
   Cloud              Lateral Movement  Forensics
       │                 │                 │
       └─────────────────┼─────────────────┘
                         │
                  SECURITY ENGINEERING
```

### The mental model we ultimately want

When you see an enterprise network, you shouldn't just see:

> **IP → Router → Switch → Server**

You should see:

> **Assets → Trust → Communication → Exposure → Attack Paths → Controls → Telemetry → Detection → Containment**

For example:

```text
                  INTERNET
                      │
                 [ DNS/CDN ]
                      │
                 [ Firewall ]
                      │
                    [WAF]
                      │
               [Reverse Proxy]
                      │
              ┌───────┴───────┐
              │               │
           Web Tier       API Tier
              │               │
              └───────┬───────┘
                      │
                 [Firewall]
                      │
                 App Servers
                      │
                 [Firewall]
                      │
                 Database
                      │
             ┌────────┴────────┐
             │                 │
        Monitoring         Backup
             │
          SIEM/SOC
```

Now the questions become:

* **What can reach the web tier?**
* **What can the web tier reach?**
* **Can the web tier reach the database directly?**
* **What happens if the web server is compromised?**
* **Can the attacker pivot from there?**
* **Which firewall should stop that movement?**
* **Would the SOC see the attempt?**
* **What DNS evidence would remain?**
* **What flow data would remain?**
* **What packet evidence would remain?**
* **Can the attacker bypass the monitoring point?**
* **What happens if the firewall itself is misconfigured?**
* **Where are the critical trust boundaries?**
* **How do we contain the compromised host?**

That is the level of networking knowledge we ultimately want for our **Red Team / Pentesting > Blue Team / SOC** career priority.

### Networking is now complete

We have:

| Level       | Questions | Primary goal                    |
| ----------- | --------: | ------------------------------- |
| **Level 1** |        90 | Foundations                     |
| **Level 2** |       100 | Real-world networking           |
| **Level 3** |       100 | Enterprise + security reasoning |
| **Total**   |   **290** | Deep networking foundation      |

And this is deliberately **more than the original 150-question target**. Networking deserves it because it underpins almost every other subject in our roadmap.
