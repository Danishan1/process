# SECURITY FUNDAMENTALS — LEVEL 1

Now we move from **Web Fundamentals** into **Security Fundamentals**, which is one of the most important subjects in our entire roadmap.

Networking taught us **how systems communicate**.
Linux taught us **how operating systems work**.
Python taught us **how to automate and build tools**.
Web Fundamentals taught us **how applications work and where their trust boundaries exist**.

Now Security Fundamentals gives us the **security language and mental framework** that connects all of them.

Our progression will be:

```text
Security Fundamentals — Level 1
        ↓
What is security?
What are we protecting?
What can go wrong?
How do attacks happen?
How do we reduce risk?
        ↓
Security Fundamentals — Level 2
        ↓
How security mechanisms actually work
        ↓
Security Fundamentals — Level 3
        ↓
How attackers, defenders, detection,
response and security architecture
work together
```

For Level 1, the objective is **not memorizing security terminology**. Every question should force you to think about:

> **Asset → Threat → Vulnerability → Attack → Impact → Control → Detection → Response → Recovery**


### Basic Question Bank

**150 Questions**

## A. Foundations of Cybersecurity

1. **What is cybersecurity, and what exactly are we trying to protect when we say that a system needs to be "secure"?**

2. **Why is cybersecurity not simply the process of preventing hackers from accessing computers?**

3. **What is information security, and how does it relate to cybersecurity?**

4. **What is the difference between security, privacy, and safety in the context of computing systems?**

5. **Why is it impossible to make a system absolutely secure against every possible threat?**

6. **What does it mean to manage security risk rather than attempt to eliminate all risk?**

7. **Why should cybersecurity be considered a continuous process rather than a one-time implementation?**

8. **What are the major areas that a cybersecurity professional needs to understand besides technical vulnerabilities?**

9. **Why does cybersecurity require understanding both technology and human behavior?**

10. **Explain how networking, operating systems, applications, users, organizations, and security controls fit together into a complete cybersecurity environment.**

## B. CIA Triad

11. **What is the CIA triad, and why is it considered one of the foundational models of information security?**

12. **What is confidentiality, and what kinds of situations can cause confidentiality to be lost?**

13. **What is integrity, and why is unauthorized modification sometimes more dangerous than unauthorized access?**

14. **What is availability, and why can a system be secure yet still fail its users if availability is not maintained?**

15. **Give a realistic example of a security incident that primarily affects confidentiality.**

16. **Give a realistic example of a security incident that primarily affects integrity.**

17. **Give a realistic example of a security incident that primarily affects availability.**

18. **Why can one security incident affect confidentiality, integrity, and availability simultaneously?**

19. **How would the security priorities of a banking system differ from those of a public information website?**

20. **Analyze a ransomware incident using the CIA triad. Explain how confidentiality, integrity, and availability could each be affected.**

## C. Assets

21. **What is a security asset, and why must an organization identify its assets before deciding how to protect them?**

22. **What types of digital assets might exist inside a typical organization?**

23. **Why should data be considered an asset even when it does not exist as a physical file?**

24. **Why can user accounts and identities be considered security assets?**

25. **Why are credentials and cryptographic keys particularly sensitive assets?**

26. **How can infrastructure such as servers, routers, databases, cloud resources, and endpoints become security assets?**

27. **Why should business processes themselves sometimes be treated as assets?**

28. **What is asset classification, and why might organizations classify information into different sensitivity levels?**

29. **Why should the importance of an asset influence the security controls applied to it?**

30. **Create an asset inventory for a hypothetical university. Identify its important systems, data, identities, infrastructure, and business processes and rank them according to security importance.**

## D. Threats

31. **What is a threat in cybersecurity?**

32. **How is a threat different from a vulnerability?**

33. **What is a threat actor?**

34. **What different categories of threat actors might target an organization?**

35. **Why can an accidental mistake by an employee be considered a security threat even when there is no malicious attacker?**

36. **What is an insider threat, and why can insiders sometimes have access that external attackers do not?**

37. **Why should organizations consider both intentional and unintentional threats?**

38. **How can natural events, hardware failures, power outages, and environmental problems become cybersecurity concerns?**

39. **Why should threat modeling consider the motivation and capabilities of potential attackers?**

40. **Create a threat profile for a small company. Identify realistic external, internal, accidental, technical, and environmental threats it should consider.**

## E. Vulnerabilities

41. **What is a vulnerability, and how does it differ from a threat?**

42. **How can vulnerabilities exist in software, hardware, configurations, processes, and human behavior?**

43. **Why does the existence of a vulnerability not automatically mean that a successful attack will occur?**

