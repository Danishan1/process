# Subject 1 — Networking

### Level 1 — Basic Questions

**Goal:** Build a strong networking foundation while continuously connecting networking concepts to cybersecurity.

### A. Networking Fundamentals

1. **What is a computer network, and why do computers need networks to communicate with each other? Explain the basic components involved in communication between two devices.**

2. **What is the difference between a LAN, WAN, MAN, PAN, and the Internet? From a cybersecurity perspective, why does the size and scope of a network matter?**

3. **What is the difference between a client-server network and a peer-to-peer network? What security advantages and disadvantages can each architecture introduce?**

4. **What are a hub, switch, router, firewall, access point, and modem? Explain the primary purpose of each and identify which layer or layers of networking they primarily operate at.**

5. **What is a network topology? Explain star, bus, ring, mesh, and hybrid topologies. How could the topology of a network influence its security and potential attack surface?**

6. **What happens at a high level when one computer sends data to another computer over a network? Describe the journey from the originating application to the destination application.**

7. **What is bandwidth, throughput, latency, jitter, and packet loss? Why are these concepts important not only for network performance but also for security monitoring?**

8. **What is the difference between a physical network and a logical network? Why might two devices appear to be on the same physical infrastructure but belong to different logical networks?**

## B. OSI and TCP/IP Models

9. **What is the OSI model, and why was it created? Explain all seven layers and the primary responsibility of each layer.**

10. **What is the TCP/IP model? How does it differ from the OSI model, and why is the TCP/IP model more closely associated with real-world Internet communication?**

11. **What is encapsulation? Explain how application data becomes a segment, packet, frame, and ultimately electrical/radio signals as it travels through a network.**

12. **What is decapsulation, and how does the receiving system reverse the encapsulation process?**

13. **Why is understanding the OSI/TCP-IP model useful to a cybersecurity professional? Give examples of attacks or security controls associated with different layers.**

14. **If a user can access a server by IP address but cannot access it by hostname, which networking layers or services would you investigate first, and why?**

15. **If a network connection completely fails, how would you use the OSI model to systematically troubleshoot the problem from the physical layer upward?**

## C. Ethernet and MAC Addresses

16. **What is Ethernet, and why is it fundamental to modern local-area networks? Explain the basic structure and purpose of an Ethernet frame.**

17. **What is a MAC address? Why does a network interface need one, and how is it different from an IP address?**

18. **What are unicast, broadcast, and multicast transmissions? Give a practical example of each and explain their security implications.**

19. **What is a broadcast domain? Why can excessive broadcast traffic become a performance or security concern?**

20. **What is a collision domain? How did switches change the behavior of collision domains compared with traditional hubs?**

21. **How does a switch learn MAC addresses and build its MAC address table? What happens when a switch does not know the destination MAC address?**

22. **What is MAC address spoofing? Why might an attacker attempt to change or impersonate a MAC address, and what defensive measures can organizations use?**

## D. IP Addressing

23. **What is an IP address, and why is IP addressing necessary for communication between devices across different networks?**

24. **What is the difference between IPv4 and IPv6? Why was IPv6 introduced, and what security considerations arise when an organization operates both protocols?**

25. **What is the difference between public and private IPv4 addresses? Why are private addresses commonly used inside organizations?**

26. **What are the private IPv4 address ranges, and why is it important for a cybersecurity professional to recognize them when analyzing network traffic?**

27. **What is a loopback address? Why is loopback communication useful, and why can services listening only on loopback be safer than services exposed to the network?**

28. **What is the difference between a static IP address and a dynamically assigned IP address? What security and operational considerations exist for each?**

29. **What is a subnet mask, and how does it determine which portion of an IPv4 address represents the network and which portion represents the host?**

30. **What is CIDR notation? Explain what `/8`, `/16`, `/24`, and `/32` mean and why CIDR is important for network design and security rules.**

## E. Subnetting

31. **Why do organizations divide networks into subnets instead of placing every device into one large network? Explain both performance and security benefits.**

32. **Given an IPv4 address and subnet mask, how would you determine the network address, broadcast address, and usable host range? Explain the process rather than simply giving the answer.**

33. **What is the difference between a network address, host address, and broadcast address? Why can these addresses not normally be assigned to ordinary hosts?**

