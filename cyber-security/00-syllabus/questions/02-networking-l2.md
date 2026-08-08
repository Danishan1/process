# Networking — Level 2

### Intermediate Question Bank

**Target: 80 questions**

## A. Network Architecture & Design

1. **You are asked to design a network for a small company with 50 employees, several servers, printers, guest devices, and Internet access. How would you divide the network logically, and what factors would influence your design decisions?**

2. **What is network segmentation, and why is it one of the most important concepts in defensive cybersecurity? Explain how segmentation can limit lateral movement after an attacker compromises one machine.**

3. **What is a DMZ? Why would an organization place a public-facing web server in a DMZ instead of directly inside its internal network?**

4. **How would you design a network containing users, application servers, database servers, security monitoring systems, and Internet-facing services so that compromise of one zone does not automatically expose everything else?**

5. **What is the difference between physical segmentation and logical segmentation? Explain how VLANs can provide logical separation without requiring completely separate physical switches.**

6. **What is a three-tier network architecture? Explain the access, distribution, and core layers and discuss why hierarchical network design can improve manageability and security.**

7. **What is a flat network, and why is a flat network generally more dangerous from a cybersecurity perspective than a properly segmented network?**

8. **An organization places employees, servers, printers, security cameras, and IoT devices on the same subnet. What risks does this architecture create, and how would you redesign it?**

## B. VLANs & Switching

9. **What is a VLAN, and how does it allow multiple logical networks to exist on the same physical switching infrastructure?**

10. **What is the difference between an access port and a trunk port? When would each be used?**

11. **What is 802.1Q tagging, and how does a switch use VLAN tags when traffic travels across a trunk link?**

12. **What is inter-VLAN routing? Why is a router or Layer 3 switch required when devices in different VLANs need to communicate?**

13. **How can VLANs improve security, and why should you not assume that simply creating VLANs automatically makes a network secure?**

14. **What is VLAN hopping at a conceptual level? Why can incorrect switch configuration potentially allow traffic to cross VLAN boundaries?**

15. **What is MAC flooding? Explain the relationship between a switch's MAC address table and the potential security impact of overwhelming that table.**

16. **What network controls can organizations use to reduce risks associated with unauthorized devices connecting to switch ports?**

## C. Routing

17. **Explain how a router determines where to forward a packet when multiple routes could potentially match the destination address.**

18. **What is longest-prefix matching, and why is it important when a router chooses between multiple routes?**

19. **What is the difference between static and dynamic routing? What are the operational and security advantages and disadvantages of each?**

20. **What are routing protocols? Explain at a high level why protocols such as OSPF and BGP are necessary in larger networks.**

21. **What is OSPF, and how does it differ conceptually from a simple static routing configuration?**

22. **Why can incorrect routing configuration cause security problems even when firewalls and authentication controls are properly configured?**

23. **What is asymmetric routing? Why can asymmetric traffic complicate firewalling, intrusion detection, and network monitoring?**

24. **A server can reach the Internet but cannot communicate with another internal subnet. How would you systematically investigate whether routing is responsible?**

## D. DHCP & DNS in Real Networks

25. **How would you design DHCP for several different VLANs? Why might DHCP relay be necessary in a segmented network?**

26. **What is DHCP starvation? Explain conceptually how an attacker could abuse DHCP resource allocation and what consequences it could have.**

27. **How could a rogue DHCP server manipulate clients' network configuration? What information could an attacker potentially control?**

28. **What is split-horizon or split-brain DNS? Why might an organization intentionally return different DNS answers to internal and external clients?**

29. **What is DNS zone transfer? Why could improperly exposed zone-transfer functionality provide valuable information to an attacker?**

30. **What is DNS poisoning? How is it different conceptually from ordinary DNS caching, and why is DNS integrity important?**

31. **How can DNS logs help a Blue Team analyst identify potentially suspicious activity? Give examples of patterns that might deserve investigation.**

32. **A user reports that one website cannot be accessed while other websites work normally. Explain how you would determine whether the problem is caused by DNS, routing, TCP connectivity, TLS, or the application itself.**

## E. TCP/IP Deepening

33. **Explain how TCP provides reliable communication despite packets potentially being lost, duplicated, delayed, or received out of order.**

34. **What are TCP flags such as SYN, ACK, FIN, RST, and PSH used for? Why are unusual combinations of TCP flags potentially interesting from a security perspective?**

35. **What is a TCP reset (RST), and under what legitimate and abnormal circumstances might you observe one?**

36. **What is a half-open TCP connection? Why is understanding this state important when studying connection handling and denial-of-service attacks?**

37. **What is a TCP SYN flood? Explain the basic resource-exhaustion concept behind the attack and describe defensive approaches at a high level.**

38. **Why might a security analyst examine TCP connection patterns rather than simply looking at individual packets? What can connection behavior reveal?**

39. **How can packet fragmentation affect network communication and security monitoring? Why can fragmented traffic complicate inspection?**

