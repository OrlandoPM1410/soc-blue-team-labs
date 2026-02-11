# Lab Overview

This lab represents a small-scale SOC environment designed to practice detection, investigation, and incident response workflows.

## Environment
- Endpoint: Windows 10 virtual machine
- Telemetry: Windows Event Logs and Sysmon
- Analysis: Local analysis and SIEM-based queries (Splunk)

## Objectives
- Generate meaningful security telemetry
- Detect suspicious activity using log-based signals
- Perform structured investigations following SOC workflows
- Document findings and response decisions

## Scope
All scenarios are simulated in a controlled environment for learning and professional development purposes.


## Endpoint Telemetry – Sysmon

To enhance visibility beyond native Windows logging, Sysmon (System Monitor) was deployed on the Windows 10 endpoint.

### Objective
The objective was to generate detailed endpoint telemetry suitable for detection engineering and incident investigation.

### Configuration
Sysmon was installed using a community-based defensive configuration focused on:

- Process creation (Event ID 1)
- Network connections (Event ID 3)
- Parent-child process relationships
- Command-line arguments

This configuration ensures sufficient behavioral visibility for SOC-level analysis.

### Validation
Telemetry was validated by executing test commands (e.g., notepad.exe, whoami, PowerShell) and confirming that Event ID 1 entries were generated under:

Microsoft-Windows-Sysmon/Operational

This confirmed that the endpoint was producing enriched telemetry ready for SIEM ingestion.

## SIEM Integration – Splunk

After validating endpoint telemetry with Sysmon, a local instance of Splunk Enterprise (Trial license) was deployed to simulate a minimal SOC ingestion and analysis workflow.

### Objective
The goal was to establish an end-to-end log pipeline:

Endpoint → Sysmon → Windows Event Log → Splunk → Search & Analysis

### Log Ingestion Configuration
Sysmon telemetry was ingested into Splunk by configuring a local Windows Event Log input:

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = main

The Splunk service was configured to run under the Local System account to ensure proper permissions to subscribe to the Sysmon event channel.

### Validation
Ingestion was validated by:
- Restarting the Splunk service
- Generating new process activity (e.g., PowerShell, whoami, notepad)
- Confirming successful indexing via:

index=main

All indexed events were confirmed to originate from:
WinEventLog:Microsoft-Windows-Sysmon/Operational

This confirmed that the telemetry pipeline was functioning correctly and that the environment was ready for detection and investigation scenarios.

