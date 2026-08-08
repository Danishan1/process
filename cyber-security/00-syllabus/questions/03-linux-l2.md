# Linux — Level 2

### Intermediate Question Bank

**Target: 110 questions**

## A. Linux Boot Process & System Architecture

1. **Walk through the Linux boot process from the moment a machine is powered on until a user receives a working shell. What roles do firmware, bootloaders, the kernel, init/systemd, and services play?**

2. **What is GRUB, and what role does it play during Linux startup? Why can bootloader configuration become security-sensitive?**

3. **What happens when the Linux kernel is loaded into memory? What major responsibilities does the kernel begin handling?**

4. **What is `systemd`'s role during system initialization? How does it determine which services and targets should be started?**

5. **What are systemd targets? How do they relate conceptually to different system operating states?**

6. **What is single-user or rescue mode? Why can recovery modes be both useful for administrators and sensitive from a security perspective?**

7. **What happens when a Linux system fails during boot? How would you determine whether the problem is related to the bootloader, kernel, filesystem, or userspace services?**

8. **Why is understanding the boot process useful when investigating persistence mechanisms or unauthorized modifications to a Linux system?**

9. **What security risks could arise if an attacker gains the ability to modify boot configuration or influence early system startup?**

10. **How would you investigate a Linux machine that suddenly starts a previously unknown service during every boot?**

## B. Processes & Process Management

11. **Explain the Linux process lifecycle from creation to termination. What happens conceptually when one process creates another?**

12. **What are process states such as running, sleeping, stopped, and zombie? Why might these states matter during troubleshooting?**

13. **What is a zombie process? Why does a zombie process consume a process-table entry even though it is no longer executing?**

14. **What is an orphan process? How does Linux handle a process whose parent terminates unexpectedly?**

15. **What is process priority? How can priority affect system performance and resource allocation?**

16. **What is the Linux `nice` value? Why might administrators adjust process priority?**

17. **What is a signal in Linux? Explain the conceptual purpose of signals such as termination, interruption, and forced termination.**

18. **Why should a security analyst understand the difference between gracefully terminating a process and forcibly terminating it?**

19. **How would you investigate a process that continuously respawns after being terminated? What mechanisms might be responsible?**

20. **A suspicious process is running under a legitimate service account. What evidence would you collect to determine whether the process is legitimate or malicious?**

## C. `/proc` and System Visibility

21. **What is the `/proc` filesystem? Why is it fundamentally different from an ordinary directory containing persistent files?**

22. **How can `/proc` provide information about running processes?**

23. **What kinds of system information can administrators learn from `/proc`?**

24. **Why can `/proc` be valuable during Linux incident response?**

25. **How could process information from `/proc` help connect a suspicious network connection to a running process?**

26. **Why should security analysts be careful about assuming that every piece of information visible through `/proc` represents persistent data on disk?**

27. **What is `/sys`? How does it differ conceptually from `/proc`?**

28. **Why is understanding kernel-exposed system information useful when investigating hardware, drivers, processes, and system configuration?**

## D. Storage & Filesystems

29. **What is a filesystem? Explain why Linux supports multiple filesystem types rather than relying on a single universal format.**

30. **What are ext4, XFS, and Btrfs at a high level? What kinds of considerations influence filesystem selection?**

31. **What is a mount point? Explain how a filesystem becomes accessible through the Linux directory hierarchy.**

32. **What is the difference between mounting a filesystem and merely having a storage device connected to a machine?**

33. **What information is typically described in `/etc/fstab`? Why can incorrect entries cause boot or availability problems?**

34. **What is an inode? What information does an inode contain, and why is it important when analyzing files?**

35. **How can a file have multiple names while referring to the same underlying filesystem object? Explain the concept of hard links.**

36. **What is a symbolic link? How does it differ from a hard link?**

37. **What security considerations arise when sensitive files are accessible through symbolic links or unexpected filesystem paths?**

38. **What happens when a filesystem becomes full? Why can disk exhaustion become both an availability and security problem?**

## E. File Permissions — Deeper

39. **How do Linux permissions behave differently for files and directories, and why is this distinction important when assessing access?**

40. **What are special permissions such as SUID, SGID, and the sticky bit? Explain their intended purposes.**

41. **Why can SUID executables be security-sensitive? What capability does SUID provide to a program?**

42. **How would you identify files with unusual or elevated permissions during a security assessment?**

43. **What security risks can arise when a privileged program is writable by an unprivileged user?**

44. **How does SGID behave differently when applied to a file versus a directory?**

45. **What is the sticky bit, and why is it commonly relevant to shared temporary directories?**

46. **What are POSIX ACLs? Why might ACLs be used when traditional owner/group/other permissions are insufficient?**

47. **How can ACLs make permission analysis more complicated during an investigation?**

