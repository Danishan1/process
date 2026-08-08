
## Web Fundamentals — Level 2

**Web Fundamentals Level 2** is where we move from *“I understand how the web works”* to:

> **“I understand how a real web application is built, how its components communicate, where trust boundaries exist, and how data moves through the entire system.”**

This level is extremely important for our **Red Team / Pentesting** priority and will become the foundation for **AppSec** later.

Our progression:

```text
Web Level 1
    ↓
How the Internet + Web works
    ↓
Web Level 2
    ↓
How real web applications work internally
    ↓
Web Level 3
    ↓
How those components can fail, be abused, detected and secured
```

For Level 2, we'll go much deeper into:

* HTTP internals
* Request/response lifecycle
* HTTP/1.1, HTTP/2 and HTTP/3 concepts
* Cookies and sessions
* Authentication
* Authorization
* Browser security
* Same-Origin Policy
* CORS
* CSRF
* JavaScript
* DOM
* Fetch/XHR
* REST APIs
* JSON
* GraphQL concepts
* WebSockets
* Reverse proxies
* Load balancers
* CDNs
* Web servers
* Application servers
* Databases
* Caching
* File uploads
* Authentication architecture
* JWT
* OAuth/OIDC concepts
* Microservices
* Containers
* Web architecture
* Trust boundaries
* Attack-surface mapping
* Security-oriented thinking

The goal is **understanding**, not yet turning this into a dedicated vulnerability-exploitation course. That comes later.


### Intermediate Question Bank

**150 questions**

## A. Deep HTTP

1. **Walk through the complete lifecycle of an HTTP request from the browser generating the request to the server producing the response.**

2. **What exactly makes up an HTTP request, and how are the request line, headers, and body related?**

3. **What exactly makes up an HTTP response, and how are the status line, headers, and body related?**

4. **What is the difference between HTTP message syntax and the semantics of an HTTP request?**

5. **Why is the HTTP method more than simply a label attached to a request?**

6. **How does the server determine which application functionality should handle a particular URL and HTTP method?**

7. **What is routing in a web application?**

8. **What is endpoint routing, and why is an endpoint an important concept when mapping a web application's attack surface?**

9. **How can two URLs pointing to the same server reach completely different application functionality?**

10. **Why is understanding HTTP routing important for both web developers and penetration testers?**

## B. HTTP Headers — Deeper Understanding

11. **How are request headers different from response headers in terms of who controls them and what information they communicate?**

12. **What role does the `Host` header play when one server hosts multiple websites?**

13. **What is content negotiation, and how do headers such as `Accept` and `Content-Type` participate in it?**

14. **What is the difference between the media type of a request body and the media type of a response body?**

15. **How does the `Authorization` header commonly participate in API authentication?**

16. **What is the purpose of the `Referer` header, and why should applications not blindly trust it for security decisions?**

17. **What information can the `User-Agent` header provide, and why is it not a reliable identity mechanism?**

18. **How can HTTP headers reveal information about the technologies or architecture behind a web application?**

19. **Why should security-sensitive decisions generally not depend solely on client-controlled HTTP headers?**

20. **During an authorized assessment, how would you systematically analyze the security significance of the request and response headers you observe?**

## C. HTTP Versions & Transport

21. **What are the major conceptual differences between HTTP/1.1, HTTP/2, and HTTP/3?**

22. **Why was HTTP/2 introduced, and what limitations of HTTP/1.1 was it intended to improve?**

23. **What is multiplexing in HTTP/2, and why does it improve communication efficiency?**

24. **What is HTTP/3's relationship with QUIC and UDP?**

25. **Why does the transport protocol matter when analyzing a web application's network behavior?**

26. **How can multiple HTTP requests share an underlying connection?**

27. **Why can performance optimizations at the HTTP layer also introduce additional complexity for security tools?**

28. **Why might a network-security tool see different characteristics when analyzing HTTP/1.1 versus HTTP/2 or HTTP/3 traffic?**

29. **Why is protocol awareness important when developing web-security tooling?**

30. **How would you explain the relationship between DNS, TCP/QUIC, TLS, and HTTP in a modern HTTPS connection?**

## D. Cookies

31. **What is a cookie technically, and how does a browser decide when to send it?**

32. **How do cookie domain and path attributes influence where a cookie is sent?**

33. **What is the difference between a host-only cookie and a cookie applicable to a wider domain?**

34. **Why is cookie scope important from a security perspective?**

35. **How does the `Secure` attribute affect cookie transmission?**

36. **How does `HttpOnly` change JavaScript's ability to access a cookie?**

