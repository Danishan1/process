# Web Fundamentals — Level 1

**Web Fundamentals is a major turning point in our roadmap**, because it becomes the bridge between our foundation and both **Red Team/Pentesting and AppSec**.

We've already built:

> **Networking → Linux → Python**

Now we add:

> **Web Fundamentals → Security Fundamentals → Network Security → Red Team / Blue Team → AppSec**

For us, Web Fundamentals should **not** mean simply learning HTML/CSS/JavaScript. We need to understand **how the web actually works**, because later we'll need to reason about:

* Web servers
* Browsers
* HTTP/HTTPS
* DNS
* TCP
* URLs
* Requests/responses
* Headers
* Cookies
* Sessions
* Authentication
* Authorization
* APIs
* Databases
* Web application architecture
* Browser security
* Trust boundaries

The progression will be:

> **Level 1 — Understand how the web works**
> **Level 2 — Understand how web applications work internally**
> **Level 3 — Understand the web from a security/attack-surface perspective**

### Basic Question Bank

**120 questions**

## A. The Internet vs the Web

1. **What is the Internet, and how is it different from the World Wide Web?**

2. **What happens conceptually when you connect a computer or phone to the Internet?**

3. **What is a client, and what is a server? Explain their relationship using a web application as an example.**

4. **What is a web server? What responsibilities does it have?**

5. **What is a web browser? What responsibilities does the browser perform when accessing a website?**

6. **What happens at a high level when you type a website address into a browser and press Enter?**

7. **Which components participate between your browser and the destination web server during a typical web request?**

8. **Why does understanding networking make it easier to understand how the web works?**

9. **What is a protocol? Why do computers need agreed-upon protocols to communicate?**

10. **What protocols are commonly involved when a user accesses a modern website? Explain the role of each at a high level.**

## B. URLs & Web Addresses

11. **What is a URL? Break down the major components of a URL and explain what each component represents.**

12. **What is a scheme such as `http` or `https`?**

13. **What is a domain name? How is it different from an IP address?**

14. **What is a port number, and how can a URL indicate a specific port?**

15. **What is the path portion of a URL?**

16. **What is a query string? Why is it commonly used by web applications?**

17. **What are URL parameters? How are they different from path components?**

18. **What is a URL fragment? Why is it generally handled differently from the rest of a URL?**

19. **Why is URL encoding necessary? Give examples of characters that may need encoding.**

20. **From a cybersecurity perspective, why is understanding every component of a URL important when analyzing a web application?**

## C. DNS & the Web

21. **Why does a browser usually need DNS before it can communicate with a web server?**

22. **Explain the conceptual process of resolving a domain name into an IP address.**

23. **What is the difference between an A record and an AAAA record?**

24. **What is a CNAME record? Why might websites use it?**

25. **What is DNS caching, and why does it improve performance?**

26. **Why can DNS information reveal useful information about an organization's web infrastructure?**

27. **What is the difference between a domain, subdomain, and hostname?**

28. **Why might a large organization have many subdomains serving different applications or services?**

29. **How could incorrect DNS configuration affect the availability or security of a website?**

30. **Why should a cybersecurity professional understand DNS before studying web application security?**

## D. HTTP Fundamentals

31. **What is HTTP, and what problem does it solve?**

32. **Explain the basic structure of an HTTP request.**

33. **Explain the basic structure of an HTTP response.**

34. **What is the HTTP request method? Why does the method communicate intent to the server?**

35. **What are GET and POST requests? Explain their typical purposes.**

36. **What are PUT, PATCH, and DELETE used for conceptually?**

37. **Why is it incorrect to think of GET as automatically "safe" simply because it normally retrieves information?**

38. **What is an HTTP status code?**

39. **What do the broad HTTP status-code categories 1xx, 2xx, 3xx, 4xx, and 5xx represent?**

40. **Why is understanding HTTP status codes important during web application troubleshooting and security assessment?**

## E. HTTP Headers

41. **What is an HTTP header? Why do requests and responses contain metadata in headers?**

42. **What is the `Host` header, and why is it important in HTTP communication?**

43. **What does the `User-Agent` header communicate?**

44. **What is the `Accept` header?**

45. **What does `Content-Type` tell the server or client?**

46. **What is `Content-Length`, and why can it matter when processing HTTP messages?**

47. **What are request headers versus response headers?**

48. **What is the purpose of the `Location` response header?**

49. **Why can HTTP headers provide useful information during security reconnaissance?**

50. **Why should security professionals avoid assuming that a header value is trustworthy simply because it came from an HTTP request?**

## F. HTTP Methods & Semantics

51. **Why does HTTP define multiple request methods instead of using one method for every operation?**

52. **What does it mean for an HTTP method to be idempotent?**

53. **What does it mean for an HTTP method to be safe according to HTTP semantics?**

54. **Why is understanding method semantics important when designing secure web applications?**

55. **What security problems could arise if a web application performs state-changing operations through a GET request?**

56. **Why might an API use POST for one operation and PUT for another?**

57. **How can the HTTP method contribute to understanding the intended behavior of an endpoint?**

