# Linux — Level 1

The same philosophy applies, but Linux deserves a slightly different structure from Networking. We don't merely want to learn commands. We want to understand **how Linux actually works**, because later that knowledge becomes the foundation for:

* Linux administration
* Linux hardening
* Bash/Python automation
* SOC investigation
* Log analysis
* Network services
* Vulnerability assessment
* Privilege escalation
* Red Team operations
* Incident response
* Security tooling

So Level 1 should answer:

> **What is Linux? → How does Linux work? → How do I operate it? → How does Linux communicate? → How does Linux enforce security? → What happens when something goes wrong?**


### Basic Question Bank

**Target: 100 questions**

## A. Linux Fundamentals

1. **What is Linux, and what exactly do people mean when they use the term "Linux"? Explain the difference between the Linux kernel, an operating system, and a Linux distribution.**

2. **What is the Linux kernel, and what responsibilities does it have between applications and the underlying hardware?**

3. **What is a Linux distribution? Explain why distributions such as Ubuntu, Debian, Fedora, Arch, and Red Hat-based systems can all use the Linux kernel while providing different user experiences and tools.**

4. **Why is Linux so widely used in servers, cloud infrastructure, networking equipment, cybersecurity tools, and embedded systems?**

5. **What are the major characteristics of Linux that make it useful from a cybersecurity perspective?**

6. **What is open-source software, and how does the open-source nature of Linux affect security, auditing, customization, and trust?**

7. **What is the difference between the Linux kernel and the shell? Why is understanding this distinction important?**

8. **What is a terminal, and how is a terminal different from a shell?**

9. **What is Bash? Why has Bash become one of the most important tools for Linux administration and security automation?**

10. **What advantages does command-line interaction provide to cybersecurity professionals compared with relying entirely on graphical interfaces?**

## B. Linux Filesystem

11. **Explain the Linux filesystem hierarchy. What is the purpose of directories such as `/`, `/bin`, `/etc`, `/home`, `/var`, `/tmp`, `/usr`, `/opt`, `/root`, and `/dev`?**

12. **Why does Linux use a single hierarchical filesystem beginning at `/` instead of drive letters such as `C:` and `D:`?**

13. **What is the root directory `/`, and why should it not be confused with the root user?**

14. **What is the purpose of `/etc`? Why is this directory particularly important during Linux security assessments?**

15. **What kinds of information would you expect to find under `/var`? Why can `/var/log` be valuable during security investigations?**

16. **What is `/home`, and how does it relate to normal users?**

17. **What is `/root`, and why is it different from `/` and `/home`?**

18. **What is `/tmp` used for? Why can temporary directories become relevant during security investigations?**

19. **What is `/dev`? Explain why Linux represents hardware and certain system interfaces through files or file-like objects.**

20. **Why is understanding the filesystem hierarchy important when investigating a potentially compromised Linux machine?**

## C. Files and Directories

21. **How do you create, copy, move, rename, and delete files and directories in Linux? Explain the purpose of the common commands used for these operations.**

22. **What is the difference between an absolute path and a relative path? Why does this distinction matter when writing scripts or investigating a system?**

23. **What do `.` and `..` represent in Linux paths? Why are these concepts important when navigating the filesystem?**

24. **What does the `ls` command provide, and what additional information can you obtain by using its common options?**

25. **What does `pwd` tell you, and why can knowing your current working directory be important when performing administrative or security operations?**

26. **What does `find` do? Why is it particularly useful for security investigations and system administration?**

27. **How would you locate files that were recently modified on a Linux system? Why could recently modified files be interesting during an investigation?**

28. **What is the difference between deleting a file and securely removing sensitive information? Why should a security professional understand that simply deleting a file may not mean the underlying data is immediately unrecoverable?**

29. **What are hidden files in Linux? Why might configuration files and security-relevant artifacts sometimes be hidden?**

30. **How could an attacker hide or disguise files on a Linux system, and what defensive techniques could help identify suspicious files?**

## D. Linux Permissions

31. **Why does Linux have a permission system? Explain how permissions protect files, directories, processes, and system resources.**

32. **Explain the Linux permission model using read, write, and execute permissions. What does each permission mean for a file?**

33. **How do read, write, and execute permissions behave differently when applied to a directory?**

34. **What do the owner, group, and others categories represent in Linux permissions?**

35. **How would you interpret a permission string such as `-rwxr-xr--`? Break it down completely.**

36. **What is numeric permission notation such as `755`, `644`, and `700`? How are these values derived?**

37. **Why is assigning `777` permissions to a file or directory generally considered dangerous? What security problem does excessive permission create?**

38. **What is the principle of least privilege, and how does Linux's permission model help implement it?**

39. **What is `chmod`? Explain how it can modify permissions and why careless use of it can create security problems.**

40. **What is `chown`? Why is file ownership an important security boundary?**

41. **What is `chgrp`? When would changing the group associated with a file be useful?**

42. **A sensitive configuration file is readable by every local user. Why could this be dangerous even if the file itself is not executable?**