40. **What is MTU, and what problems can occur when different network segments have incompatible assumptions about maximum packet size?**

## F. NAT, Firewalls & Network Boundaries

41. **Explain the difference between NAT, a stateful firewall, and a proxy. Why are these technologies sometimes confused with one another?**

42. **What is a stateful firewall? How does it determine whether an incoming packet belongs to an established connection?**

43. **What is a stateless firewall or packet filter? What information can it use when deciding whether to allow or block traffic?**

44. **How would you design firewall rules for a web application that needs public HTTPS access but should not expose its database directly to the Internet?**

45. **Why is the principle of least privilege important when designing firewall rules? Explain why "allow everything except known bad traffic" can be dangerous.**

46. **What is an ingress rule and what is an egress rule? Why should organizations pay attention to outbound traffic instead of focusing only on incoming connections?**

47. **A compromised workstation is allowed to make unrestricted outbound connections to the Internet. What opportunities could this provide to an attacker, and how could egress filtering reduce the risk?**

48. **What is a firewall misconfiguration? Give several examples of configurations that could unintentionally expose sensitive systems.**

## G. Network Reconnaissance

49. **What is network reconnaissance, and why is it generally the first technical phase of an authorized security assessment?**

50. **What is host discovery? Explain the different kinds of evidence that can indicate that a host is alive without relying on a single protocol.**

51. **What is port scanning? Explain what an attacker or security tester is actually trying to learn by scanning ports.**

52. **What is the difference between discovering an open port and identifying the service running behind that port? Why is service enumeration important?**

53. **Why is version detection valuable during vulnerability assessment? What additional information does it provide beyond simply knowing that a port is open?**

54. **Why should a security professional distinguish between "open," "closed," and "filtered" ports when interpreting scan results?**

55. **How might network firewalls, IDS/IPS systems, and rate limiting affect the results of an authorized network scan?**

56. **A scan reveals TCP 22, 80, 443, and 445 on a server. How would you reason about the potential attack surface without immediately assuming that any particular service is vulnerable?**

## H. Wireshark & Packet Analysis

57. **What information can you obtain from a packet capture that you cannot easily obtain from a simple ping test?**

58. **How would you identify a TCP three-way handshake inside a Wireshark capture? What fields would you examine?**

59. **How would you identify a DNS query and its corresponding response in a packet capture? What information would you extract?**

60. **How could you distinguish between normal HTTP traffic and suspicious HTTP behavior using packet captures?**

61. **What is a Wireshark display filter? Why are filters important when investigating large packet captures?**

62. **How would you investigate a suspected connection between an internal host and an unfamiliar external IP address using packet capture data?**

63. **Suppose you observe a single internal host communicating with hundreds of different IP addresses over a short period. What hypotheses could explain this behavior, and what additional evidence would you collect?**

64. **Why is encrypted HTTPS traffic still useful to a network defender even though the HTTP contents cannot normally be read directly from the packet capture?**

## I. Network Attacks & Abuse

65. **How can an attacker use network reconnaissance to reduce uncertainty before attempting further activity? Explain the relationship between reconnaissance, enumeration, and exploitation.**

66. **What is a man-in-the-middle attack? Explain the general concept without focusing on a particular tool or exploit.**

67. **What network conditions make man-in-the-middle attacks more feasible? How can encryption and certificate validation reduce the risk?**

68. **What is ARP spoofing, and how could it potentially be used as part of a larger man-in-the-middle attack on a local network?**

69. **What is a denial-of-service attack? Explain the difference between exhausting bandwidth, network devices, connection state, and application resources.**

70. **What is the difference between a DoS and a DDoS attack? Why is distributed traffic more difficult to mitigate?**

71. **How could an attacker abuse an unnecessarily exposed network service without immediately exploiting a software vulnerability? Consider authentication, configuration, and information disclosure.**

72. **Why does reducing the number of exposed services generally reduce an organization's attack surface? Explain the principle of attack-surface minimization.**

## J. Network Defense

73. **What is network access control, and how can technologies such as NAC help organizations control which devices are allowed onto a network?**

74. **What is an IDS? Explain how it differs from an IPS and a traditional firewall.**

75. **What is the difference between signature-based and behavior/anomaly-based network detection? What are the strengths and weaknesses of each?**

76. **What is network segmentation, and how could it be used to isolate high-value assets such as domain controllers, databases, and security infrastructure?**

77. **What is a honeypot? How could deliberately exposed but monitored systems help defenders detect unauthorized activity?**

78. **Why is centralized network logging valuable? What information should a security team ideally be able to correlate across firewalls, DNS, DHCP, VPN, and endpoint systems?**

79. **What is network baseline behavior? Why is understanding normal traffic important when trying to detect abnormal or malicious activity?**

80. **A company discovers that an attacker compromised one workstation but did not reach the database server. What network-security controls might have limited the attacker's movement, and how would you investigate which control was effective?**

## K. Troubleshooting as a Security Skill

