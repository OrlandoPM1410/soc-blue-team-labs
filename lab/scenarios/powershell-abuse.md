# Example Detection – PowerShell Abuse

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