34. **What is the purpose of subnetting from a security perspective? Explain how subnet boundaries can help limit the movement of an attacker.**

35. **What is the difference between `/24`, `/25`, `/26`, `/27`, and `/28` networks? Explain how the number of available hosts changes as the prefix becomes larger.**

36. **What is a default gateway, and why does a host need one when communicating with a device outside its local subnet?**

## F. ARP

37. **What is ARP, and why is it necessary in an IPv4 Ethernet network when a device already knows the destination IP address?**

38. **Walk through an ARP request and ARP reply between two devices on the same network. What information does each device learn?**

39. **What is an ARP cache? Why do operating systems maintain one instead of performing an ARP request for every packet?**

40. **What is ARP spoofing or ARP poisoning? Explain how an attacker can abuse the trust relationship between IP addresses and MAC addresses.**

41. **What could an attacker accomplish after successfully performing ARP poisoning on a local network? Explain the potential impact on confidentiality, integrity, and availability.**

42. **How could a defender detect suspicious ARP activity? What network evidence might indicate that ARP poisoning is occurring?**

## G. TCP and UDP

43. **What is TCP, and why is it described as connection-oriented? Explain what TCP provides that a simple connectionless protocol does not.**

44. **Explain the TCP three-way handshake using SYN, SYN-ACK, and ACK. Why is understanding this process important when analyzing network traffic?**

45. **What are TCP sequence numbers and acknowledgment numbers? Why are they necessary for reliable communication?**

46. **What is UDP, and how does it differ from TCP? Why might an application deliberately choose UDP despite its lack of TCP-style reliability mechanisms?**

47. **Compare TCP and UDP from a cybersecurity perspective. What kinds of security monitoring or attacks might be associated with each?**

48. **What happens when a TCP connection is terminated? Explain the general purpose of FIN and ACK messages.**

## H. Ports and Sockets

49. **What is a network port? Why does an IP address alone not identify the specific application or service communicating with a machine?**

50. **What is a socket, and how are an IP address and port number used together to identify a network communication endpoint?**

51. **What are well-known, registered, and dynamic/private ports? Why should a security professional understand these categories?**

52. **Why is an open port not automatically equivalent to a vulnerability? What additional information would you need before deciding whether an exposed service represents a security risk?**

53. **Why should defenders care about unnecessary listening services? Explain how an unused or forgotten service can increase an organization's attack surface.**

54. **If you discover that a server has ports 22, 80, 443, and 3389 open, what questions would you ask before deciding whether the server is securely configured?**

## I. DNS

55. **What is DNS, and why is it necessary for normal Internet usage? Explain how a human-readable domain name is translated into an IP address.**

56. **What is the difference between a DNS resolver, recursive DNS server, authoritative DNS server, root server, and TLD server? Explain how they work together during a lookup.**

57. **What are A, AAAA, CNAME, MX, NS, TXT, and PTR DNS records? Give a practical purpose for each.**

58. **What is DNS caching? Why is caching beneficial for performance, and what security or troubleshooting problems can stale DNS information create?**

59. **Why is DNS security important? Explain how attackers could abuse DNS for phishing, redirection, reconnaissance, or command-and-control without going into advanced exploitation techniques.**

60. **If a user can access a website using its IP address but not its domain name, how would you determine whether DNS is responsible for the problem?**

## J. DHCP

61. **What is DHCP, and why do most client devices use it instead of requiring administrators to manually configure every IP address?**

62. **Explain the DHCP DORA process: Discover, Offer, Request, and Acknowledgment. What happens during each stage?**

63. **What information can a DHCP server provide to a client besides an IP address? Why are options such as the default gateway and DNS server important?**

64. **What is a DHCP lease? Why does a device not necessarily keep the same dynamically assigned IP address forever?**

65. **What is a rogue DHCP server? How could an attacker abuse one, and what security consequences could result?**

## K. ICMP and Basic Troubleshooting

66. **What is ICMP, and why does it exist even though it does not carry normal application data like HTTP?**

67. **How does the `ping` utility use ICMP to test connectivity? What can a successful or unsuccessful ping tell you—and what can it not tell you?**

68. **What is TTL in an IP packet? How does TTL help prevent packets from circulating indefinitely, and how is it used by tools such as traceroute?**

69. **If a host responds to ping but its web application cannot be reached, what does that tell you about the network problem? What would you investigate next?**