44. **What factors determine whether a particular vulnerability represents significant security risk?**

45. **Why can insecure default configurations create vulnerabilities even when the underlying software is functioning correctly?**

46. **What is misconfiguration, and why is it one of the common causes of security problems?**

47. **Why can outdated software create security vulnerabilities?**

48. **Why is vulnerability management more than simply installing patches?**

49. **Why should organizations prioritize vulnerabilities instead of treating every vulnerability as equally urgent?**

50. **Take a hypothetical vulnerable server and explain the chain from vulnerability to potential exploitation, impact, detection, remediation, and verification.**

## F. Risk

51. **What is cybersecurity risk?**

52. **How are threat, vulnerability, likelihood, and impact related to risk?**

53. **Why can a technically severe vulnerability represent relatively low organizational risk in one environment and extremely high risk in another?**

54. **What is the difference between likelihood and impact when evaluating security risk?**

55. **Why is risk assessment necessary when security resources are limited?**

56. **What is risk acceptance?**

57. **What is risk mitigation?**

58. **What is risk transfer, and when might an organization use it?**

59. **What is risk avoidance?**

60. **Imagine an organization discovers a serious vulnerability in a non-critical internal system. Explain how you would reason about its risk rather than automatically declaring it either "safe" or "critical."**

## G. Attack Concepts

61. **What is an attack in cybersecurity, and how does it relate to a threat actor and vulnerability?**

62. **What is the difference between an attack vector and an attack surface?**

63. **What is an attack path, and why can an attacker require multiple steps to reach a valuable asset?**

64. **Why do attackers often begin by gathering information before attempting exploitation?**

65. **What is reconnaissance, and why is it important to both offensive and defensive security professionals?**

66. **What is exploitation at a conceptual level?**

67. **What does post-exploitation mean conceptually?**

68. **Why might gaining initial access not be the ultimate objective of an attacker?**

69. **What is lateral movement, and why is it significant in enterprise security?**

70. **Describe a hypothetical attack chain beginning with reconnaissance and ending with access to a sensitive internal system. Identify where security controls could interrupt the chain.**

## H. Attack Surface

71. **What is an attack surface?**

72. **How can an organization's attack surface include people, applications, devices, networks, cloud resources, APIs, and physical systems?**

73. **Why does an organization's attack surface tend to grow as more services are connected to the Internet?**

74. **How can unnecessary services and unused accounts increase attack surface?**

75. **Why is asset inventory important for attack-surface management?**

76. **What is external attack surface versus internal attack surface?**

77. **Why can third-party services become part of an organization's effective attack surface?**

78. **How can remote access technologies change an organization's attack surface?**

79. **Why should attack-surface reduction be considered a security control?**

80. **Design an attack-surface inventory for a company that has employees, laptops, servers, cloud services, websites, APIs, VPN access, and third-party SaaS platforms.**

## I. Security Controls

81. **What is a security control?**

82. **What is the difference between preventive, detective, corrective, deterrent, and compensating controls?**

83. **Give examples of preventive controls and explain what they prevent.**

84. **Give examples of detective controls and explain what they detect.**

85. **What are corrective controls, and why are they necessary even when preventive controls exist?**

86. **Why is security stronger when multiple independent controls protect the same asset?**

87. **What is defense in depth?**

88. **Why can relying on a single security control create a single point of failure?**

89. **How should an organization decide which security controls are appropriate for a particular asset or threat?**

90. **Design a defense-in-depth strategy for protecting a company's sensitive database from external compromise. Explain the purpose of each layer.**

## J. Authentication

91. **What is authentication, and what security question does authentication attempt to answer?**

92. **What is the difference between authentication and authorization?**

93. **What are the major categories of authentication factors?**

94. **Why is a username not normally considered an authentication factor by itself?**

95. **What makes a password a knowledge factor?**

96. **What makes a hardware security key a possession factor?**

97. **What is multi-factor authentication, and why does combining independent factors improve security?**

98. **Why does MFA reduce risk without making an account completely immune to compromise?**

99. **What security problems can occur when authentication systems are poorly designed?**

100. **Design a secure authentication system for an organization containing employees, administrators, contractors, and service accounts.**

## K. Authorization & Least Privilege

101. **What is authorization, and what question does it answer after authentication?**

102. **What is the principle of least privilege?**

103. **Why should users normally have only the permissions required for their responsibilities?**

104. **How can excessive privileges increase the impact of a compromised account?**

105. **Why are administrative accounts especially important to protect?**

106. **What is privilege escalation at a conceptual level?**