58. **Why should a web server reject methods that an application does not actually need?**

59. **How could an attacker benefit from discovering that an application supports an unexpected HTTP method?**

60. **How would you document the expected HTTP methods for a web application as part of its attack-surface inventory?**

## G. HTTP State & Statelessness

61. **What does it mean to say that HTTP is stateless?**

62. **Why does a web application still need mechanisms to maintain state between requests?**

63. **What are cookies, and why were they introduced?**

64. **How does a browser store and send cookies?**

65. **What is the difference between a session cookie and a persistent cookie?**

66. **What is the purpose of the `Secure` cookie attribute?**

67. **What is the purpose of the `HttpOnly` cookie attribute?**

68. **What is the `SameSite` cookie attribute, and why is it security-relevant?**

69. **Why should session identifiers be treated as sensitive information?**

70. **From a security perspective, what could happen if an attacker obtains another user's session identifier?**

## H. HTTPS & TLS

71. **What is HTTPS, and how does it differ from HTTP?**

72. **What security properties does TLS provide to HTTPS?**

73. **What is confidentiality in the context of HTTPS?**

74. **What is integrity in the context of HTTPS?**

75. **What is authentication in the context of TLS?**

76. **What is a digital certificate, and what role does it play in HTTPS?**

77. **What is a Certificate Authority (CA)?**

78. **Why does a browser need to verify the certificate presented by a web server?**

79. **What could happen if TLS certificate validation were completely ignored?**

80. **Why does HTTPS protect data in transit but not automatically make a web application secure?**

## I. HTML Fundamentals

81. **What is HTML, and what role does it play in a web application?**

82. **What is an HTML element?**

83. **What is the difference between an HTML element and an HTML attribute?**

84. **What is the purpose of the `<head>` and `<body>` sections of an HTML document?**

85. **What are links in HTML, and how does the browser use them to navigate between resources?**

86. **What are HTML forms? Why are forms particularly important from a cybersecurity perspective?**

87. **What are form actions and methods? How do they determine where and how submitted data is sent?**

88. **What are common form input types, and why should developers choose them appropriately?**

89. **Why should client-side validation never be considered a replacement for server-side validation?**

90. **From a security perspective, why are HTML forms an important boundary between user-controlled input and application processing?**

## J. CSS Fundamentals

91. **What is CSS, and what role does it play in a web application?**

92. **How does CSS differ from HTML?**

93. **What are selectors in CSS?**

94. **What are classes and IDs in HTML/CSS, and how are they commonly used?**

95. **Why is CSS generally considered presentation logic rather than application logic?**

96. **Can CSS itself create security problems? Discuss the concept without assuming that every unusual CSS behavior is a vulnerability.**

97. **Why should a cybersecurity professional understand basic CSS even though it is not normally a major security boundary?**

98. **How can CSS help you understand the structure and components of a web page during security analysis?**

## K. JavaScript Fundamentals

JavaScript is extremely important for us because later **AppSec** will require understanding what happens inside the browser.

99. **What is JavaScript, and what role does it play in modern web applications?**

100. **How is JavaScript different from HTML and CSS?**

101. **What is the Document Object Model (DOM)?**

102. **How can JavaScript interact with the DOM?**

103. **What is an event in the browser? Give examples of common browser events.**

104. **What is client-side JavaScript execution? How is it different from server-side processing?**

105. **What is the difference between JavaScript code embedded in HTML and JavaScript loaded from an external file?**

106. **What are browser APIs? Why are they important to modern web applications?**

107. **What is asynchronous JavaScript? Why is asynchronous behavior common in web applications?**

108. **Why should cybersecurity professionals understand JavaScript even if they do not intend to become professional JavaScript developers?**

## L. Browser Architecture

109. **What major components of a modern browser participate when loading and displaying a web application?**

110. **What is the browser's rendering process at a high level?**

111. **Why does a browser execute JavaScript in a controlled environment rather than giving every website unrestricted access to the operating system?**

112. **What is browser isolation, and why is it important for security?**

113. **What is the browser sandbox? Explain its security purpose at a high level.**

114. **What types of resources can a web page request from a browser?**

115. **Why does a browser need security policies governing how one website can interact with another?**

116. **What is the same-origin concept? Why is it fundamental to web security?**

117. **What information about a browser environment can JavaScript legitimately access, and why are there restrictions on more sensitive information?**

118. **Why is browser security fundamentally different from simply securing the web server?**

## M. Web Servers & Application Servers

119. **What is the difference between a web server and an application server?**

120. **What happens after a web server receives an HTTP request for a dynamic application? Describe the high-level flow from request to response.**

## N. The Web Application Architecture

These final questions are especially important because they prepare us for **Level 2**.

121. **What is a static website? How does it differ from a dynamic web application?**

122. **What is a backend? What responsibilities typically belong to the backend of a web application?**

123. **What is a frontend? What responsibilities typically belong to the frontend?**

124. **What is a database, and why do many web applications require one?**

125. **What is the typical relationship between browser, frontend, backend, and database?**