81. **A user can reach an internal server by IP address but not by hostname. Walk through a structured troubleshooting process and explain which networking components you would test first.**

82. **A user can resolve a domain name but cannot establish a TCP connection to the destination. Which parts of the network stack would you investigate next?**

83. **A TCP connection succeeds, but the application returns an error. Why does this indicate that lower-layer networking is probably functioning, and what should you investigate next?**

84. **A server suddenly becomes unreachable from one subnet but remains reachable from another. What network configuration or security controls could explain this behavior?**

85. **A network becomes extremely slow during a suspected security incident. How would you distinguish between congestion, packet loss, routing problems, a broadcast storm, and malicious traffic?**

86. **Why is systematic troubleshooting more valuable than randomly changing network configurations? Explain how a layered troubleshooting methodology can prevent introducing additional security problems.**

## L. Cybersecurity Reasoning

87. **You discover an Internet-facing server with an outdated service, an open management port, and weak access controls. How would you determine which issue represents the greatest risk rather than simply labeling all three as vulnerabilities?**

88. **An attacker has obtained access to an internal workstation. Using only networking concepts, what information would you want to understand about the environment before assessing the possible impact of the compromise?**

89. **How can an organization's network architecture influence the difference between a small endpoint compromise and a major enterprise-wide security incident?**

90. **Explain how the following concepts connect together during a real security incident: IP address, subnet, MAC address, ARP, DNS, TCP, port, service, firewall, routing, logging, and monitoring.**

## M. Red Team + Blue Team Scenarios

91. **During an authorized assessment, you discover that several internal systems expose services that are unnecessary for their intended roles. How would you assess the risk, and what would you recommend to the organization?**

92. **A SOC analyst notices a workstation making repeated connections to many internal hosts on sequential ports. What could this behavior represent, what networking concepts explain it, and what evidence would you collect to determine whether it is malicious?**

93. **A security team detects unusual ARP activity from one workstation. Explain how you would investigate the event, what legitimate explanations you would consider, and what evidence could confirm ARP poisoning.**

94. **A firewall shows that an internal workstation has established an outbound connection to an unfamiliar external server. What questions would you ask about DNS, IP reputation, destination port, process ownership, timing, and traffic volume before declaring an incident?**

95. **An attacker compromises a web server located in a DMZ. What networking controls should ideally prevent that attacker from directly reaching the internal database, and what evidence would you examine to determine whether those controls worked?**

96. **An organization has no network segmentation. A single employee workstation is compromised through phishing. Explain, step by step, why the absence of segmentation could significantly increase the potential impact of the incident.**

97. **You are performing an authorized assessment of a company's network. You discover an exposed management service that is accessible from a network where it should not be. What questions would you ask before testing it, and how would you communicate the finding responsibly?**

98. **A network defender blocks a suspicious IP address, but the suspicious communication continues using different IP addresses. What networking concepts might explain this behavior, and why is blocking individual IP addresses sometimes insufficient?**

99. **A company claims that its network is secure because it has a firewall. Explain why a firewall alone cannot provide comprehensive network security, and identify other controls that should work alongside it.**

100. **Design a hypothetical secure network for a medium-sized organization. Explain where you would place users, servers, databases, domain controllers, security monitoring systems, guest devices, IoT devices, and Internet-facing applications, and justify your segmentation, routing, firewall, and monitoring decisions.**

## What Level 2 Should Accomplish

Level 1 asked:

> **"What is this?"**

Level 2 should force you toward:

> **"How does this behave in a real environment?"**

and eventually:

> **"What happens if someone abuses it?"**

The progression we're creating is:

```text
LEVEL 1
Understand the components
        ↓
LEVEL 2
Understand how components interact
        ↓
LEVEL 2
Understand how networks are designed
        ↓
LEVEL 2
Troubleshoot the network
        ↓
LEVEL 2
Analyze network traffic
        ↓
LEVEL 2
Recognize attack surface
        ↓
LEVEL 2
Think like attacker + defender
        ↓
LEVEL 3
Operate at advanced/enterprise level
```

### The most important Level 2 shift

You should start being able to look at something like:

```text
Internet
   │
Firewall
   │
DMZ
 ┌─┴───────────────┐
 │                 │
Web Server      Mail Server
 │
Firewall
 │
Internal Network
 │
 ├── Users
 ├── AD
 ├── Applications
 │
 └── Database
```

and **immediately start asking security questions**:

* What can communicate with what?
* Which ports are exposed?
* Which services actually need to be exposed?
* Where are the trust boundaries?
* Where could an attacker move?
* What happens if the web server is compromised?
* Can the DMZ reach the database?
* Can users reach the domain controller?
* What outbound traffic is allowed?
* Where are the logs?
* How would we detect scanning?
* How would we detect lateral movement?
* What happens if DNS is compromised?
* What happens if a workstation is compromised?
* Which controls contain the blast radius?

That is the beginning of **cybersecurity-level networking**, rather than simply networking knowledge.
