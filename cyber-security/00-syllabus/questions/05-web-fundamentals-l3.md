# Web Fundamentals — Level 3

**Web Fundamentals Level 3** is where Web Fundamentals becomes directly connected to our main career path:

> **Red Team / Pentesting → Blue Team / SOC → Cloud Security → AppSec**

At Level 1, we learned **how the web works**.

At Level 2, we learned **how modern web applications are constructed and how components communicate**.

At Level 3, we ask:

> **How can these components fail?**
> **How can trust boundaries be abused?**
> **How do attackers reason about the application?**
> **How do defenders prevent, detect, and recover from these failures?**

This is still a **fundamentals/understanding level**, not yet a dedicated exploit-development course. The goal is to build the mental model we'll later use in **Red Team/VAPT and AppSec**.

### Advanced Security-Oriented Question Bank

**150 questions**

## A. Web Attack Surface & Reconnaissance

1. **What constitutes the attack surface of a modern web application, and why is it larger than simply the homepage?**

2. **How would you identify the different components that make up a web application's externally visible attack surface?**

3. **What is the difference between an asset, endpoint, parameter, functionality, and trust boundary during web reconnaissance?**

4. **Why should reconnaissance begin with understanding the application's normal architecture rather than immediately searching for vulnerabilities?**

5. **What information can DNS reveal about an organization's web infrastructure?**

6. **How can subdomains expand an organization's web attack surface?**

7. **Why might development, staging, testing, administrative, and production environments have different security implications?**

8. **What is service fingerprinting in the context of web reconnaissance?**

9. **What types of information can HTTP responses, headers, certificates, URLs, and application behavior reveal about a web application?**

10. **How would you build an authorized web attack-surface inventory containing domains, subdomains, technologies, endpoints, APIs, authentication mechanisms, and exposed functionality?**

## B. Threat Modeling Web Applications

11. **What is threat modeling, and why should it happen before vulnerability testing?**

12. **What is an asset in a web application's threat model?**

13. **What is a threat actor, and why does the assumed threat actor influence security decisions?**

14. **What is a trust boundary, and why is it one of the most important concepts in web security?**

15. **How would you identify trust boundaries between a browser, CDN, reverse proxy, application server, internal services, and database?**

16. **What is an abuse case, and how is it different from a normal use case?**

17. **Why should security requirements be derived from what an application is supposed to protect?**

18. **How can data-flow diagrams help identify web application security weaknesses?**

19. **Why are authentication, authorization, sensitive data, administrative functions, and business-critical operations often high-value parts of a threat model?**

20. **Create a threat model for an online banking application. Identify assets, actors, trust boundaries, sensitive operations, likely abuse cases, and security controls.**

## C. Input & Trust Boundaries

21. **Why should data received from a browser generally be considered untrusted by the server?**

22. **What is the difference between client-side validation and server-side validation?**

23. **Why can client-side validation improve usability without being considered a reliable security boundary?**

24. **What categories of input can a web application receive from a client?**

25. **Why should HTTP headers, cookies, query parameters, form fields, JSON bodies, and uploaded files all be treated carefully?**

26. **What is input validation, and what should validation attempt to establish?**

27. **What is allowlisting, and why can it be safer than trying to block every possible malicious input?**

28. **What is canonicalization, and why can inconsistent representations of the same input create security problems?**

29. **Why should validation rules reflect the intended data type and business requirement rather than simply checking whether input "looks safe"?**

30. **Design an input-validation strategy for a web application that receives usernames, email addresses, numeric identifiers, URLs, and uploaded files.**

## D. Injection Concepts

31. **What is injection as a general vulnerability concept?**

32. **Why does injection occur when data is incorrectly interpreted as instructions by another component?**

33. **What is the conceptual difference between data and code in the context of injection vulnerabilities?**

34. **How can injection vulnerabilities arise when an application constructs commands, queries, templates, or expressions dynamically?**

35. **Why are parameterized interfaces generally safer than dynamically constructing interpreter commands?**

36. **What is SQL injection conceptually, and why does it occur?**

37. **What is command injection conceptually, and why is it especially dangerous on server-side applications?**

38. **What is template injection, and why can server-side template processing create security risks?**

39. **Why should developers avoid relying on blacklists as the primary defense against injection?**

40. **Design a secure server-side data-processing architecture that prevents untrusted input from becoming executable instructions across database, operating-system, and template boundaries.**

## E. Cross-Site Scripting — XSS

41. **What is Cross-Site Scripting (XSS), and what fundamental trust problem does it represent?**

42. **What is the difference between reflected, stored, and DOM-based XSS?**