107. **What is the difference between horizontal and vertical privilege escalation?**

108. **Why should service accounts also follow least-privilege principles?**

109. **How can access reviews help organizations identify excessive privileges?**

110. **Design an access-control model for a company containing normal employees, managers, IT administrators, security analysts, and external contractors.**

## L. Cryptography Fundamentals

111. **What is cryptography, and what security problems can it help solve?**

112. **What is encryption, and what security property does it primarily provide?**

113. **What is decryption?**

114. **What is the conceptual difference between symmetric and asymmetric cryptography?**

115. **Why is symmetric encryption generally efficient for protecting large amounts of data?**

116. **Why is asymmetric cryptography useful for key exchange, authentication, and digital signatures?**

117. **What is a cryptographic key, and why is key management often as important as the cryptographic algorithm itself?**

118. **What is hashing, and how is it fundamentally different from encryption?**

119. **Why should cryptographic algorithms generally not be invented from scratch by application developers?**

120. **Explain how encryption, hashing, digital signatures, and key management can work together to provide confidentiality, integrity, and authenticity.**

## M. Digital Signatures & Integrity

121. **What is a digital signature, and what security properties does it provide?**

122. **How does a digital signature differ from encryption?**

123. **Why does a digital signature help establish message integrity?**

124. **How can a digital signature help establish authenticity?**

125. **Why does possession of a private signing key need to be carefully protected?**

126. **What is a certificate, and how does it relate to public-key cryptography?**

127. **What problem does a Public Key Infrastructure (PKI) attempt to solve?**

128. **Why does trust in a certificate authority matter to HTTPS?**

129. **What could happen if an attacker gains control of a private cryptographic key?**

130. **Explain conceptually how a browser can use certificates, public-key cryptography, and digital signatures to establish trust in an HTTPS connection.**

## N. Malware Fundamentals

131. **What is malware, and why is it considered a broad category rather than a single type of threat?**

132. **What is a virus, and how does it differ conceptually from other forms of malware?**

133. **What is a worm, and why can worms spread differently from traditional viruses?**

134. **What is a Trojan, and why is the term associated with malicious software disguised as something legitimate?**

135. **What is ransomware, and how can it affect confidentiality, integrity, and availability?**

136. **What is spyware, and what types of information might it attempt to collect?**

137. **What is a botnet, and why can compromised systems become valuable to an attacker even when the attacker does not directly use them?**

138. **Why is malware analysis important for both Red Team and Blue Team professionals?**

139. **What security controls can reduce the likelihood or impact of malware infections?**

140. **Describe a hypothetical malware infection from initial delivery through execution, persistence, impact, detection, containment, eradication, and recovery.**

## O. Security Mindset & Incident Thinking

141. **What is the difference between an event, an alert, an incident, and a security breach?**

142. **Why does a security event not automatically mean that an organization has suffered a security incident?**

143. **What is incident response, and why is preparation important before an incident occurs?**

144. **What are the broad phases of a typical incident-response lifecycle?**

145. **Why is containment different from eradication?**

146. **Why is recovery an important security activity rather than simply an IT operations task?**

147. **Why should organizations preserve evidence during security incidents?**

148. **Why is post-incident analysis important even after systems have been restored?**

149. **How can lessons learned from one security incident improve an organization's future security posture?**

150. **Imagine an employee's account has been compromised and the attacker has accessed internal resources. Walk through how you would think about the incident from detection through containment, investigation, eradication, recovery, and lessons learned.**

## The Core Security Model You Should Build

By the end of Level 1, I want you to stop seeing cybersecurity as a collection of tools and start seeing it as a chain:

```text
                     ASSET
                       │
                       ▼
                    THREAT
                       │
                       ▼
                 VULNERABILITY
                       │
                       ▼
                     ATTACK
                       │
                       ▼
                    IMPACT
                       │
          ┌────────────┴────────────┐
          ▼                         ▼
       CONTROL                    RISK
          │
    ┌─────┼─────┐
    ▼     ▼     ▼
 Prevent Detect Correct
    │     │     │
    └─────┼─────┘
          ▼
       RESPONSE
          │
          ▼
       RECOVERY
          │
          ▼
    LESSONS LEARNED
```

This model will appear again and again throughout our journey.

## The Three Questions Every Security Concept Should Trigger

Whenever we learn something, I want you to automatically ask:

### 1. What are we protecting?

```text
Data
Identity
Credentials
Systems
Applications
Infrastructure
Business processes
```

### 2. What can go wrong?

```text
Threat
    ↓
Vulnerability
    ↓
Attack
    ↓
Impact
```

### 3. What can we do about it?

