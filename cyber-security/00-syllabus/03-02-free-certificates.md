Yes. If the goal is **free certifications that actually help a cybersecurity fresher's internship resume**, I would be selective. Don't collect 15 random course certificates.

As of **September 2026**, there are still some genuinely useful free options, but several certifications that used to be free are now paid or have closed their free programs. For example, ISC2's large free CC initiative stopped accepting new participants on May 20, 2026. ([ISC2][1])

## My recommended free certification path

| Priority | Certification / Credential                             |                                          Cost | Value for fresher                              | Target                     |
| -------- | ------------------------------------------------------ | --------------------------------------------: | ---------------------------------------------- | -------------------------- |
| ⭐⭐⭐⭐⭐    | **Fortinet NSE 1–3**                                   |                                          Free | Good                                           | General / Network Security |
| ⭐⭐⭐⭐     | **Cisco cybersecurity credentials/courses**            |                                  Free options | Good                                           | General / SOC              |
| ⭐⭐⭐⭐     | **ISC2 Certified in Cybersecurity (CC)**               | Free **only if already enrolled / have code** | Very good                                      | General Cybersecurity      |
| ⭐⭐⭐      | **Cisco Networking Academy cybersecurity credentials** |                      Free courses/credentials | Good                                           | General / SOC              |
| ⭐⭐⭐      | **Qualys training/certificates**                       |                                  Free options | Moderate                                       | Vulnerability Management   |
| ⭐⭐⭐      | **IBM Cybersecurity Fundamentals**                     |              Free learning/credential options | Moderate                                       | General                    |
| ⭐⭐⭐      | **AttackIQ Foundations**                               |                                          Free | Moderate                                       | Blue Team / SOC            |
| ⭐⭐       | **Microsoft security fundamentals learning**           |             Training free; exam normally paid | Good learning, not normally free certification | Cloud/Security             |
| ⭐⭐       | **Google Cybersecurity Certificate**                   |                      Usually paid after trial | Good learning, not a genuinely free cert       | General                    |

### 1. Fortinet NSE 1–3 — **my #1 free choice**

Fortinet currently provides free self-paced cybersecurity training, and its certification program lists entry-level NSE credentials. ([Fortinet][2])

For your students, I'd do:

**NSE 1 → NSE 2 → NSE 3**

This fits particularly well with your curriculum because students are already studying:

```text
Networking
   ↓
Firewalls
   ↓
Network Security
   ↓
SOC
   ↓
VAPT
```