37. **How does `SameSite` influence cross-site cookie transmission?**

38. **What is the difference between `SameSite=Strict`, `Lax`, and `None` conceptually?**

39. **Why should session cookies generally have restrictive attributes?**

40. **Design a secure conceptual cookie policy for a web application's authentication session and explain the reasoning behind every important attribute.**

## E. Sessions

41. **Why does a web application need session management when HTTP itself is stateless?**

42. **What is a session identifier?**

43. **What should make a session identifier difficult to guess?**

44. **Why should session identifiers be generated using strong randomness?**

45. **What is session fixation at a conceptual level?**

46. **Why should a session identifier generally change after a significant authentication event such as login?**

47. **What is session expiration, and why is it important?**

48. **What is the difference between idle timeout and absolute session lifetime?**

49. **What should happen to a user's session when they explicitly log out?**

50. **What security problems could arise if an application does not properly invalidate sessions after logout or account recovery?**

## F. Authentication

51. **What is authentication, and how is it different from identification?**

52. **What is the difference between authentication and authorization?**

53. **What are the major factors commonly used in authentication?**

54. **What is multi-factor authentication, and why is it stronger than password-only authentication?**

55. **Why does MFA not automatically make every part of an application secure?**

56. **What properties should a secure password authentication system have?**

57. **Why should applications store password verifiers rather than plaintext passwords?**

58. **What is password hashing, and how is it different from encryption?**

59. **Why are purpose-built password-hashing algorithms preferable to ordinary general-purpose hashes for password storage?**

60. **What factors should developers consider when designing a password-reset mechanism?**

## G. Authentication Flows

61. **Walk through a typical username/password login flow from browser submission to authenticated session.**

62. **What security decisions occur on the server after a user submits credentials?**

63. **Why should authentication errors avoid unnecessarily revealing whether a particular account exists?**

64. **What is account lockout, and what security and usability trade-offs does it introduce?**

65. **What is rate limiting in an authentication system?**

66. **Why can password-reset functionality become a critical part of an application's authentication security?**

67. **What information should a password-reset token contain conceptually, and what properties should such a token have?**

68. **Why should password-reset tokens expire and generally be single-use?**

69. **What happens conceptually when a user changes their password while already logged in?**

70. **Design a secure conceptual authentication architecture covering login, logout, password reset, MFA, session management, and account recovery.**

## H. Authorization

71. **What is authorization, and why must it be enforced independently of authentication?**

72. **What is role-based access control?**

73. **What is attribute-based access control?**

74. **What is the principle of least privilege in a web application?**

75. **Why should authorization decisions be made on the server rather than relying on frontend controls?**

76. **What is horizontal access control?**

77. **What is vertical access control?**

78. **Why can changing a URL or object identifier reveal authorization weaknesses in poorly designed applications?**

79. **Why should every sensitive server-side operation perform an appropriate authorization check?**

80. **Design an authorization model for an application containing normal users, moderators, administrators, and service accounts. Explain where authorization decisions should occur.**

## I. Same-Origin Policy

81. **What is the browser's Same-Origin Policy?**

82. **What components determine a web origin?**

83. **Why are scheme, host, and port important when determining whether two resources share an origin?**

84. **Why does the browser restrict scripts from freely interacting with resources belonging to other origins?**

85. **What security problem would arise if every website could freely read sensitive data from every other website?**

86. **How does the Same-Origin Policy protect users even when they visit untrusted websites?**

87. **What types of browser interactions are affected by origin boundaries?**

88. **Why is the Same-Origin Policy one of the foundational security mechanisms of the modern web?**

89. **How does the Same-Origin Policy differ from the concept of a domain?**

90. **Explain why understanding origins is essential before learning CORS, CSRF, and browser-based attacks.**

## J. CORS

91. **What is Cross-Origin Resource Sharing (CORS), and why does it exist?**

92. **What problem is CORS solving in relation to the Same-Origin Policy?**

93. **What is the purpose of the `Access-Control-Allow-Origin` response header?**

94. **What is a CORS preflight request?**

95. **Why might a browser send an OPTIONS request before the actual cross-origin request?**

96. **What is the purpose of `Access-Control-Allow-Methods`?**

97. **What is the purpose of `Access-Control-Allow-Headers`?**

98. **Why does allowing credentials in cross-origin requests require additional care?**

99. **Why is an overly permissive CORS configuration potentially dangerous?**

100. **Explain how you would evaluate whether a CORS policy is appropriate for a web application's legitimate cross-origin requirements.**

