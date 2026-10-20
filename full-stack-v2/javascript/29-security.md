# 27. JavaScript Security

For SDE-II, JavaScript security should not be learned as a list of vulnerabilities.

The important skill is understanding:

> **Where does untrusted data enter the system, how does it flow through the application, and at what point can it become executable, privileged, or dangerous?**

A useful mental model:

```text
                    Untrusted Input
                          │
             ┌────────────┼────────────┐
             │            │            │
           Browser       API          Package
             │            │            │
             ▼            ▼            ▼
            XSS        Injection    Supply Chain
             │            │            │
             └────────────┼────────────┘
                          │
                    Application
                          │
             ┌────────────┼────────────┐
             │            │            │
          Identity      Data         Browser
             │            │            │
          CSRF/Auth   Pollution    SOP/CORS/CSP
```

The central principle is:

> **Never trust input simply because it came through your own frontend.**

---

# 1. XSS — Cross-Site Scripting

XSS happens when attacker-controlled data becomes executable JavaScript in another user's browser.

Suppose:

```js
const comment = userInput;

element.innerHTML = comment;
```

Attacker provides content that contains executable HTML/JavaScript.

The dangerous flow is:

```text
Attacker input
     ↓
Application
     ↓
HTML sink
     ↓
Browser interprets it
     ↓
Attacker-controlled code executes
```

---

# 2. Why `innerHTML` Can Be Dangerous

Consider:

```js
element.innerHTML = userInput;
```

`innerHTML` tells the browser:

> "Interpret this string as HTML."

Whereas:

```js
element.textContent = userInput;
```

means:

> "Treat this as text."

So:

```text
User input
    │
    ├── textContent → text
    │
    └── innerHTML   → HTML parsing
```

If the input is untrusted, `textContent` is generally much safer.

---

# 3. XSS Is Not Just `innerHTML`

Other dangerous patterns can include:

```js
element.innerHTML = input;
```

```js
element.outerHTML = input;
```

or dynamically constructing executable browser contexts.

The broader concept is:

> **Untrusted data must not accidentally cross into an executable context.**

This is why frameworks often provide automatic escaping.

For example, React's normal JSX rendering:

```jsx
<div>{user.name}</div>
```

escapes text by default.

But intentionally bypassing that protection, such as using raw HTML rendering, requires much more care.

---

# 4. Stored vs Reflected XSS

### Stored XSS

Attacker input is stored:

```text
Attacker
   ↓
POST comment
   ↓
Database
   ↓
Victim opens page
   ↓
Malicious content rendered
```

Example:

```text
comment = malicious content
```

stored in PostgreSQL/MongoDB.

Every victim who views the comment could be affected.

---

### Reflected XSS

Input comes directly from a request and is reflected into the response.

For example:

```text
/search?q=<attacker-controlled-value>
```

and the application puts that value into HTML without proper escaping.

---

# 5. DOM-Based XSS

This happens primarily through client-side JavaScript.

```text
URL
 ↓
JavaScript
 ↓
dangerous DOM operation
 ↓
HTML/JS execution
```

For example:

```js
const value = new URLSearchParams(location.search).get("name");

element.innerHTML = value;
```

The server may never see the malicious input.

---

# 6. XSS Prevention

The core defenses are:

### 1. Output encoding/escaping

Treat data as data.

### 2. Avoid dangerous HTML sinks

Prefer:

```js
element.textContent = value;
```

over:

```js
element.innerHTML = value;
```

when HTML isn't actually required.

### 3. Sanitize HTML when HTML is genuinely required

If your application needs user-generated rich text, use a well-maintained HTML sanitizer configured appropriately.

### 4. Content Security Policy

CSP provides another layer of browser-side defense.

### 5. Secure cookies

`HttpOnly` cookies prevent JavaScript from directly reading the cookie.

But remember:

> **HttpOnly reduces token theft through JavaScript; it does not prevent XSS itself.**

---

# 7. CSRF — Cross-Site Request Forgery

CSRF is fundamentally about **unwanted authenticated actions**.

Suppose authentication uses a cookie:

```text
Browser
  │
  └── automatically sends authentication cookie
```

Victim is logged into:

```text
bank.example
```

Attacker tricks them into visiting:

```text
attacker.example
```

The attacker page causes a request to the bank.

Conceptually:

```text
Victim browser
      │
      │ authenticated cookie automatically included
      ▼
Bank API
      ▲
      │
attacker-controlled page triggers request
```

The server may see:

```text
authenticated request
```

even though the victim did not intentionally perform the action.

---

# 8. Why Cookies Matter for CSRF

