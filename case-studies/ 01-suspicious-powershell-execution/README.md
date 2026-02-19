# Case 01 – Suspicious PowerShell Execution (EncodedCommand)

## Summary

Detected suspicious PowerShell execution using:

- -ExecutionPolicy Bypass
- -EncodedCommand

Technique mapped to **MITRE ATT&CK T1059.001**.

---

## Environment

- Windows 10 endpoint
- Sysmon installed
- Splunk SIEM
- Log source: Microsoft-Windows-Sysmon/Operational

---

## Attack Simulation

```powershell
powershell -ExecutionPolicy Bypass -EncodedCommand ZQBjAGgAbwAgACIASABlAGwAbABvACIA
```

---

## Evidence Collected

### Sysmon Event ID 1 – Process Creation

Key fields:

- Image: powershell.exe
- CommandLine: includes encoded content
- ParentImage: explorer.exe (example)
- User: logged-in user

---

## Detection (SPL)

```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1 Image="*powershell.exe"
| search CommandLine="*-EncodedCommand*" OR CommandLine="*-ExecutionPolicy Bypass*"
| table _time Computer User Image CommandLine ParentImage
| sort - _time
```

---

## Analyst Triage Workflow

1. Confirm user context (expected admin activity?)
2. Review parent process
3. Extract Base64 string
4. Decode payload offline
5. Determine malicious vs legitimate usage

---

## Risk Assessment

- Encoded commands reduce visibility
- Often used for malware staging
- High-confidence suspicious on user workstations

---

## Conclusion

Suspicious PowerShell execution identified successfully.
Command-line logging enabled detection.

---

## Improvements / Next Iteration

- Add detection for:
  - `-nop`
  - `-w hidden`
  - `IEX`
- Add correlation with network events (EID 3)
- Add base64 length thresholding