## E. Users and Groups

43. **Why does Linux use separate user accounts instead of allowing everyone to operate as the root user?**

44. **What is the root user? Why does root have significantly greater capabilities than ordinary users?**

45. **What is a user ID (UID), and why does Linux use numeric identifiers internally for users?**

46. **What is a group ID (GID), and how do groups simplify permission management?**

47. **What are `/etc/passwd` and `/etc/shadow`? What type of information does each contain, and why is `/etc/shadow` more restricted?**

48. **Why is password information not normally stored as plaintext in `/etc/passwd` or `/etc/shadow`?**

49. **What is a service account? Why should service accounts generally have only the permissions required to perform their specific function?**

50. **What security risks can arise from unnecessary user accounts remaining on a Linux system?**

51. **How would you identify users currently configured on a Linux system? Which sources of information would you examine?**

52. **How would you identify which users currently have active sessions on a Linux system? Why could this information be useful during incident response?**

## F. sudo and Privilege

53. **What is `sudo`, and why is it preferable to routinely logging in as root for many administrative tasks?**

54. **How does `sudo` support the principle of least privilege?**

55. **What is the purpose of `/etc/sudoers` and the related sudo configuration? Why should this configuration be protected carefully?**

56. **What could happen if an ordinary user is accidentally granted unrestricted sudo access?**

57. **Why can excessive sudo permissions become a privilege-escalation risk?**

58. **What is the difference between authentication and authorization in the context of `sudo`?**

59. **If a security analyst discovers that a user can execute a sensitive administrative program through sudo, what questions should the analyst ask before determining whether this represents a privilege-escalation risk?**

60. **Why is monitoring privileged command execution important from a Blue Team and SOC perspective?**

## G. Processes

61. **What is a process? Explain the relationship between a running program, a process, and the Linux kernel.**

62. **What is a process ID (PID), and why is it useful when managing or investigating processes?**

63. **What is the difference between a parent process and a child process? Why can process relationships be valuable during security investigations?**

64. **What is a daemon? Why do Linux systems use background processes for services?**

65. **What is the difference between a foreground and background process?**

66. **How can you identify running processes on a Linux system? What information would you want to examine beyond simply knowing the process name?**

67. **Why might a security analyst investigate processes consuming unusually high CPU, memory, or other resources?**

68. **How could a malicious process attempt to disguise itself as a legitimate process? What evidence could help distinguish legitimate and suspicious processes?**

69. **Why is process ownership important when investigating suspicious activity?**

70. **If a suspicious process is running as root, why is that more concerning than the same process running under a restricted user account?**

## H. Services and System Management

71. **What is a system service? Explain how services differ from ordinary interactive applications.**

72. **What is systemd, and what role does it play in modern Linux systems?**

73. **How can you determine which services are currently running on a Linux machine?**

74. **Why should administrators disable services that are not required? Explain the relationship between unnecessary services and attack surface.**

75. **What is service persistence? Why might attackers attempt to configure malicious software to automatically start when a system boots?**

76. **What kinds of locations or mechanisms might Linux use to automatically start applications or services? Explain conceptually without relying on a particular attack technique.**

77. **How could a defender investigate a Linux system for suspicious services or unexpected startup mechanisms?**

78. **A server is running several network-facing services. How would you determine which services are necessary, which are unnecessary, and which may represent security risks?**

## I. Linux Networking

79. **How does a Linux system participate in a network? Explain the roles of network interfaces, IP addresses, routing tables, DNS configuration, and the default gateway.**

80. **How would you identify the network interfaces and IP addresses configured on a Linux system? Why is this information useful during both administration and security assessments?**

81. **What is a Linux routing table? How does the operating system use it to determine where packets should be sent?**

82. **How can you determine which ports are listening on a Linux system? Why is this information important from an attack-surface perspective?**

83. **What is the relationship between a listening port, a process, and a network service?**

84. **A Linux server has a service listening on `0.0.0.0` rather than `127.0.0.1`. What security difference might this create?**

85. **Why is it important to know whether a service is listening only locally, on an internal interface, or on all network interfaces?**

86. **How can Linux firewall mechanisms help control network access to services? What is the relationship between a firewall rule and a listening service?**

## J. Linux Shell and Command-Line Skills

87. **What are stdin, stdout, and stderr? Why are these streams important when working with Linux commands and security automation?**

88. **What is command piping? Explain how the output of one command can become the input of another and why this is powerful for security analysis.**

89. **What is output redirection? How can it be used to create files, append information, or capture command results?**

90. **What are environment variables? Why can variables such as `PATH`, `HOME`, and user-related environment information matter from a security perspective?**

91. **What is the `PATH` environment variable? How does the shell use it to locate executable programs?**

92. **Why can an insecure `PATH` configuration create security problems? Explain the concept without relying on a specific exploitation procedure.**

93. **What is command substitution? Why is understanding shell interpretation important when writing security scripts?**