Cookies are automatically attached by the browser according to cookie rules.

That makes cookie-based authentication particularly relevant to CSRF.

Compare:

```text
Cookie authentication
→ browser automatically sends cookie
→ CSRF needs consideration
```

versus:

```text
Authorization: Bearer <token>
→ JavaScript explicitly adds header
→ cross-site attacker generally cannot simply cause an arbitrary authenticated request with that header
```

But token-based auth has its own security tradeoffs.

---

# 9. CSRF Defenses

Common defenses include:

### SameSite cookies

For example:

```text
SameSite=Lax
```

or:

```text
SameSite=Strict
```

depending on application requirements.

### CSRF tokens

Server generates a token that the legitimate client must include.

```text
request
+
CSRF token
↓
server validates
```

### Origin/Referer validation

Servers can validate request origin information where appropriate.

### Architectural choices

Use authentication mechanisms and request designs that don't unnecessarily expose state-changing operations to cross-site requests.

---

# 10. XSS vs CSRF

Very common interview question.

| XSS                                               | CSRF                                                                     |
| ------------------------------------------------- | ------------------------------------------------------------------------ |
| Attacker gets code to execute in victim's browser | Attacker causes victim's browser to make an unwanted request             |
| Exploits unsafe rendering/execution               | Exploits authentication/request semantics                                |
| JavaScript execution is central                   | Authentication credentials being automatically included is often central |
| Output encoding/CSP are important defenses        | SameSite/CSRF tokens/origin checks are important defenses                |

And they can interact.

For example:

```text
XSS
 ↓
attacker JavaScript runs in your origin
 ↓
CSRF defenses may become less meaningful
```

An XSS vulnerability can be extremely powerful because the attacker is executing code in the application's own origin.

---

# 11. Prototype Pollution

This is particularly JavaScript-specific.

JavaScript objects have prototypes.

For example:

```js
const obj = {};
```

ultimately participates in the prototype chain.

Prototype pollution happens when attacker-controlled input can modify a prototype in an unintended way.

Conceptually:

```text
Attacker input
      ↓
unsafe object merge / assignment
      ↓
prototype
      ↓
unexpected properties inherited by many objects
```

---

# 12. Why Prototype Pollution Is Dangerous

Imagine some application logic:

```js
if (options.isAdmin) {
    grantAdminAccess();
}
```

Normally:

```js
const options = {};
```

doesn't have `isAdmin`.

If an attacker can manipulate the prototype:

```text
Object.prototype.isAdmin = true
```

then:

```js
options.isAdmin
```

may unexpectedly resolve to:

```text
true
```

This can turn an apparently harmless object lookup into a security problem.

The exact exploitability depends on the application and vulnerable operation.

---

# 13. Common Sources of Prototype Pollution

Historically, vulnerable object merge/deep-set utilities have been a common source.

Risky patterns include blindly processing attacker-controlled paths such as:

```text
__proto__
constructor
prototype
```

For example, generic code that takes:

```text
"some.nested.path"
```

and dynamically creates object properties can become dangerous if it doesn't validate keys.

---

# 14. Prototype Pollution Defenses

### Validate keys

Don't blindly accept arbitrary property paths.

### Use safe object operations

Be deliberate about dynamic assignment and merging.

### Keep dependencies updated

Many prototype pollution vulnerabilities have existed in third-party packages.

### Consider null-prototype objects for specific dictionaries

```js
const dictionary = Object.create(null);
```

This creates an object without the normal `Object.prototype`.

But this isn't a universal security solution.

### Use `Map` for maps

If you need a key-value collection:

```js
const data = new Map();
```

may be clearer than using a normal object.

---

# 15. Injection

Injection means:

> **Untrusted data changes the meaning of a command/query rather than remaining data.**

JavaScript applications can encounter multiple forms:

```text
SQL injection
NoSQL injection
OS command injection
LDAP injection
template injection
HTML injection
```

The fundamental pattern is:

```text
Data
 ↓
incorrectly combined with executable/query syntax
 ↓
interpreted as instructions
```

---

# 16. SQL Injection

Bad:

```js
const query = `
    SELECT *
    FROM users
    WHERE email = '${email}'
`;
```

If `email` contains malicious SQL syntax, the query structure itself may be altered.

Instead use parameterized queries:

```js
const result = await db.query(
    "SELECT * FROM users WHERE email = $1",
    [email]
);
```

Now:

```text
SQL structure
+
user value
```

remain separate.

This is one of the most important security principles:

> **Don't build executable/query languages by concatenating untrusted strings.**

---

# 17. NoSQL Injection