43. **Why is stored XSS potentially more persistent than reflected XSS?**

44. **What role does the browser's interpretation of HTML and JavaScript play in XSS?**

45. **Why is output encoding context-dependent?**

46. **Why is encoding HTML content different from safely handling data inserted into a JavaScript, URL, CSS, or HTML-attribute context?**

47. **What is the difference between input validation and output encoding as XSS defenses?**

48. **Why can client-side filtering fail to provide reliable XSS protection?**

49. **How does Content Security Policy contribute to reducing the impact of XSS?**

50. **Design a layered XSS defense strategy covering secure coding, output handling, browser protections, Content Security Policy, and security testing.**

## F. SQL Injection & Database Security

51. **Why does SQL injection occur when application data becomes part of a database query's structure?**

52. **Why are parameterized queries an important defense against SQL injection?**

53. **What is the difference between application-level validation and database-level protection?**

54. **Why should a database account used by a web application follow the principle of least privilege?**

55. **Why should an application generally not connect to a database using an administrative account?**

56. **What is the potential security impact of SQL injection beyond simply reading database records?**

57. **Why can database error messages reveal useful information to attackers?**

58. **How can excessive database privileges increase the impact of an application vulnerability?**

59. **What role can database logging and monitoring play in detecting suspicious application behavior?**

60. **Design a secure database-access architecture for a web application, including query handling, application privileges, error handling, logging, and monitoring.**

## G. Authentication Security

61. **What makes authentication a security boundary rather than simply a login page?**

62. **What common assumptions about passwords can lead to weak authentication systems?**

63. **Why should authentication mechanisms resist automated guessing and credential abuse?**

64. **What is credential stuffing, and why is it different from traditional password guessing?**

65. **Why does password reuse increase the risk of compromise across multiple services?**

66. **How can MFA reduce the impact of stolen passwords?**

67. **Why should authentication endpoints receive particularly careful rate limiting and monitoring?**

68. **What security considerations should surround account recovery mechanisms?**

69. **Why can changing a password be insufficient if existing authenticated sessions remain active indefinitely?**

70. **Design a secure authentication lifecycle from account creation through login, MFA, session establishment, password change, recovery, logout, and account termination.**

## H. Session Security

71. **What makes a session identifier sensitive?**

72. **What properties should a secure session identifier have?**

73. **What is session fixation, and how can an application prevent it?**

74. **What is session hijacking conceptually?**

75. **How can HTTPS reduce the risk of session interception while not eliminating every session-related threat?**

76. **Why are cookie attributes such as `Secure`, `HttpOnly`, and `SameSite` important to session security?**

77. **Why should applications invalidate sessions after sensitive security events?**

78. **What is session rotation, and when might it be appropriate?**

79. **Why can long-lived sessions create additional security risk?**

80. **Design a secure session-management architecture for a financial web application. Explain session generation, storage, rotation, expiration, invalidation, and cookie protections.**

## I. Authorization & Access Control

81. **Why is broken authorization potentially more dangerous than a simple authentication failure?**

82. **What is horizontal privilege escalation?**

83. **What is vertical privilege escalation?**

84. **What is an object-level authorization decision?**

85. **Why should authorization be checked on every sensitive server-side operation rather than only when a user initially logs in?**

86. **Why is hiding administrative functionality from the frontend not an authorization control?**

87. **Why should object identifiers not be treated as proof that the requesting user is authorized to access the object?**

88. **How can centralized authorization logic reduce inconsistent access-control behavior?**

89. **What is the principle of least privilege in application authorization?**

90. **Design an authorization system for a multi-tenant SaaS application where users must never access another organization's data. Identify the critical authorization boundaries.**

## J. CSRF

91. **What security assumption makes Cross-Site Request Forgery possible in many traditional web applications?**

92. **Why does automatic browser handling of authentication state matter for CSRF?**

93. **Why are state-changing operations the primary concern for CSRF?**

94. **How do CSRF tokens establish additional evidence that a request originated from the legitimate application flow?**

95. **How does SameSite cookie behavior affect CSRF risk?**

96. **Why should applications not assume that simply changing a request from POST to another method automatically solves CSRF?**

97. **Why can APIs using bearer tokens in an Authorization header have a different CSRF risk profile from cookie-authenticated applications?**

98. **What security considerations should be applied when designing cross-origin integrations?**

99. **How can a security tester determine whether a state-changing action has appropriate CSRF protections in an authorized assessment?**

100. **Design a defense-in-depth strategy against CSRF for a web application with browser sessions and several state-changing operations.**

## K. CORS & Cross-Origin Security