94. **What is quoting in Bash? Why can incorrect handling of spaces, special characters, and variables cause commands or scripts to behave unexpectedly?**

95. **Why should cybersecurity professionals understand shell syntax rather than simply memorizing Linux commands?**

## K. Linux Logs

96. **Why are logs important on a Linux system? Explain their value for troubleshooting, monitoring, auditing, and incident response.**

97. **What kinds of information might appear in authentication-related logs? Why are failed and successful authentication events both valuable?**

98. **What is the purpose of system logs? What kinds of events might indicate that something unusual has happened on a Linux machine?**

99. **What is journald? How does it relate to systemd and modern Linux logging?**

100. **Imagine a Linux server is suspected of being compromised. Which categories of evidence would you examine first—users, authentication, processes, services, network connections, filesystem changes, scheduled tasks, and logs—and why?**

## L. Linux Security Reasoning

The first 100 questions establish the core. But because Linux is one of our **major cybersecurity foundations**, I want to add a second layer of scenario questions even at Level 1.

101. **A normal user suddenly has access to a sensitive configuration file containing credentials. What Linux concepts would you investigate to understand why the access was possible?**

102. **A server has several services running as root even though they do not appear to require administrative privileges. Why is this potentially dangerous?**

103. **A Linux server suddenly begins making outbound connections to an unfamiliar external system. What information would you collect from the operating system before deciding whether the behavior is malicious?**

104. **A new user account appears on a production server without an obvious business reason. What questions would you ask, and what Linux artifacts could help establish who created or used the account?**

105. **A process with an unfamiliar name is consuming significant CPU and running under root. How would you investigate it without immediately assuming that it is malicious?**

106. **A web server has been compromised. Which Linux areas would you examine to determine whether the attacker established persistence?**

107. **Why is file ownership and permission analysis important after a suspected compromise? What could unusual ownership or permissions reveal?**

108. **A Linux machine has a large number of failed SSH authentication attempts. What possibilities could explain this behavior, and what additional evidence would you collect?**

109. **A system administrator says, "The server is secure because SSH is protected by a password." Explain why this statement is incomplete and identify other Linux security controls that should be considered.**

110. **A Linux server contains sensitive application data, runs multiple network services, and is accessible from the Internet. Using only concepts learned in Level 1, explain how you would begin assessing its attack surface.**

## What Linux Level 1 Should Give You

By the end of this level, you should be able to mentally connect:

```text
                    LINUX
                      │
        ┌─────────────┴─────────────┐
        │                           │
     USERSPACE                    KERNEL
        │                           │
 ┌──────┼─────────┐                 │
 │      │         │                 │
Shell  Apps    Services          Hardware
 │      │         │
 │      │         └──── systemd
 │      │
 │      └──────── Files
 │
 └────────────── Commands
```

But from a cybersecurity perspective, that becomes:

```text
USER
  ↓
AUTHENTICATION
  ↓
AUTHORIZATION
  ↓
PROCESS
  ↓
FILE / RESOURCE
  ↓
NETWORK SERVICE
  ↓
LOG
  ↓
DETECTION
```

And eventually:

```text
ATTACKER
   ↓
Initial Access
   ↓
Linux Account
   ↓
Process
   ↓
Filesystem
   ↓
Privilege
   ↓
Persistence
   ↓
Network
   ↓
Lateral Movement
```

while the Blue Team sees:

```text
EVENT
  ↓
LOG
  ↓
PROCESS / USER / NETWORK
  ↓
ANALYSIS
  ↓
DETECTION
  ↓
CONTAINMENT
  ↓
RECOVERY
```

### Linux Level 1 Coverage

| Area                | Questions |
| ------------------- | --------: |
| Linux fundamentals  |        10 |
| Filesystem          |        10 |
| Files & directories |        10 |
| Permissions         |        12 |
| Users & groups      |        10 |
| sudo & privilege    |         8 |
| Processes           |        10 |
| Services            |         8 |
| Networking          |         8 |
| Shell               |         9 |
| Logs                |         5 |
| Security scenarios  |        10 |
| **Total**           |   **110** |

So we've deliberately exceeded our **50-question minimum** by a large margin.

### The standard we should use for answering these

Don't try to answer these like exam questions.

For our journey, a strong answer should increasingly follow:

**1. What?** — Define it
**2. Why?** — Why does Linux need it?
**3. How?** — How does it work?
**4. Example** — Show it in a real system
**5. Failure** — What happens when it breaks?
**6. Security** — Why does it matter?
**7. Red Team** — How could it become an attack surface?
**8. Blue Team** — How could we detect or prevent abuse?

That means even a basic question like **"What is Linux file permission?"** eventually becomes much more than memorizing `chmod 755`.

It becomes:

> **Who owns this resource? Who can access it? Why? What happens if permissions are wrong? Could sensitive information be exposed? Could an attacker abuse the permission boundary? How would we discover that? How would we fix it?**

That is exactly the type of thinking we want to carry forward into **Linux Level 2 → Linux Level 3 → Red Team → Blue Team → Active Directory**.
