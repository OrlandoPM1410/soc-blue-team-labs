# SOC Blue Team Labs

## Content Overview
- [Lab Environment](lab)
- [Case Studies](case-studies)
- [Detections](detections)

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-Active%20Development-green)

Hands-on Blue Team / SOC case studies focused on detection engineering, investigation, and incident response using Windows telemetry.  
This project is part of my cybersecurity learning path focused on Blue Team and SOC analyst skills.

## Objectives
This repository demonstrates:
- Log-based detection using Windows Event Logs and Sysmon
- SOC triage and investigation methodology
- Detection engineering (SPL-based logic, Sigma-style methodology)
- Mapping to MITRE ATT&CK
- Incident response decision-making

## Lab Environment
- Windows 10 endpoint with Sysmon installed
- Microsoft-Windows-Sysmon/Operational telemetry
- Local Splunk Enterprise instance (SIEM)
- SPL-based detection logic
- Scheduled alert configuration and validation

## Repository Structure
- `/lab` → Lab environment setup and configuration
- `/case-studies` → Simulated SOC investigations
- `/detections` → Detection rules and logic

## Case Studies

| Case | Scenario | Techniques | Detections | Status | Environment |
|------|----------|------------|------------|--------|------------|
| 01 | [Suspicious PowerShell execution](case-studies/01-suspicious-powershell-execution) | MITRE ATT&CK T1059.001 / T1027 | **TBD** (add PowerShell detection doc) | Implemented and validated | Windows 10 + Sysmon + Splunk Enterprise |
| 02 | [Internal Network Discovery](case-studies/02-internal-network-discovery) | MITRE ATT&CK T1046 | [Process + Network Correlation](detections/detect-process-network-correlation.md) | Implemented and validated | Windows 10 + Sysmon + Splunk Enterprise |
| 03 | [Suspicious Outbound Connection (IP:443)](case-studies/03-suspicious-outbound-connection) | MITRE ATT&CK T1071 / T1059 | [Process + Network Correlation](detections/detect-process-network-correlation.md) | Implemented and validated | Windows 10 + Sysmon + Splunk Enterprise |

## Roadmap
- Add lateral movement case
- Add persistence detection case
- Add credential access detection
- Expand SIEM-based detections (SPL / KQL equivalents)

## Lab Architecture (Current)
- Endpoint + SIEM: Windows 10 + Sysmon + Splunk (Host-Only: 192.168.171.12)
- Attacker: Kali Linux (Host-Only: 192.168.171.13)
- Networking:
  - Host-Only (192.168.171.0/24) → internal attack simulation
  - NAT → internet access for updates and tools

## Architectural Decision: SIEM Deployment Model
In this laboratory environment, Splunk is installed on the same Windows machine that acts as the monitored endpoint.

### Rationale (Lab Context)
For a personal SOC laboratory, this approach is:
- ✔ Resource-efficient (fewer virtual machines required)
- ✔ Simpler to manage
- ✔ Easier to maintain and troubleshoot
- ✔ Suitable for endpoint-focused detection development

### Real-World Considerations
In enterprise environments, SIEM solutions are typically deployed on:
- Dedicated servers
- Distributed architectures
- Separate ingestion and search heads

This lab prioritizes reproducibility and hardware efficiency while maintaining detection engineering realism.

## Disclaimer
All scenarios were performed in a controlled lab environment.
