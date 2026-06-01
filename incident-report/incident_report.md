# Incident Response Investigation Report

## Executive Summary

A Security Operations Center (SOC) investigation was conducted following suspicious Secure Shell (SSH) authentication activity observed on an Ubuntu Linux system.

Analysis of Linux authentication logs identified multiple failed SSH authentication attempts originating from a single source IP address, followed by successful authentication to a valid user account. Subsequent activity included privileged command execution through `sudo` and creation of a new local account.

The investigation successfully reconstructed the attack timeline, identified Indicators of Compromise (IOCs), and documented activity consistent with authentication abuse, privilege escalation, and persistence-related behavior.

---

## Incident Overview

| Category | Details |
|-----------|-----------|
| Incident Type | Unauthorized Access Investigation |
| Detection Source | Splunk Enterprise |
| Log Source | Linux Authentication Logs |
| Target Host | `matt-flores-VirtualBox` |
| Source IP | `192.168.1.35` |
| Severity | Medium |

---

## Investigation Scope

The investigation focused on:

- Failed SSH authentication attempts
- Successful SSH authentication events
- User session establishment
- Privileged command execution
- Local account creation activity
- Timeline reconstruction
- IOC identification

---

## Timeline of Events

| Time | Event |
|--------|--------|
| 14:17:00 | Failed SSH login attempt against `admin` |
| 14:17:06 | Failed SSH login attempt against `admin` |
| 14:17:10 | Failed SSH login attempt against `admin` |
| 14:17:29 | Failed SSH login attempt against `root` |
| 14:17:34 | Failed SSH login attempt against `root` |
| 14:17:59 | Failed SSH login attempt against `test` |
| 14:35:37 | Successful SSH authentication for `matt-flores` |
| 15:25:08 | Privileged command executed via `sudo` |
| 15:36:58 | `useradd backupsvc` executed |
| 15:36:59 | `backupsvc` account created |

---

## Indicators of Compromise (IOCs)

| IOC Type | Value |
|-----------|-----------|
| Source IP Address | `192.168.1.35` |
| Targeted Accounts | `admin`, `root`, `test` |
| Authenticated Account | `matt-flores` |
| Privileged Command | `sudo whoami` |
| Persistence Command | `useradd backupsvc` |
| Created Account | `backupsvc` |

---

## Investigation Findings

### Finding 1: Multiple Failed Authentication Attempts

Authentication logs showed repeated failed SSH login attempts targeting common usernames including `admin`, `root`, and `test`. This activity is consistent with username enumeration and credential guessing behavior.

### Finding 2: Successful Authentication Achieved

Following the failed login attempts, successful authentication to the `matt-flores` account was observed from the same source IP address.

### Finding 3: Privileged Access Obtained

The authenticated account executed commands through `sudo`, resulting in root-level command execution and privileged session activity.

### Finding 4: Persistence-Related Activity Identified

A new local account named `backupsvc` was created after privileged access was obtained. Creation of new accounts is a commonly monitored persistence technique because it can enable continued access to a system.

---

## Severity Assessment

**Medium Severity**

The investigation confirmed successful authentication, privileged command execution, and local account creation activity. While no evidence of malware deployment, lateral movement, or data exfiltration was identified, the observed activity demonstrated multiple stages of an attack lifecycle and warranted further investigation.

---

## Recommendations

1. Review and validate all newly created local accounts.
2. Investigate repeated authentication failures targeting privileged accounts.
3. Restrict SSH access to approved hosts where possible.
4. Monitor `sudo` activity and account-management commands.
5. Implement Multi-Factor Authentication (MFA) for remote administrative access.
6. Conduct periodic reviews of authentication logs for anomalous behavior.

---

## Report Conclusion

The investigation successfully reconstructed the sequence of events from initial failed authentication attempts through account creation activity. Analysis identified evidence of authentication abuse, privileged command execution, and persistence-related behavior originating from a single source IP address.