```text
Prevent
Detect
Respond
Recover
```

This prevents us from falling into the trap of **tool-based learning**.

For example, don't learn:

> "Firewall = security."

Instead:

> **What asset is the firewall protecting?**

> **What threat are we concerned about?**

> **What traffic does the firewall control?**

> **What happens if the firewall rule is wrong?**

> **How could an attacker attempt to bypass the control?**

> **How would we detect that?**

> **What happens if the firewall itself fails?**

That is security thinking.

## How Level 1 Connects With Our Existing Subjects

This is especially important because we're deliberately **not studying subjects in isolation**.

### Networking + Security

```text
Networking
    ↓
Packets
    ↓
Protocols
    ↓
Communication
    ↓
Trust
    ↓
Network Attacks
    ↓
Network Defense
```

You'll eventually be able to understand things like:

```text
"Why is this port exposed?"
"Who can reach it?"
"What service is listening?"
"What happens if that service is compromised?"
"How would we detect the traffic?"
```

### Linux + Security

```text
Linux
 ↓
Users
 ↓
Groups
 ↓
Permissions
 ↓
Processes
 ↓
Services
 ↓
Logs
 ↓
Security
```

So instead of simply learning:

> `chmod`

you'll eventually think:

> **"Who should have access to this file, what happens if permissions are too broad, and how would I detect unauthorized modification?"**

### Python + Security

```text
Python
 ↓
Programming
 ↓
Automation
 ↓
Parsing
 ↓
Networking
 ↓
Security Tools
 ↓
Detection / Analysis
```

Python becomes a way to **solve security problems**, rather than merely another programming language.

### Web + Security

We've already built:

```text
Web
 ↓
HTTP
 ↓
Sessions
 ↓
Authentication
 ↓
Authorization
 ↓
Trust Boundaries
 ↓
Attack Surface
```

Now Security Fundamentals gives us:

```text
Asset
 ↓
Threat
 ↓
Vulnerability
 ↓
Risk
 ↓
Control
 ↓
Detection
 ↓
Response
```

Combine them:

```text
WEB APPLICATION
       │
       ▼
  ASSET / DATA
       │
       ▼
     THREAT
       │
       ▼
 VULNERABILITY
       │
       ▼
     ATTACK
       │
       ▼
     IMPACT
       │
       ├───────────────┐
       ▼               ▼
  RED TEAM          BLUE TEAM
       │               │
   Exploit/Test     Detect/Respond
       │               │
       └───────┬───────┘
               ▼
          IMPROVEMENT
```

This is exactly where our **Red Team > Blue Team > Cloud Security > AppSec** career priority begins to converge.

## The Level 1 Security Mindset

If you encounter a new technology tomorrow, don't ask only:

> **"How does this technology work?"**

Ask:

```text
What assets does it contain?

Who should access those assets?

Who could attack it?

What are the likely threats?

What vulnerabilities could exist?

What is the attack surface?

What happens if it is compromised?

What controls protect it?

How would we detect compromise?

How would we respond?

How would we recover?

How would we prevent the same incident again?
```

That mindset is more valuable than memorizing hundreds of security tools.

## Where Level 1 Takes Us

Our progression now becomes:

```text
NETWORKING
     │
     ├──────────────┐
     ▼              │
   LINUX            │
     │              │
     ├──────┐       │
     ▼      ▼       │
  PYTHON   WINDOWS  │
     │      │       │
     └──┬───┘       │
        ▼           │
 WEB FUNDAMENTALS ──┘
        │
        ▼
SECURITY FUNDAMENTALS
        │
        ├── CIA
        ├── Assets
        ├── Threats
        ├── Vulnerabilities
        ├── Risk
        ├── Attack Surface
        ├── Security Controls
        ├── Authentication
        ├── Authorization
        ├── Cryptography
        ├── Malware
        └── Incident Thinking
                │
                ▼
       SECURITY FUNDAMENTALS L2
                │
                ▼
       SECURITY FUNDAMENTALS L3
                │
        ┌───────┴────────┐
        ▼                ▼
 Network Security    Red Team / VAPT
        │                │
        ▼                ▼
   Blue Team / SOC   Pentesting
        │                │
        └───────┬────────┘
                ▼
        Advanced Security
```

The key outcome of **Security Fundamentals Level 1** is therefore not that you can define 150 terms.

It is that when you encounter **any system**, you can begin thinking:

> **What are we protecting? → Who threatens it? → What can go wrong? → What is the risk? → What controls exist? → How can we detect failure? → How do we respond and recover?**

That becomes the **security foundation underneath every subject that follows.**