101. **Why is CORS a browser security mechanism rather than a general-purpose server authorization mechanism?**

102. **What security assumptions are made when an application allows another origin to read API responses?**

103. **Why is an unrestricted cross-origin policy potentially dangerous when sensitive data is accessible?**

104. **Why should credentialed cross-origin requests receive special attention?**

105. **What is the security significance of dynamically reflecting an arbitrary Origin value in CORS responses?**

106. **Why should an application explicitly define which origins require cross-origin access?**

107. **How can overly broad CORS policies expose information even when authentication itself is correctly implemented?**

108. **Why is CORS not a substitute for authentication or authorization?**

109. **How would you review a CORS configuration during an authorized security assessment?**

110. **Design a secure CORS policy for a frontend hosted on one origin that communicates with an API hosted on another origin.**

## L. Browser Security

111. **What security boundaries does the browser enforce between websites?**

112. **Why is the Same-Origin Policy fundamental to browser security?**

113. **What is clickjacking, and what browser behavior makes it possible?**

114. **How can framing controls help mitigate clickjacking?**

115. **What is Content Security Policy, and what problem is it designed to address?**

116. **What is the purpose of security-related response headers such as CSP, HSTS, and frame-related controls?**

117. **Why should browser security headers be considered defense-in-depth rather than a replacement for secure server-side code?**

118. **What is HSTS, and why can it improve HTTPS security?**

119. **Why can browser security controls reduce the impact of a vulnerability without necessarily removing its underlying cause?**

120. **Design a browser-security baseline for a sensitive web application. Which browser-enforced controls would you consider, and what threats would each address?**

## M. File Upload Security

121. **Why can file-upload functionality significantly expand a web application's attack surface?**

122. **Why is checking only a file's extension insufficient for secure upload handling?**

123. **What types of properties should a secure file-upload system validate?**

124. **Why should uploaded files generally not be stored directly in a web-accessible directory without additional controls?**

125. **Why can the location and permissions of uploaded files matter as much as their contents?**

126. **Why should applications limit file size and resource consumption during uploads?**

127. **What security risks can arise when uploaded files are later processed by image, document, archive, or media libraries?**

128. **Why should uploaded files be treated as untrusted data even when they appear to have safe filenames or extensions?**

129. **How could antivirus or malware-scanning systems fit into a secure file-upload architecture?**

130. **Design a secure file-upload pipeline from browser submission through validation, storage, scanning, access control, and eventual retrieval.**

## N. Server-Side Request Behavior

131. **What is server-side request behavior, and why can it become a security concern?**

132. **Why is a server that can make outbound network requests different from a browser making requests on behalf of a user?**

133. **What is Server-Side Request Forgery (SSRF) conceptually?**

134. **Why can SSRF become especially serious in cloud environments?**

135. **Why should applications restrict where server-side components are allowed to make outbound requests when such requests are not required universally?**

136. **Why is validating a URL more complicated than checking whether a string begins with `https://`?**

137. **Why can redirects complicate server-side URL validation?**

138. **What role can network segmentation play in reducing the impact of SSRF-like weaknesses?**

139. **What logging and monitoring could help detect suspicious server-side outbound requests?**

140. **Design a secure architecture for a web application that legitimately needs to retrieve resources from external URLs while minimizing the risks associated with server-side outbound requests.**

## O. Business Logic Security

This is particularly important for **real pentesting**.

141. **What is a business-logic vulnerability, and why might traditional vulnerability scanners miss it?**

142. **Why can an application behave exactly according to its programmed rules and still have a security flaw?**

143. **What is a workflow in a web application?**

144. **Why should security testing consider sequences of actions rather than isolated requests?**

145. **How can assumptions about the order of operations create security weaknesses?**

146. **Why should applications independently enforce important business rules on the server?**

147. **What kinds of business operations deserve particularly careful authorization and validation?**

148. **How could an attacker abuse legitimate functionality in an unintended sequence without exploiting a traditional software vulnerability?**

149. **How would you approach identifying business-logic weaknesses during an authorized web application assessment?**

150. **Design a threat model for an e-commerce checkout workflow. Identify the important states, trust boundaries, business rules, security assumptions, abuse cases, and controls that should exist from product selection through payment and order completion.**

## The Level 3 Mental Model

At Level 1:

```text
Browser
   ↓
HTTP
   ↓
Server
   ↓
Application
   ↓
Database
```

At Level 2:

```text
Browser
 ↓
JavaScript
 ↓
HTTP / APIs
 ↓
Authentication
 ↓
Sessions
 ↓
Authorization
 ↓
Business Logic
 ↓
Database
```

Now at **Level 3**, we add the security perspective:

```text
                         USER
                           │
                           ▼
                    ┌──────────────┐
                    │    BROWSER   │
                    │              │
                    │ HTML / JS    │
                    │ Cookies      │
                    │ DOM          │
                    └──────┬───────┘
                           │
                    UNTRUSTED INPUT
                           │
                           ▼
                 ┌────────────────────┐
                 │ WEB / API LAYER    │
                 │                    │
                 │ Validation         │
                 │ Authentication     │
                 │ Authorization      │
                 │ Session Management │
                 └─────────┬──────────┘
                           │
                     TRUST BOUNDARY
                           │
                           ▼
                 ┌────────────────────┐
                 │ BUSINESS LOGIC     │
                 │                    │
                 │ Workflows          │
                 │ Rules              │
                 │ Transactions       │
                 └─────────┬──────────┘
                           │
                     TRUST BOUNDARY
                           │
                           ▼
                 ┌────────────────────┐
                 │ INTERNAL SERVICES  │
                 │                    │
                 │ APIs               │
                 │ Queues             │
                 │ Storage            │
                 └─────────┬──────────┘
                           │
                           ▼
                 ┌────────────────────┐
                 │ DATA LAYER         │
                 │                    │
                 │ Database           │
                 │ Files              │
                 │ Secrets            │
                 └────────────────────┘
```

And around the whole thing:

```text
                    SECURITY CONTROLS

       ┌─────────────────────────────────────────┐
       │                                         │
       │  Authentication                         │
       │  Authorization                           │
       │  Input Validation                        │
       │  Output Encoding                         │
       │  Session Security                        │
       │  CSRF Protection                         │
       │  CORS Policy                             │
       │  CSP                                     │
       │  TLS                                     │
       │  Rate Limiting                            │
       │  Logging                                 │
       │  Monitoring                              │
       │  Least Privilege                         │
       │  Network Segmentation                    │
       │                                         │
       └─────────────────────────────────────────┘
```

## The Attacker's Mental Model

By the end of Level 3, we want to be able to look at a web application like this:

```text
                         APPLICATION
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
        INPUT              IDENTITY            DATA
          │                   │                   │
      Parameters          Authentication      Database
      Headers             Sessions            Files
      JSON                Authorization       APIs
      Files               Roles               Objects
          │                   │                   │
          └───────────────────┼───────────────────┘
                              │
                         BUSINESS LOGIC
                              │
                         TRUST BOUNDARIES
```

And ask:

```text
What can I control?

What does the application trust?

Where is validation performed?

Where is authorization performed?

Can the same data reach another interpreter?

Can one user access another user's object?

Can a low-privileged user reach privileged functionality?

Can state be manipulated?

Can workflows be abused?

Can the browser be tricked?

Can the server be induced to communicate with something it shouldn't?

Can sensitive information cross an unintended boundary?

What happens when assumptions between components disagree?
```

That is the beginning of **offensive security reasoning**.

## The Defender's Mental Model

But because our second priority is **Blue Team / SOC**, we also need the other side:

```text
                     WEB REQUEST
                          │
                          ▼
                     LOGGING
                          │
              ┌───────────┼───────────┐
              ↓           ↓           ↓
         Authentication  API       Application
             Logs        Logs          Logs
              │           │           │
              └───────────┼───────────┘
                          ↓
                     CORRELATION
                          ↓
                      DETECTION
                          ↓
                       ALERT
                          ↓
                    INVESTIGATION
                          ↓
                      RESPONSE
```

So for every web-security concept, we should eventually be able to ask **four questions**:

### 1. How does it work?

### 2. How can it fail?

### 3. How can it be exploited or abused?

### 4. How can we prevent, detect, and respond to it?

That four-question framework will become extremely useful throughout our cybersecurity journey.

## The Vulnerability Families We Should Recognize

By the end of Web Fundamentals Level 3, you should have a conceptual map like:

```text
WEB SECURITY
│
├── Authentication
│   ├── Weak authentication
│   ├── Credential abuse
│   ├── Session weaknesses
│   └── Recovery weaknesses
│
├── Authorization
│   ├── Horizontal access issues
│   ├── Vertical access issues
│   └── Object-level authorization
│
├── Injection
│   ├── SQL
│   ├── Command
│   ├── Template
│   └── Other interpreters
│
├── Browser Security
│   ├── XSS
│   ├── CSRF
│   ├── Clickjacking
│   ├── CORS
│   └── CSP
│
├── Server-Side
│   ├── SSRF
│   ├── File handling
│   ├── Deserialization
│   └── Configuration
│
├── Application Logic
│   ├── Workflow abuse
│   ├── Race conditions
│   ├── Business logic
│   └── State manipulation
│
└── Infrastructure
    ├── TLS
    ├── Web server
    ├── Reverse proxy
    ├── CDN
    └── Cloud architecture
```