48. **A file appears to have restrictive traditional permissions, but a user can still access it. What mechanisms would you investigate?**

## F. User & Group Management

49. **How does Linux authenticate a user when they attempt to log in? Describe the major components involved without reducing authentication to simply "checking a password."**

50. **What is PAM? Why is the Pluggable Authentication Modules framework important to Linux authentication and authorization?**

51. **How can PAM allow different applications to use centralized authentication policies?**

52. **What security consequences can arise from poorly configured PAM policies?**

53. **What is account expiration? Why might organizations configure expiration policies for users or service accounts?**

54. **Why should dormant accounts be reviewed regularly? What risks do they create?**

55. **What is group-based privilege management? Why is assigning permissions to groups often preferable to managing users individually?**

56. **How would you investigate a situation where a user unexpectedly becomes a member of a privileged group?**

57. **Why should service accounts generally avoid interactive login access?**

58. **What information would you examine when investigating whether a suspicious account was created legitimately?**

## G. SSH

59. **What is SSH, and why is it one of the most important remote administration protocols in Linux environments?**

60. **Explain the major security properties provided by SSH: confidentiality, integrity, authentication, and secure remote administration.**

61. **What is the difference between password-based and public-key authentication in SSH?**

62. **How does SSH public-key authentication work conceptually? Why does the server not need to know the user's private key?**

63. **What is an SSH host key? Why should users and administrators care when the host key unexpectedly changes?**

64. **What security risks can arise from poorly configured SSH access? Consider authentication, exposed accounts, excessive privileges, and network exposure.**

65. **Why is restricting SSH access by network location or firewall policy useful even when strong authentication is already enabled?**

66. **How would you investigate repeated failed SSH authentication attempts? What information would you collect before deciding whether it is an attack?**

67. **What is SSH tunneling at a conceptual level? Why can legitimate tunneling capabilities become relevant to both Red Team operations and Blue Team detection?**

68. **How could an attacker who compromises an SSH-accessible account potentially use that access to move deeper into an environment? What controls can reduce the risk?**

## H. Linux Networking — Intermediate

69. **Explain how a Linux host determines whether a destination is local, belongs to another subnet, or should be reached through the default gateway.**

70. **What is the difference between a Layer 2 network interface and a Layer 3 IP configuration on Linux?**

71. **What is a loopback interface? Why is `127.0.0.1` fundamentally different from an address reachable from another machine?**

72. **What is a network namespace? Why are network namespaces useful in containers and security research?**

73. **What is a virtual network interface? Give examples of why Linux might have interfaces that do not correspond directly to physical network cards.**

74. **How would you determine which process owns a particular listening TCP or UDP port? Why is this relationship important during security investigations?**

75. **A service is listening on TCP port 8080 but users cannot reach it remotely. What layers and controls would you investigate?**

76. **A service is accessible from the local machine but not from another machine on the network. What does that tell you about the possible problem?**

77. **How can Linux firewall rules affect the apparent availability of a service even when the application itself is functioning correctly?**

78. **Why should administrators distinguish between application-level connectivity problems and network-level connectivity problems?**

## I. Linux Firewalling

79. **What is the purpose of a host-based firewall? How does it complement network firewalls?**

80. **What are the conceptual roles of packet filtering, connection tracking, and stateful firewalling on Linux?**

81. **What are `iptables` and `nftables`? Why has Linux firewall configuration evolved from older tooling toward nftables?**

82. **What is a default-deny firewall policy? Why is it generally safer than allowing arbitrary traffic and blocking only known malicious traffic?**

83. **How would you design host firewall rules for a Linux web server that should accept HTTPS but should not expose unnecessary administrative services?**

84. **Why should outbound traffic from Linux servers also be considered when designing firewall policies?**

85. **How can host-based firewall logs assist a SOC analyst during an investigation?**

86. **A service works locally but becomes inaccessible after a firewall configuration change. How would you troubleshoot the issue without blindly disabling the firewall?**

## J. Package Management & Software

87. **Why do Linux distributions use package managers? Explain how package managers simplify installation, updates, dependency management, and removal.**

88. **What are repositories? Why should organizations carefully control which software repositories their servers trust?**

89. **What is a software dependency? Why can dependency management become a security issue?**

90. **Why are security updates important even when a vulnerable package is not directly exposed to the Internet?**

91. **What is patch management? Why is simply installing updates manually on individual servers not an ideal enterprise strategy?**

92. **How would you determine whether a Linux server is running outdated software? What information would you collect?**

93. **Why can unsupported or end-of-life software create significant security risk?**

94. **What is software provenance? Why should security teams care about where installed software came from?**

95. **How could a malicious or compromised package repository become a supply-chain security problem?**

## K. Bash & Automation