The same concept can happen with NoSQL databases.

Suppose an API expects:

```json
{
  "username": "danish",
  "password": "secret"
}
```

but blindly passes the entire request body into a database query.

An attacker may provide unexpected object structures rather than strings.

Therefore:

> **Validate input types and explicitly construct the query you intend.**

Don't assume:

```text
"MongoDB doesn't use SQL"
```

means:

```text
"No injection risk."
```

---

# 18. OS Command Injection

Dangerous:

```js
exec(`ping ${userInput}`);
```

If user input becomes part of an operating-system command, the attacker may be able to alter what the shell executes.

Safer approaches include:

* avoiding shell execution entirely
* using dedicated APIs
* passing arguments separately
* validating against an allowlist

The best solution is often:

> **Don't invoke a shell when you don't need one.**

---

# 19. Unsafe Evaluation

JavaScript provides powerful dynamic execution mechanisms:

```js
eval()
```

and:

```js
new Function(...)
```

These can turn strings into executable JavaScript.

For example:

```js
eval(userInput);
```

is essentially:

```text
attacker input
      ↓
JavaScript execution
```

That's an extremely dangerous boundary.

---

# 20. Why `eval()` Is Bad

Security isn't the only problem.

`eval()` can cause:

* code injection
* difficult static analysis
* harder debugging
* optimization problems
* unpredictable behavior

Avoid it unless you have an extremely strong, well-understood reason.

And:

```js
new Function(...)
```

has similar concerns.

---

# 21. "Don't Use eval" Is Not Enough

The broader principle is:

> **Don't turn data into executable code.**

For example, instead of:

```js
eval(`calculate(${value})`);
```

use explicit logic:

```js
calculate(value);
```

Instead of:

```js
eval(`obj.${field}`);
```

use:

```js
obj[field];
```

Dynamic property access is data-driven.

`eval()` turns a string into code.

---

# 22. Dependency Vulnerabilities

Your application may be secure while one of your dependencies isn't.

Your dependency graph might look like:

```text
Application
   │
   ├── express
   │     └── dependency-A
   │            └── vulnerable package
   │
   └── library-B
         └── dependency-C
```

You didn't directly install the vulnerable package.

But it still exists in your application.

This is why dependency security matters.

---

# 23. Supply-Chain Attacks

A supply-chain attack goes beyond a known vulnerability.

The attacker compromises something you trust:

```text
Developer
    ↓
Package manager
    ↓
Third-party package
    ↓
Your application
```

Potential attack points include:

* compromised package
* malicious package maintainer
* stolen maintainer credentials
* malicious dependency update
* typosquatting
* compromised build infrastructure

For example, you intend to install:

```text
some-popular-package
```

but accidentally install:

```text
some-popular-packag
```

which is a malicious package.

---

# 24. Dependency Security Practices

For production:

```text
package-lock.json
        ↓
reproducible dependency tree
```

Use:

```bash
npm audit
```

where appropriate.

Also:

* update dependencies regularly
* review dependency changes
* remove unused dependencies
* use trusted registries
* protect package publishing credentials
* automate security scanning
* understand transitive dependencies

But don't blindly run:

```bash
npm audit fix
```

in production without reviewing changes.

Security updates can introduce behavior changes or breaking upgrades.

---

# 25. Token Storage

This is an important application-security design decision.

Suppose your application has an access token.

Possible storage:

```text
localStorage
sessionStorage
cookie
memory
```

There isn't one universally correct answer.

---

# 26. `localStorage`

Example:

```js
localStorage.setItem("token", token);
```

Advantages:

* simple
* persistent across browser sessions

But:

> JavaScript can read it.

Therefore, if your application has an XSS vulnerability:

```text
XSS
 ↓
JavaScript executes
 ↓
localStorage.getItem("token")
 ↓
token potentially stolen
```

This is a major consideration.

---

# 27. HttpOnly Cookies

A cookie can be configured:

```text
HttpOnly
Secure
SameSite
```

`HttpOnly` means:

> JavaScript cannot directly read the cookie.

So:

```js
document.cookie
```

doesn't expose an HttpOnly cookie.

This helps reduce direct token theft through XSS.

But remember:

```text
HttpOnly ≠ XSS protection
```

If an attacker gets JavaScript execution in your origin, they may still be able to perform actions as the user even if they cannot read the cookie.

---

# 28. Cookie Security Attributes

### `HttpOnly`

Prevents JavaScript access.

### `Secure`

Cookie is sent over HTTPS connections.

### `SameSite`

Controls cross-site cookie sending behavior.

Common choices:

```text
Strict
Lax
None
```

`SameSite=None` requires `Secure` in modern browsers.

---

# 29. CORS

CORS = Cross-Origin Resource Sharing.

It controls whether browser JavaScript from one origin can access resources from another origin.

Suppose:

```text
Frontend:
https://app.example.com

API:
https://api.example.com
```

These are different origins because the hostnames differ.

The browser applies same-origin restrictions.

The server can explicitly allow certain cross-origin requests through CORS headers.

For example conceptually:

```text
Access-Control-Allow-Origin
```

---

# 30. CORS Is Not Authentication

This is one of the most common interview mistakes.

CORS does **not** mean:

> "Only trusted users can call my API."

CORS primarily controls:

> **Which browser origins are allowed to read responses through browser APIs.**

A server-to-server request isn't blocked by browser CORS enforcement.

An attacker can still use:

```text
curl
Postman
their backend
```

to send requests.

Therefore:

```text
CORS
≠
authentication
≠
authorization
```

---

# 31. Same-Origin Policy

The Same-Origin Policy is a fundamental browser security mechanism.

An origin consists of:

```text
scheme + host + port
```

For example:

```text
https://example.com:443
```

Compare:

```text
https://example.com
https://api.example.com
http://example.com
https://example.com:8080
```

These can represent different origins.

The browser restricts many forms of cross-origin interaction unless explicitly permitted.

CORS provides a controlled mechanism for some cross-origin resource access.

---

# 32. CORS Example

Suppose:

```text
https://frontend.example
```

calls:

```text
https://api.example
```

The browser asks whether the API allows the frontend origin.

The API may respond with appropriate CORS headers.

Conceptually:

```text
Browser
   │
   │ Origin: https://frontend.example
   ▼
API
   │
   │ Access-Control-Allow-Origin:
   │ https://frontend.example
   ▼
Browser allows JS access
```

---

# 33. Never Use `*` Carelessly

You might see:

```http
Access-Control-Allow-Origin: *
```

This means broadly:

> Any origin may access responses, subject to other browser rules.

It is not automatically insecure for every public API, but it is often inappropriate for authenticated/private APIs.

Especially understand:

```text
credentials
+
wildcard origin
```

have important browser restrictions.

For credentialed cross-origin requests, explicitly allowing trusted origins is generally required.

---

# 34. Content Security Policy

CSP is a browser security mechanism that controls what resources and execution sources a page is allowed to use.

For example, conceptually:

```text
Content-Security-Policy:
  script-src 'self'
```

means:

> Only scripts from approved sources should execute.

A stronger CSP can restrict:

```text
scripts
styles
images
fonts
frames
connections
```

and more.

---

# 35. CSP as Defense in Depth

Suppose an XSS bug accidentally exists.

Without CSP:

```text
XSS
 ↓
malicious script
 ↓
execution
```

With a strong CSP:

```text
XSS
 ↓
browser CSP enforcement
 ↓
malicious script may be blocked
```

CSP therefore provides:

> **Defense in depth**

It does not replace proper output encoding and secure application design.

---

# 36. Secure Handling of User Input

This is the umbrella concept behind almost everything we've discussed.

Think in terms of:

```text
Input
  ↓
Validation
  ↓
Normalization
  ↓
Business logic
  ↓
Safe output/context
```

But there is an important nuance:

> **Validation and sanitization are not the same thing.**

---

# 37. Validation

Validation asks:

> **Is this input acceptable?**

Example:

```js
if (!Number.isInteger(age) || age < 0 || age > 150) {
    throw new Error("Invalid age");
}
```

You define what the application accepts.

For example:

```text
email → valid email structure
age → integer
role → allowed enum
quantity → positive integer
```

Prefer allowlists where practical.

---

# 38. Sanitization

Sanitization transforms potentially dangerous input into a safer representation.

This is especially relevant when the application intentionally accepts rich HTML.

But don't think:

```text
sanitize everything once
```

and you're done.

The correct defense depends on **where the data will be used**.

---

# 39. Context Matters

The same string can be safe in one context and dangerous in another.

For example:

```text
HTML context
JavaScript context
SQL context
URL context
shell context
```

Therefore:

> **Use context-appropriate encoding/parameterization.**

For SQL:

```text
parameterized query
```

For HTML:

```text
escaping
```

For rich HTML:

```text
sanitization
```

For OS commands:

```text
avoid shell / structured arguments
```

---

# 40. Input Validation ≠ Security Boundary

Suppose frontend validates:

```js
if (age < 0) {
    showError();
}
```

An attacker can simply bypass the frontend:

