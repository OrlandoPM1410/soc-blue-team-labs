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
|------|----------|------------|------------|------------|------------|
| 01 | [Suspicious PowerShell execution](case-studies/case-01-suspicious-powershell) | MITRE ATT&CK T1059.001 | PowerShell abuse detection via (SPL + scheduled alert) (Event ID 1) | Implemented and validated | Windows 10 + Sysmon + Splunk Enterprise |


## Roadmap

- Add lateral movement case
- Add persistence detection case
- Add credential access detection
- Expand SIEM-based detections (SPL / KQL equivalents)

## Lab Architecture (Current)
- SIEM: Splunk (Host-Only: 192.168.171.14)
- Endpoint: Windows + Sysmon (Host-Only: 192.168.171.12)
- Attacker: Kali (Host-Only: 192.168.171.13)
- Networking: Host-Only (internal) + NAT (internet)

## Disclaimer

All scenarios were performed in a controlled lab environment.