We are **not yet trying to master exploitation of every item**.

We're building the map first.

That matters because later, when we study **Security Fundamentals → Basic Red Team/VAPT → AppSec**, you won't be memorizing vulnerability names. You'll understand **why the vulnerability exists**.

## Connecting Level 3 to Our Other Subjects

This is where our "learn multiple subjects together" strategy becomes powerful.

### Networking + Web Security

```text
DNS
 ↓
IP
 ↓
TCP/QUIC
 ↓
TLS
 ↓
HTTP
 ↓
Web Application
 ↓
Attack Surface
```

### Linux + Web Security

```text
Linux
 ↓
Processes
 ↓
Sockets
 ↓
Web Server
 ↓
Application
 ↓
Files
 ↓
Permissions
```

### Python + Web Security

```text
Python
 ↓
HTTP
 ↓
APIs
 ↓
Parsing
 ↓
Automation
 ↓
Assessment / Detection
```

### Web + Red Team

```text
Recon
 ↓
Attack Surface
 ↓
Endpoints
 ↓
Parameters
 ↓
Authentication
 ↓
Authorization
 ↓
Business Logic
 ↓
Security Testing
```

### Web + Blue Team

```text
HTTP Traffic
 ↓
Application Logs
 ↓
Authentication Events
 ↓
API Activity
 ↓
Correlation
 ↓
Detection
 ↓
Investigation
```

## The Ultimate Level 3 Test

If we give you:

```text
https://shop.example.com
```

I don't want your first thought to be:

> "Let's scan it."

Instead:

```text
1. What is the architecture?

2. What assets exist?

3. What domains/subdomains exist?

4. What technologies are exposed?

5. What endpoints exist?

6. What APIs exist?

7. What authentication mechanisms exist?

8. How are sessions maintained?

9. How is authorization enforced?

10. What data is user-controlled?

11. Where are the trust boundaries?

12. What business-critical workflows exist?

13. What security assumptions does the application make?

14. What security controls protect those assumptions?

15. What evidence would demonstrate that a control works?

16. What evidence would demonstrate that it fails?

17. How would exploitation affect confidentiality,
    integrity, or availability?

18. How could defenders detect the activity?

19. How could the vulnerability be remediated?

20. How would we verify the remediation?
```

That is the mindset we want.

## Web Fundamentals — Complete Three-Level Journey

|                 | Level 1            | Level 2             | Level 3                 |
| --------------- | ------------------ | ------------------- | ----------------------- |
| Main goal       | Understand the Web | Understand Web Apps | Understand Web Security |
| Internet        | ✓                  | ✓                   | ✓                       |
| DNS             | ✓                  | Deep                | Security                |
| HTTP            | Basic              | Deep                | Security analysis       |
| HTTPS/TLS       | Basic              | Deep                | Security implications   |
| HTML            | Basic              | Intermediate        | Security context        |
| CSS             | Basic              | Intermediate        | Security context        |
| JavaScript      | Basic              | Intermediate        | Browser security        |
| Cookies         | Basic              | Deep                | Security                |
| Sessions        | Basic              | Deep                | Security                |
| Authentication  | Basic              | Deep                | Security                |
| Authorization   | Basic              | Deep                | Security                |
| APIs            | Basic              | Deep                | API security            |
| Browser         | Basic              | Deep                | Browser security        |
| Databases       | Basic              | Architecture        | Security                |
| Business Logic  | —                  | Introduction        | Deep                    |
| Attack Surface  | Introduction       | Mapping             | Analysis                |
| Threat Modeling | —                  | Introduction        | Applied                 |
| XSS             | —                  | Concept             | Security analysis       |
| SQLi            | —                  | Concept             | Security analysis       |
| CSRF            | Concept            | Deep                | Security analysis       |
| CORS            | Concept            | Deep                | Security analysis       |
| SSRF            | —                  | Introduction        | Security analysis       |
| File Upload     | —                  | Introduction        | Security analysis       |

The final transition is:

> **Web Level 1:** *I understand the web.*

> **Web Level 2:** *I understand how web applications work.*

> **Web Level 3:** **I can reason about the security of web applications.**

And that gives us the exact foundation we want before moving deeper into **Security Fundamentals**, followed by **Network Security & Analysis**, **Basic Red Team/VAPT**, **Basic Blue Team/SOC**, and eventually dedicated **AppSec/Web Pentesting** work.