```bash
curl ...
```

Therefore:

```text
Frontend validation
        ↓
UX improvement

Backend validation
        ↓
security + correctness
```

The backend must enforce security-sensitive constraints.

---

# 41. A Real Backend Security Flow

Imagine:

```text
POST /orders
```

with:

```json
{
  "productId": "123",
  "quantity": 2
}
```

A secure backend might do:

```text
HTTP request
     ↓
Authentication
     ↓
Authorization
     ↓
Input validation
     ↓
Business validation
     ↓
Parameterized DB query
     ↓
Safe response encoding
```

And around the application:

```text
CORS
CSP
Secure cookies
Rate limiting
Dependency scanning
Logging/monitoring
```

---

# 42. Security Layers

Don't rely on one defense.

```text
                  Security
                     │
       ┌─────────────┼──────────────┐
       │             │              │
    Browser        Backend       Dependencies
       │             │              │
    SOP/CORS       Auth           Audit
    CSP            RBAC           Updates
    Cookies        Validation     Scanning
       │             │              │
       └─────────────┼──────────────┘
                     │
                 Data safety
```

This is **defense in depth**.

---

# 43. SDE-II Security Scenario

Interview question:

> "A developer stores JWTs in localStorage. Is that secure or insecure?"

Don't answer:

> "Always insecure."

A better answer:

> "The main concern is that JavaScript can access localStorage. If the application has an XSS vulnerability, malicious JavaScript may be able to read the token. HttpOnly Secure cookies can reduce direct token theft by JavaScript, but then CSRF and cookie configuration become important. The right choice depends on the application's authentication architecture and threat model."

That's the level expected from an SDE-II.

---

# 44. Another Scenario

> "We enabled CORS for our frontend. Is our API now protected?"

**No.**

CORS controls browser cross-origin access to responses. It doesn't authenticate users or prevent direct requests from tools or servers.

You still need:

```text
authentication
authorization
input validation
rate limiting
secure business logic
```

---

# 45. Another Scenario

> "We use TypeScript, so user input is typed. Are we safe?"

**No.**

TypeScript types are primarily compile-time information.

External data:

```text
HTTP
JSON
database
message queue
user input
```

still exists at runtime.

So:

```text
TypeScript interface
       ≠
runtime validation
```

---

# 46. Another Scenario

> "We escaped HTML, so SQL injection is impossible."

No.

Security defenses are **context-specific**.

HTML escaping doesn't protect SQL.

You need:

```text
HTML → output escaping
SQL → parameterized queries
Shell → avoid shell / safe arguments
URLs → appropriate URL handling
```

---

# 47. SDE-II Security Checklist

You should be able to explain these confidently:

### Browser

```text
XSS
CSRF
Same-Origin Policy
CORS
CSP
cookies
localStorage
sessionStorage
```

### JavaScript-specific

```text
Prototype pollution
eval
new Function
dynamic property access
unsafe DOM APIs
```

### Backend

```text
SQL injection
NoSQL injection
command injection
input validation
parameterized queries
authentication
authorization
```

### Supply chain

```text
npm dependencies
transitive dependencies
lockfiles
dependency vulnerabilities
malicious packages
typosquatting
```

---

# 48. The Security Mental Model

If you remember only one thing from Point 27, remember this:

```text
                  UNTRUSTED DATA
                        │
                        ▼
                 Can it change
                 program behavior?
                        │
             ┌──────────┴──────────┐
             │                     │
            YES                    NO
             │                     │
             ▼                     ▼
         Dangerous              Data
             │
    ┌────────┼─────────┐
    │        │         │
   XSS   Injection   Pollution
    │        │         │
 HTML      SQL       Objects
 JS       NoSQL      Prototype
 Shell
```

Then apply the appropriate defense:

```text
XSS
→ output encoding / safe DOM APIs / CSP

CSRF
→ SameSite / CSRF tokens / origin checks

Injection
→ parameterization / structured APIs / validation

Prototype pollution
→ safe object handling / key validation / dependency updates

eval
→ don't execute untrusted strings

Dependencies
→ lockfiles / updates / auditing / supply-chain controls

Tokens
→ choose storage based on threat model

CORS
→ control browser cross-origin access

SOP
→ browser origin isolation

CSP
→ restrict allowed browser execution/resources

User input
→ validate at trust boundaries + context-appropriate output handling
```

And the most important SDE-II principle:

> **Security is primarily about controlling trust boundaries.**
> Treat browser input, HTTP requests, uploaded data, database contents, queue messages, environment configuration, and third-party packages as things that require explicit trust decisions—not assumptions.
