# Lab Architecture & Telemetry Pipeline

## 1. Purpose

This lab environment was built to understand and validate the complete detection lifecycle in a controlled SOC-like setup:

- Endpoint telemetry generation
- Log ingestion into SIEM
- Detection logic implementation
- Alert configuration
- Controlled validation

The objective is to work with real Windows telemetry instead of static datasets.

---

# 2. General Architecture

## High-Level Architecture

+--------------------------------------------------+
|                 Windows 10 VM                    |
|--------------------------------------------------|
|  - Sysmon installed                              |
|  - Windows Event Logging                         |
|  - Controlled attack simulation                  |
+---------------------------+----------------------+
                            |
                            | WinEventLog API
                            v
+--------------------------------------------------+
|               Splunk Enterprise (SIEM)           |
|--------------------------------------------------|
|  - WinEventLog ingestion                         |
|  - SPL detection logic                           |
|  - Scheduled alerts                              |
|  - Manual investigation                          |
+--------------------------------------------------+



This architecture simulates a minimal SOC monitoring pipeline.

---

# 3. Endpoint Configuration

## 3.1 Operating System

- Windows 10 (dedicated VM)
- Used exclusively for telemetry generation and attack simulation

---

## 3.2 Sysmon Deployment

Sysmon (System Monitor) is installed to enhance default Windows logging.

Telemetry source:

Microsoft-Windows-Sysmon/Operational

### Relevant Event Types Enabled

- Event ID 1 – Process Creation
- Parent-child process relationships
- Command-line logging
- User and integrity level context

Sysmon provides enriched visibility required for behavior-based detection.

---

# 4. Log Ingestion in Splunk

## 4.1 Input Configuration

Splunk ingests logs directly from the Sysmon channel using:

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index = main
disabled = 0


This configuration allows Splunk to subscribe to the Sysmon event channel via the Windows Event Log API.

---

## 4.2 Service Context

The Splunk service runs under:

- Local System account

This ensures proper permissions to read the Sysmon event channel.

---

# 5. Log Flow

The implemented telemetry flow is as follows:
Process Execution (e.g., PowerShell)
↓
Sysmon Event ID 1 generated
↓
Windows Event Log (Sysmon channel)
↓
Splunk WinEventLog input
↓
Indexed into "main"
↓
SPL detection logic
↓
Scheduled alert evaluation


This confirms a complete end-to-end monitoring pipeline.

---

# 6. Event Types Used for Detection

Current detection cases rely primarily on:

## Sysmon Event ID 1 – Process Creation

Key fields leveraged:

- Image
- CommandLine
- ParentImage
- ParentProcessId
- User
- IntegrityLevel
- ProcessId
- ComputerName

These fields allow behavioral analysis instead of static signature-based detection.

---

# 7. Detection Implementation Model

Detections in this lab follow a structured workflow:

1. Select MITRE ATT&CK technique
2. Identify suspicious behavioral indicators
3. Translate behavior into SPL query
4. Validate ingestion
5. Convert query into scheduled alert
6. Perform controlled execution for validation
7. Document limitations and future improvements

---

# 8. Example Detection – PowerShell Abuse

Technique:  
MITRE ATT&CK – T1059.001 (PowerShell)

Detection SPL:

index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=1
Image="powershell"
(CommandLine="-EncodedCommand" OR CommandLine="-ExecutionPolicy Bypass")


Alert Configuration:

- Type: Scheduled
- Frequency: Hourly
- Time range: Last 60 minutes
- Trigger condition: Number of results > 0
- Severity: Medium
- Scope: Shared in App

Validation is performed via controlled execution inside the Windows VM.

---

# 9. Current Limitations

- No behavioral correlation across multiple event types
- No network telemetry enrichment (Sysmon Event ID 3 not yet integrated)
- No baseline-based anomaly detection
- No threat intelligence enrichment
- Single-endpoint environment (no lateral movement simulation)

---

# 10. Planned Improvements

- Parent-child anomaly detection (Office → PowerShell)
- Network-based correlation (process + outbound connection)
- Encoded command entropy/length detection
- Sigma rule portability
- Multi-host simulation

---

This lab serves as a controlled environment to iteratively develop detection engineering skills and SOC analytical thinking.