96. **Why is shell scripting important for Linux administrators and cybersecurity professionals?**

97. **What makes a good Bash script different from a sequence of commands typed manually?**

98. **What are variables, conditionals, loops, functions, and exit codes in Bash? Explain why each is useful in automation.**

99. **Why should scripts check whether commands succeed or fail instead of assuming that every command completed correctly?**

100. **What is idempotence in system administration? Why is it useful when writing automation that may be executed repeatedly?**

101. **Why can poorly written shell scripts create security vulnerabilities? Consider input handling, permissions, temporary files, environment variables, and command execution.**

102. **How could a Bash script be used defensively to identify unusual users, permissions, processes, services, or network listeners?**

103. **Design the logic for a script that generates a basic Linux security inventory. What information should it collect?**

104. **How would you make an administrative script safer so that it does not accidentally modify or delete the wrong files?**

## L. Logs & Monitoring

105. **How should a Linux administrator distinguish between logs useful for authentication, system operation, applications, networking, and security events?**

106. **Why is centralized logging important when an organization has hundreds or thousands of Linux systems?**

107. **What is log rotation? Why is it necessary for maintaining reliable logging over long periods?**

108. **How can an attacker attempt to interfere with local logs, and why should defenders avoid relying exclusively on logs stored on the potentially compromised machine?**

109. **What Linux events would you consider high-value telemetry for a SOC? Explain why authentication, privilege changes, process creation, service changes, file changes, and network connections can all matter.**

110. **A Linux server is suspected of compromise. Build an investigation sequence using the knowledge from Level 1 and Level 2. Explain how you would move from users and authentication to processes, persistence, files, network activity, logs, and containment.**

## The Core Level 2 Mental Model

At Level 1, we learned:

> **What is Linux?**

Level 2 should make you think:

> **What is this machine doing right now, why is it doing it, and is that behavior legitimate?**

A Linux server should start looking like this:

```text
                    LINUX HOST
                        │
        ┌───────────────┼────────────────┐
        │               │                │
      USERS           PROCESSES        NETWORK
        │               │                │
   UID / GID        PID / PPID       IP / Port
        │               │                │
   Groups           Services          Sockets
        │               │                │
   sudo/PAM         systemd          Firewall
        │               │                │
        └───────────────┼────────────────┘
                        │
                     FILESYSTEM
                        │
              ┌─────────┼─────────┐
              │         │         │
          Ownership  Permissions  ACL
              │         │         │
              └─────────┼─────────┘
                        │
                      LOGS
                        │
                  Investigation
```

But now add the attacker:

```text
                    ATTACKER
                       │
                 Initial Access
                       ↓
                    Account
                       ↓
                   Process
                       ↓
                  Persistence
                       ↓
                 Privilege
                       ↓
                  Discovery
                       ↓
                Network Access
                       ↓
              Lateral Movement
```

And the defender:

```text
                    SOC
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
       Users      Processes   Network
          │          │          │
          └──────────┼──────────┘
                     ↓
                   Logs
                     ↓
                 Detection
                     ↓
               Investigation
                     ↓
                Containment
```

That intersection is exactly why Linux is so important for our roadmap.

## What Changes from Level 1 → Level 2?

| Level 1               | Level 2                                                   |
| --------------------- | --------------------------------------------------------- |
| What is a process?    | How do I investigate a suspicious process?                |
| What are permissions? | How do permissions create or prevent security boundaries? |
| What is a user?       | How do authentication and authorization interact?         |
| What is SSH?          | How do I secure and investigate SSH access?               |
| What is a service?    | How do I identify unexpected services and persistence?    |
| What is a port?       | Which process owns the port and why is it exposed?        |
| What is a firewall?   | How do host firewall rules affect attack surface?         |
| What are logs?        | How do I reconstruct activity from logs?                  |
| What is Bash?         | How do I automate security analysis safely?               |
| What is Linux?        | **How do I assess the security of a Linux system?**       |

### The most important transition

By the end of Level 2, when someone gives you a Linux server, you shouldn't just think:

> "What commands do I know?"

You should think:

> **Who can access this machine?**

> **What is running?**

> **What is exposed?**

> **What privileges exist?**

> **What services are enabled?**

> **What is communicating over the network?**

> **What changed recently?**

> **What is persistent?**

> **What is logged?**

> **What shouldn't be here?**

> **If this machine were compromised, where would I look first?**

That mindset is the bridge into **Linux Level 3**, where we'll go substantially deeper into **hardening, advanced permissions, capabilities, namespaces/containers, kernel security, system internals, persistence mechanisms, privilege-escalation concepts, Linux forensics, advanced logging, attack paths, and enterprise Linux security**—which will directly support our eventual **Red Team / Pentesting > Blue Team / SOC** priority.