70. **Why might a host fail to respond to ping even though it is online and reachable through another service? Why should a security professional avoid assuming that "no ping response" means "host is down"?**

## L. Routing and Network Boundaries

71. **What is routing, and why is it necessary when two devices belong to different IP networks?**

72. **What is a routing table? What information does a host or router use from a routing table to decide where to send a packet?**

73. **What is a default route, and why is it particularly important for ordinary client devices?**

74. **What is the difference between a switch and a router from the perspective of how they make forwarding decisions?**

75. **Why are routers and network boundaries important from a cybersecurity perspective? Explain how segmentation and routing can restrict communication between systems.**

## M. NAT

76. **What is NAT, and why became it widely used with IPv4 networks?**

77. **Explain how a private IP address can communicate with a public Internet server through NAT. What information is translated during the process?**

78. **What is PAT, and how does it allow many internal devices to share one public IP address?**

79. **Does NAT itself constitute a complete security mechanism? Explain why NAT should not be confused with a firewall or comprehensive network security control.**

## N. Networking From a Cybersecurity Perspective

80. **Why is networking knowledge considered one of the most important foundations for a cybersecurity professional, especially for someone interested in Red Team and Blue Team work?**

81. **An attacker scans a network and discovers several hosts, open ports, and running services. Which networking concepts did the attacker rely on to obtain this information?**

82. **A defender sees repeated connection attempts to many ports across multiple internal machines. What networking concepts would help the defender recognize this as potentially suspicious scanning activity?**

83. **Why can an unnecessarily exposed network service become part of an organization's attack surface? Explain the relationship between IP address, port, service, vulnerability, and risk.**

84. **How can network segmentation reduce the impact of a successful compromise? Give an example involving a user workstation, an application server, and a database server.**

85. **Why is packet capture useful to both Red Team and Blue Team professionals? Explain how the same network traffic can provide useful information to an attacker and evidence to a defender.**

86. **How could DNS, DHCP, ARP, TCP, UDP, and HTTP logs or packet captures each contribute different pieces of evidence during a security investigation?**

87. **Imagine that a company's internal network contains user PCs, servers, printers, cameras, and IoT devices on one large subnet. What security problems could arise from this design, and how could network segmentation improve the situation?**

88. **Suppose an attacker gains control of a machine inside a company's network. Why is understanding routing, subnets, ports, services, and network boundaries important for determining what the attacker could potentially reach next?**

89. **A security analyst discovers that a server is communicating with an unfamiliar external IP address. What networking information would you want to collect before determining whether the connection is malicious?**

90. **Imagine that you are performing an authorized security assessment of a small organization. Starting only with the network information you are given, how would you reason about hosts, subnets, gateways, ports, services, and exposed attack surface?**

## Level 1 — What These 90 Questions Should Give You

I intentionally went beyond the minimum **50** and made this **90-question foundation set**, because Networking is our **#1 subject and the foundation for almost everything that follows**.

By the time you can answer these properly, you should be able to reason through:

```text
Physical Network
      ↓
Ethernet
      ↓
MAC
      ↓
ARP
      ↓
IP
      ↓
Subnet
      ↓
Router
      ↓
TCP / UDP
      ↓
Port
      ↓
Service
      ↓
DNS / DHCP / ICMP
      ↓
HTTP / HTTPS
      ↓
Network Traffic
      ↓
Attack Surface
      ↓
Detection / Defense
```

And, importantly, you should start seeing the **security chain**:

```text
NETWORK
   ↓
HOST
   ↓
IP
   ↓
PORT
   ↓
SERVICE
   ↓
APPLICATION
   ↓
VULNERABILITY
   ↓
EXPLOIT
   ↓
IMPACT
```

That's the mental model we want to develop.

### How we should eventually use these questions

Don't treat this as a question bank to memorize.

For each question, your eventual answer should ideally contain:

1. **Definition** — What is it?
2. **Mechanism** — How does it work?
3. **Example** — Where is it used?
4. **Troubleshooting** — What happens when it breaks?
5. **Security** — Why does it matter?
6. **Offensive perspective** — How could it be abused?
7. **Defensive perspective** — How could we detect/prevent it?

That will turn a "90-question networking list" into a genuine **networking-to-cybersecurity reasoning framework**.
