# Detecting and Investigating RDP Brute Force Attacks

## Overview

This project demonstrates the detection and investigation of a Remote Desktop Protocol (RDP) brute force attack within a SOC-style monitoring environment.

The lab simulates an attacker attempting repeated authentication attempts against a Windows Server system over RDP while security logs are centrally collected and analyzed using Splunk SIEM.

The objective of the project was to identify brute force behavior, generate alerts, analyze attack patterns, and demonstrate incident detection and response workflows commonly performed by Security Operations Center (SOC) analysts.

---

# Objectives

- Detect brute force authentication attempts targeting RDP services
- Monitor failed Windows authentication events
- Generate SIEM alerts for suspicious login activity
- Investigate attacker behavior through log analysis
- Demonstrate SOC investigation and response workflows

---

# Architecture

```text
Kali Linux Attacker (192.168.2.5)
            │
            ▼
      pfSense Firewall
            │
            ▼
Windows Server Target (192.168.1.1)
            │
            ▼
       Splunk SIEM
```

---

# Project Details

| Category | Details |
|---|---|
| Project Type | Security Monitoring & Incident Detection |
| Security Focus | RDP Brute Force Detection |
| MITRE ATT&CK | T1110 – Brute Force |
| SIEM Platform | Splunk |
| Firewall | pfSense |
| Target Protocol | Remote Desktop Protocol (RDP) |
| Target Port | 3389 |

---

# Lab Environment

## Attacker System

| Component | Details |
|---|---|
| Operating System | Kali Linux |
| IP Address | `192.168.2.5` |
| Network Zone | OPT1 (External) |

## Target System

| Component | Details |
|---|---|
| Operating System | Windows Server |
| IP Address | `192.168.1.1` |
| Exposed Service | RDP |
| Port | `3389` |
| Network Zone | LAN (Intrenal) |

## Tools Used
<p>

<img src="https://img.shields.io/badge/pfSense-212121?style=for-the-badge" />
<img src="https://img.shields.io/badge/Windows_Server-0078D6?style=for-the-badge&logo=windows&logoColor=white" />
<img src="https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white" />

<img src="https://img.shields.io/badge/Splunk_Enterprise-000000?style=for-the-badge&logo=splunk&logoColor=white" />
<img src="https://img.shields.io/badge/Nmap-00457C?style=for-the-badge" />
<img src="https://img.shields.io/badge/Hydra-8B0000?style=for-the-badge" />

</p>

---

# Attack Simulation

The attack simulation followed the steps below:

1. Network reconnaissance was performed using Nmap
2. RDP service exposure was identified on the target server
3. Hydra was used to perform brute force authentication attempts
4. Multiple failed login events were generated against the Administrator account
5. Authentication logs were ingested into Splunk for analysis

---

# Detection Methodology

Detection was performed using Splunk SIEM and Windows authentication logs.

The monitoring strategy focused on:
- High volumes of failed authentication attempts
- Repeated login failures from a single source IP
- Authentication attempts targeting privileged accounts
- RDP-related login activity

---

# Splunk Detection Query

```spl
index=main EventCode=4625
| stats count by src_ip, user
| where count >= 5
```

## Detection Logic

This query identifies:
- Failed Windows authentication attempts
- Multiple authentication failures from the same source
- Potential brute force behavior against targeted accounts

---

# Investigation Findings

The activity matched brute force attack behavior due to:

- Repeated failed authentication attempts
- Consistent source IP activity
- Continuous targeting of RDP services
- Multiple attempts against the same account
- Authentication threshold violations

The investigation confirmed suspicious authentication behavior consistent with MITRE ATT&CK technique T1110.

---

# Security Impact Assessment

## Confidentiality Impact

- Unauthorized remote access to systems
- Credential compromise risk
- Potential exposure of sensitive information

## Integrity Impact

- Possible system configuration modification
- Deployment of malicious payloads
- Persistence establishment

## Availability Impact
- Operational disruption
- Potential downtime of remote services

---

# Containment and Remediation

The following remediation actions were identified:

- Block malicious source IP addresses
- Restrict external RDP exposure
- Enforce strong password policies
- Reset compromised credentials
- Enable account lockout protections
- Monitor failed authentication events continuously
- Validate system integrity after incident response

---

# Skills Demonstrated

- SIEM Monitoring
- Threat Detection
- Log Analysis
- Security Event Correlation
- Incident Investigation
- SOC Operations
- Windows Event Analysis
- Splunk Query Development
- Firewall Traffic Analysis
- MITRE ATT&CK Mapping

  ---
  
# Lessons Learned

- Exposed RDP services significantly increase attack surface
- Failed authentication monitoring is critical for early detection
- SIEM correlation rules improve detection efficiency
- Strong password policies reduce brute force success rates
- Centralized logging enhances visibility and investigation capability





