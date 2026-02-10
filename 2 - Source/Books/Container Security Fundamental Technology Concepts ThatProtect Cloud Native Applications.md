
#computer-science #cloud-native #docker #security
[[2026-02-10]]

Table of Contents
Preface. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . xi
1. Container Security Threats. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1
Risks, Threats, and Mitigations 2
Container Threat Model 3
Security Boundaries 7
Multitenancy 8
Shared Machines 9
Virtualization 9
Container Multitenancy 10
Container Instances 11
Security Principles 11
Least Privilege 11
Defense in Depth 12
Reducing the Attack Surface 12
Limiting the Blast Radius 12
Segregation of Duties 12
Applying Security Principles with Containers 12
Summary 13
2. Linux System Calls, Permissions, and Capabilities. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 15
System Calls 15
File Permissions 17
setuid and setgid 18
Security Implications of setuid 21
Linux Capabilities 21
Privilege Escalation 23
Summary 24