## K. CSRF

101. **What is Cross-Site Request Forgery conceptually?**

102. **Why can browser-managed authentication state create conditions where CSRF becomes possible?**

103. **Why does CSRF primarily concern state-changing actions?**

104. **What is a CSRF token?**

105. **How does a CSRF token help a server distinguish legitimate application requests from forged requests?**

106. **How can the `SameSite` cookie attribute help mitigate CSRF?**

107. **Why should developers not assume that SameSite alone is always sufficient for every application architecture?**

108. **Why does requiring a custom header or other application-specific proof sometimes provide an additional CSRF defense?**

109. **Why is CSRF conceptually different from XSS even though both involve interactions with a user's browser?**

110. **Design a layered CSRF-defense strategy for a state-changing web application and explain why multiple controls may be useful.**

## L. JavaScript & Browser Execution

111. **What happens when a browser encounters JavaScript while processing an HTML document?**

112. **What is the DOM, and how does JavaScript interact with it?**

113. **What is the difference between modifying the DOM and modifying the original HTML source?**

114. **What are browser events, and how can JavaScript respond to them?**

115. **What is event-driven programming in the browser?**

116. **What is asynchronous JavaScript, and why is it essential for modern web applications?**

117. **What are promises in JavaScript at a conceptual level?**

118. **What is the purpose of `async` and `await`?**

119. **Why does asynchronous behavior matter when analyzing web applications from a security perspective?**

120. **How could a security analyst use browser developer tools to understand JavaScript-driven application behavior?**

## M. Fetch, XHR & Browser-to-Server Communication

121. **What is XMLHttpRequest (XHR), and why was it historically important for web applications?**

122. **What is the Fetch API, and how does it enable JavaScript to communicate with servers?**

123. **How does a JavaScript `fetch()` request differ conceptually from a traditional browser navigation?**

124. **Why do modern applications frequently communicate with backend APIs without reloading the entire page?**

125. **What information can you observe in the browser's Network panel when an application makes an API request?**

126. **How can you identify which JavaScript action caused a particular HTTP request?**

127. **Why is understanding frontend-generated API requests important when mapping a web application's attack surface?**

128. **How can browser developer tools help distinguish static resources from API requests?**

129. **What security-sensitive information might accidentally be exposed through frontend JavaScript or API responses?**

130. **During an authorized assessment, how would you systematically map the browser's communication with a web application's backend?**

## N. REST APIs

131. **What is a REST-style API, and what does it mean to expose resources through HTTP?**

132. **What is the difference between an API endpoint and a web page?**

133. **Why might a web application have separate frontend and API endpoints?**

134. **What does it mean for an API to be stateless?**

135. **How are JSON objects commonly used in API requests and responses?**

136. **Why should API input be validated independently of frontend validation?**

137. **Why must API authorization be enforced even when the API is only intended to be called by the application's frontend?**

138. **What is API versioning, and why might an organization maintain multiple API versions?**

139. **Why can undocumented or deprecated API endpoints become part of an application's security attack surface?**

140. **Design an API inventory for a hypothetical application. What information would you record for each endpoint?**

## O. JWT

141. **What is a JSON Web Token (JWT), and what problem is it designed to solve?**

142. **What are the conceptual components of a JWT?**

143. **What is the difference between encoding and cryptographic signing in the context of JWTs?**

144. **Why should developers never assume that simply decoding a JWT means the information inside it is trustworthy?**

145. **What is the purpose of a JWT signature?**

146. **What kinds of information might appropriately appear in JWT claims?**

147. **Why should sensitive information generally not be placed in a JWT merely because it is signed?**

148. **What are token expiration and token audience, and why can they matter for API security?**

149. **What are the trade-offs between server-side sessions and stateless token-based authentication?**

150. **Design a conceptual authentication architecture using a browser, frontend, API, authentication service, session/token mechanism, and backend services. Explain every trust boundary and identify where authentication, authorization, session management, and security validation should occur.**

## The Level 2 Mental Model

At Level 1, we had:

```text
Browser
   ↓
DNS
   ↓
TCP/TLS
   ↓
HTTP
   ↓
Web Server
   ↓
Application
   ↓
Database
```

Level 2 should make you see the **internal mechanics**:

```text
                         USER
                           │
                           ▼
                       BROWSER
                           │
                ┌──────────┴──────────┐
                │                     │
             HTML/CSS             JavaScript
                                      │
                                      ▼
                                  Fetch/XHR
                                      │
                                      ▼
                                   HTTPS
                                      │
                                      ▼
                              Reverse Proxy/CDN
                                      │
                                      ▼
                                Web Server
                                      │
                                      ▼
                              Application/API
                                      │
              ┌───────────────────────┼───────────────────────┐
              │                       │                       │
        Authentication          Authorization             Logic
              │                       │                       │
              └───────────────────────┼───────────────────────┘
                                      │
                           ┌──────────┴──────────┐
                           │                     │
                       Database                Cache
                           │                     │
                           └──────────┬──────────┘
                                      │
                                   Response
                                      │
                                      ▼
                                   Browser
```

But now we also understand the security boundaries:

```text
                    ┌───────────────────┐
                    │      BROWSER      │
                    │                   │
                    │ HTML              │
                    │ JavaScript        │
                    │ Cookies           │
                    │ DOM               │
                    └─────────┬─────────┘
                              │
                       TRUST BOUNDARY
                              │
                              ▼
                    ┌───────────────────┐
                    │   WEB / API       │
                    │                   │
                    │ Routing           │
                    │ Authentication    │
                    │ Authorization     │
                    │ Validation        │
                    └─────────┬─────────┘
                              │
                       TRUST BOUNDARY
                              │
                              ▼
                    ┌───────────────────┐
                    │    BACKEND        │
                    │                   │
                    │ Business Logic    │
                    │ Services          │
                    │ Internal APIs     │
                    └─────────┬─────────┘
                              │
                       TRUST BOUNDARY
                              │
                              ▼
                    ┌───────────────────┐
                    │     DATA          │
                    │                   │
                    │ Database          │
                    │ Files             │
                    │ Cache             │
                    └───────────────────┘
```

## What I Want You to Start Thinking About

Level 1 asked:

> **"How does this work?"**

Level 2 should increasingly make you ask:

> **"Who controls this?"**

> **"Who trusts this?"**

> **"Where is this validated?"**

> **"Where is authentication performed?"**

> **"Where is authorization performed?"**

> **"What crosses this boundary?"**

> **"What does the browser enforce?"**

> **"What does the server enforce?"**

> **"What happens if the frontend and backend disagree?"**

> **"What happens if a user modifies something the frontend normally controls?"**

> **"What information is exposed through the API?"**

> **"What assumptions does one component make about another component?"**

Those questions are the beginning of **real web-security thinking**.

## How Level 2 Connects to Our Other Subjects

This is where our integrated learning approach becomes important.

### Networking + Web

```text
DNS
 ↓
IP
 ↓
TCP / QUIC
 ↓
TLS
 ↓
HTTP
 ↓
Web Application
```

### Linux + Web

```text
Linux
 ↓
Processes
 ↓
Sockets
 ↓
Listening Services
 ↓
Web Server
 ↓
Application
```

### Python + Web

```text
Python
 ↓
HTTP Clients
 ↓
APIs
 ↓
JSON
 ↓
Automation
 ↓
Web Analysis
```

### Web + Security

```text
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

And eventually:

```text
Web Fundamentals
       +
Security Fundamentals
       +
Networking
       +
Linux
       +
Python
       ↓
Web Pentesting / AppSec
```

## The Most Important Level 2 Outcome

If I give you a hypothetical application:

```text
https://shop.example.com
```

I don't want you to see just:

> **"It's a website."**

I want you to mentally decompose it:

```text
                    shop.example.com
                           │
                         DNS
                           │
                       IP Address
                           │
                     TLS / HTTPS
                           │
                    Reverse Proxy
                           │
                      Web Server
                           │
                    Frontend / API
                    ┌──────┴──────┐
                    │             │
                  HTML          JavaScript
                                  │
                                Fetch
                                  │
                                  ▼
                              API Calls
                                  │
                        Authentication
                                  │
                         Authorization
                                  │
                         Business Logic
                                  │
                    ┌─────────────┴─────────────┐
                    │                           │
                Database                      Cache
```

And then the cybersecurity brain kicks in:

```text
What are the endpoints?

What inputs exist?

What is user-controlled?

What is trusted?

Where is authentication enforced?

Where is authorization enforced?

How is session state maintained?

What APIs exist?

What data does each API expose?

What browser security controls apply?

What server-side controls apply?

What happens across each trust boundary?
```

That is the bridge between **learning the web** and eventually becoming capable of doing **professional web penetration testing and AppSec**.

**Level 3 will take this model and turn it explicitly toward security:** attack surface analysis, security controls, common web vulnerability classes, secure architecture, abuse cases, detection, and defensive design—while still keeping the underlying web fundamentals strong.