[Fortinet Training & Certification](https://www.fortinet.com/training-certification?utm_source=chatgpt.com)

---

# 2. ISC2 Certified in Cybersecurity — CC

This is the one I'd **prioritize if the student already has access to the old free program**.

ISC2's CC is specifically an entry-level cybersecurity certification and requires no work experience. ([ISC2][3])

However, there is an important 2026 change:

> The **One Million Certified in Cybersecurity** program stopped accepting new participants on **May 20, 2026**.

Existing participants with valid exam codes can still take their exam through December 31, 2026. ([ISC2][1])

So I would **not put "ISC2 CC – Free" on a new student's plan assuming they can register today.**

If they already enrolled/received a code → **take it.**

Otherwise → move on to Fortinet/Cisco.

---

# 3. Cisco Cybersecurity

Cisco is particularly useful because it connects cybersecurity with networking.

Cisco currently offers free Networking Academy learning, including **Introduction to Cybersecurity** and networking courses. ([Cisco][4])

Cisco also has an entry-level **CCST Cybersecurity** certification. ([Cisco][5])

However, don't confuse:

> **Free Cisco training**

with:

> **Free CCST certification exam**

The training can be free while the certification exam may not be.

For your students, I'd definitely take the **free Cisco cybersecurity courses/credentials**, but I wouldn't promise that the formal CCST exam itself is free.

[Cisco Networking Academy](https://www.cisco.com/site/us/en/learn/training-certifications/training/netacad/index.html?utm_source=chatgpt.com)

---

# 4. AttackIQ Foundations

This is a nice additional credential for students leaning toward:

**SOC / Blue Team / Detection**

It complements:

```text
Networking
      +
Windows
      +
Logs
      +
SIEM
      +
Incident Response
      +
AttackIQ
```

rather than being a replacement for a major certification.

---

# 5. Qualys

Qualys has free cybersecurity learning/training options and is particularly relevant to:

* Vulnerability management
* Vulnerability scanning
* Asset discovery
* Security assessment

That makes it useful if the student wants:

**VAPT / Vulnerability Management internships.**

But again, I would treat it as a **supporting credential**, not the centerpiece of the resume.

---

# 6. Microsoft SC-900 — excellent learning, but don't count it as free

Microsoft's **Security, Compliance, and Identity Fundamentals (SC-900)** is genuinely relevant to entry-level security and Microsoft security concepts. Microsoft describes it as a beginner-level certification for students and IT professionals. ([Microsoft Learn][6])

But normally:

```text
Microsoft Learn training → FREE
SC-900 exam → PAID
```

So don't put it into a "**100% free certifications**" roadmap unless the student has a legitimate exam voucher.

---

# What I would actually put on Zuber's resume

I wouldn't fill the resume with every certificate.

If Zuber completes the semester roadmap, I'd aim for:

### Certifications

```text
Fortinet NSE 1: Cybersecurity
Fortinet NSE 2: The Evolution of Cybersecurity
Fortinet NSE 3: Fundamentals of Cybersecurity

Cisco Networking Academy:
Introduction to Cybersecurity
Networking Basics

ISC2 Certified in Cybersecurity (CC)   [if already eligible/enrolled]
```

Then put the actual practical work **above certifications**.

For example:

```text
PROJECTS

Web Security Lab
SOC / Log Analysis Lab
Network Security Lab
Python Network Reconnaissance Tool
```

That will be more convincing than having:

```text
15 certificates
0 projects
0 writeups
```

---

# The ideal FREE roadmap for your students

Given the curriculum we've designed, I'd make it:

```text
                    SEMESTER 1
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
      Networking       Linux          Security
          │              │              │
          └──────────────┼──────────────┘
                         ↓
                    Cisco Courses
                         │
                         ↓
                  Fortinet NSE 1–3
                         │
                         ↓
                 Security Projects
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
           VAPT                     SOC
             │                       │
          Qualys                  AttackIQ
             │                       │
             └───────────┬───────────┘
                         ↓
                  Portfolio + Resume
                         ↓
                   INTERNSHIPS
```

## And I would impose a rule:

**Maximum 3–5 certifications/credentials before internship applications.**

The student should spend the remaining time on:

**Labs > Projects > Writeups > CTFs > GitHub > Certifications**

because a recruiter can verify a certificate, but a strong GitHub project and security investigation show **what the student can actually do**.

Also, free training is abundant: Cisco explicitly offers free self-paced learning, and Fortinet says its self-paced cybersecurity curriculum is available at no cost. ([Cisco][4])

If you want, I can next make a **“Free Cybersecurity Certification Roadmap 2026”** specifically for these students, with **exact course → exam → project → resume placement**, split into **VAPT, SOC, Cloud Security, and AppSec**.

[1]: https://www.isc2.org/insights/2026/04/one-million-certified-cyber-conclusion?queryID=ff0d7aa6d4bdbea9953953f5064069b7&utm_source=chatgpt.com "ISC2 Provided 1 Million People Globally with Free Access to the Certified in Cybersecurity Certification Course and Exam"
[2]: https://www.fortinet.com/training-certification?utm_source=chatgpt.com "How to get Certified for Cybersecurity | Fortinet"
[3]: https://www.isc2.org/certifications/CC?utm_source=chatgpt.com "CC Certified in Cybersecurity Certification | ISC2"
[4]: https://www.cisco.com/site/us/en/learn/training-certifications/training/netacad/index.html?utm_source=chatgpt.com "Cisco Networking Academy - Cisco"
[5]: https://www.cisco.com/site/us/en/learn/training-certifications/certifications/cybersecurity/index.html?utm_source=chatgpt.com "Cybersecurity - Cisco"
[6]: https://learn.microsoft.com/en-us/credentials/certifications/security-compliance-and-identity-fundamentals/?utm_source=chatgpt.com "Microsoft Certified: Security, Compliance, and Identity Fundamentals - Certifications | Microsoft Learn"
