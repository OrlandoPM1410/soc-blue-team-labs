# SOC Blue Team Labs

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-Active%20Development-green)

Hands-on Blue Team / SOC case studies focused on detection engineering, investigation, and incident response using Windows telemetry.

## Objectives

This repository demonstrates:

- Log-based detection using Windows Event Logs and Sysmon
- SOC triage and investigation methodology
- Detection engineering (Sigma-style logic)
- Mapping to MITRE ATT&CK
- Incident response decision-making

## Lab Environment

- Windows endpoint with Sysmon installed
- Windows Event Logs
- Local log collection
- Manual log analysis (expandable to SIEM in future)

## Repository Structure

- `/lab` → Lab environment setup and configuration
- `/case-studies` → Simulated SOC investigations
- `/detections` → Detection rules and logic

## Case Studies

| Case | Scenario | Techniques | Detections |
|------|----------|------------|------------|
| 01 | [Suspicious PowerShell execution](case-studies/case-01.md) | T1059.001 | PowerShell encoded command detection |



## Roadmap

- Add lateral movement case
- Add persistence detection case
- Add credential access detection
- Include SIEM-based queries (KQL/SPL)

## Disclaimer

All scenarios were performed in a controlled lab environment.
