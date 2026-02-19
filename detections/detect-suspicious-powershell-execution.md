# Detection – Suspicious PowerShell Encoded Execution

## Technique Mapping

- MITRE ATT&CK T1059.001 – PowerShell
- MITRE ATT&CK T1027 – Obfuscated / Encoded Files or Information

---

## Objective

Detect suspicious PowerShell execution using:

- `-EncodedCommand`
- `-ExecutionPolicy Bypass`

This behavior is commonly associated with obfuscated or malicious script execution.

---

## Data Sources

- Sysmon Event ID 1 – Process Creation
- Log source: Microsoft-Windows-Sysmon/Operational
- SIEM: Splunk Enterprise

---

## Detection Logic (Splunk SPL)

### Basic EncodedCommand Detection

```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1 Image="*powershell.exe"
| search CommandLine="*-EncodedCommand*" OR CommandLine="*-ExecutionPolicy Bypass*"
| table _time Computer User Image CommandLine ParentImage
| sort - _time
```

---

## Detection Description

This query:

1. Filters Sysmon process creation events.
2. Identifies executions of `powershell.exe`.
3. Searches for suspicious flags commonly used for:
   - Obfuscation
   - Defense evasion
   - Malicious payload staging

---

## Why This Matters

Encoded PowerShell commands:

- Hide malicious intent
- Bypass execution restrictions
- Reduce analyst visibility
- Are frequently used in fileless attacks

Legitimate usage exists but is rare on standard user endpoints.

---

## Tuning Recommendations

- Allowlist known administrative automation scripts
- Add detection for:
  - `-nop`
  - `-w hidden`
  - `IEX`
  - `FromBase64String`
- Add base64 length thresholding for stronger signal

---

## Optional Enhancement – Add Network Correlation

To increase detection fidelity, correlate with Sysmon Event ID 3:

- Detect outbound connections initiated by the same ProcessGuid
- Flag PowerShell generating network traffic shortly after execution

---

## Analyst Triage Checklist

When alert triggers:

- Validate user context
- Review ParentImage
- Extract and decode Base64 string
- Determine business justification
- Check for related network activity

---

## Lab Context

- Windows 10 endpoint with Sysmon
- Splunk SIEM
- Controlled test using encoded PowerShell execution
