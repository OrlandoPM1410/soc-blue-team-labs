# Scenario – Suspicious PowerShell Execution (EncodedCommand)

## Technique

**MITRE ATT&CK – T1059.001 (PowerShell)**  
**MITRE ATT&CK – T1027 (Obfuscated/Encoded Files or Information)**

---

## Objective

Simulate suspicious PowerShell execution using the `-ExecutionPolicy Bypass`
and `-EncodedCommand` flags to validate:

- Sysmon process creation logging
- Command-line visibility
- Detection logic in Splunk

---

## Attack Execution

Executed on Windows endpoint:

```powershell
powershell -ExecutionPolicy Bypass -EncodedCommand <BASE64_STRING>
```

Example test payload:

```powershell
powershell -ExecutionPolicy Bypass -EncodedCommand ZQBjAGgAbwAgACIASABlAGwAbABvACIA
```

This decodes to:

```
echo "Hello"
```

---

## Telemetry Source

- Sysmon Event ID 1 – Process Creation
- Log: Microsoft-Windows-Sysmon/Operational
- SIEM: Splunk

---

## Detection Logic (SPL)

### Basic detection

```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1 Image="*powershell.exe"
| search CommandLine="*-EncodedCommand*" OR CommandLine="*-ExecutionPolicy Bypass*"
| table _time Computer User Image CommandLine ParentImage
| sort - _time
```

---

## Expected Behavior

- Sysmon EID 1 logged
- Image: powershell.exe
- CommandLine includes:
  - -ExecutionPolicy Bypass
  - -EncodedCommand

---

## Validation

1. Execute test command
2. Confirm Sysmon EID 1 ingestion
3. Run detection query
4. Verify command-line visibility in Splunk

---

## Detection Rationale

Encoded PowerShell execution is frequently associated with:

- Malware loaders
- Fileless attacks
- Initial access payloads
- Defense evasion techniques

Legitimate use exists but is rare on workstations.

---

## Limitations

- Admin scripts may trigger false positives
- Requires proper Sysmon command-line logging
- Base64 payload content is not automatically decoded