126. **Why should a browser generally not connect directly to a database?**

127. **What is an API in the context of a web application?**

128. **How does a frontend communicate with a backend API?**

129. **What is a three-tier architecture? Explain the presentation, application, and data layers.**

130. **What security boundaries exist between the browser, web server, application server, API, and database?**

## O. Web Security Thinking

Now we begin forcing the **cybersecurity mindset** into the fundamentals.

131. **Which parts of a web application are controlled by the user, and which parts are controlled by the server? Why is this distinction important for security?**

132. **Why should everything received from a browser be considered potentially untrusted by the server?**

133. **What is the difference between authentication and authorization in a web application?**

134. **Why can a web application have strong authentication but weak authorization?**

135. **What is an attack surface in the context of a web application?**

136. **What components contribute to a web application's attack surface?**

137. **Why are URLs, HTTP methods, parameters, headers, cookies, forms, APIs, and file uploads all potentially relevant to a security assessment?**

138. **Why should security professionals understand the normal behavior of an application before looking for abnormal or malicious behavior?**

139. **What kinds of information could an attacker learn simply by observing normal HTTP traffic?**

140. **Why is understanding how a web application works normally the foundation for understanding how it could be abused?**

## P. Putting Everything Together

141. **Trace a complete web request from a user entering a URL into the browser to the final response being rendered on the screen. Include DNS, TCP, TLS, HTTP, the web server, application logic, and database interaction where appropriate.**

142. **Imagine a user logs into an online application. Explain the major components involved from entering credentials to receiving an authenticated session.**

143. **Trace what happens when a user submits an HTML form containing username, email, and password information.**

144. **What information travels between the browser and server during a typical authenticated session, and which parts should be considered sensitive?**

145. **Suppose a web application suddenly starts returning HTTP 500 errors. What layers would you investigate, starting from the browser and moving toward the backend?**

146. **Suppose the browser can reach the domain but cannot establish an HTTPS connection. Which layers of the web stack would you investigate?**

147. **Suppose HTTPS works, but the application returns HTTP 404 for a known page. What does this tell you about which layers are probably functioning correctly?**

148. **Suppose a web application works correctly for one user but another authenticated user can access data they should not see. Which security concept does this immediately make you think about, and why?**

149. **During an authorized web assessment, you discover many subdomains, APIs, HTTP methods, forms, cookies, and endpoints. How would you organize this information into an attack-surface inventory?**

150. **Design a complete mental model of a modern web application from browser to database. For every layer, explain what it does, what data crosses the boundary, what could go wrong, and what a cybersecurity professional should observe when assessing or defending it.**

## The Web Fundamentals Level 1 Mental Model

By the end of Level 1, I want your mental picture to look something like this:

```text
                         INTERNET
                            │
                           DNS
                            │
                         TCP/IP
                            │
                           TLS
                            │
                          HTTPS
                            │
                    ┌───────▼───────┐
                    │   WEB SERVER  │
                    └───────┬───────┘
                            │
                    Application/API
                            │
                    ┌───────▼───────┐
                    │    BACKEND    │
                    └───────┬───────┘
                            │
                         DATABASE
```

But the browser side is equally important:

```text
                     BROWSER
                        │
        ┌───────────────┼────────────────┐
        │               │                │
       HTML            CSS          JavaScript
        │               │                │
        └───────────────┼────────────────┘
                        │
                       DOM
                        │
                   Browser APIs
                        │
                     HTTP(S)
                        │
                      SERVER
```

And from the **security perspective**:

```text
                 WEB APPLICATION
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
    Browser         Network          Server
       │               │                │
    HTML/JS          DNS/TLS         Backend
    Cookies          HTTP            Database
    DOM              APIs            Files
       │               │                │
       └───────────────┼────────────────┘
                       ↓
                 TRUST BOUNDARIES
                       ↓
                 ATTACK SURFACE
```

## What We Should Be Able to Explain

After Web Fundamentals Level 1, if someone asks:

> **"What happens when you open `https://example.com/login`?"**

You shouldn't answer:

> "The browser sends a request to the server."

You should mentally walk through something closer to:

```text
URL
 ↓
DNS resolution
 ↓
IP address
 ↓
TCP connection
 ↓
TLS negotiation
 ↓
HTTPS request
 ↓
HTTP headers
 ↓
Web server
 ↓
Application routing
 ↓
Authentication/session logic
 ↓
Database interaction
 ↓
HTTP response
 ↓
Browser
 ↓
HTML/CSS/JavaScript
 ↓
DOM
 ↓
Rendered page
```

And then immediately be able to ask:

```text
Where is the trust boundary?

What data is user-controlled?

Where does authentication happen?

Where does authorization happen?

Where is session state stored?

What information crosses the network?

What does the browser enforce?

What does the server enforce?

What endpoints exist?

What APIs exist?

What happens if something unexpected is supplied?
```

**That is exactly the mindset we need before Web Fundamentals Level 2**, where we'll go considerably deeper into **HTTP, sessions, authentication, authorization, APIs, databases, browser internals, JavaScript, web architecture, and how the different components interact**.

