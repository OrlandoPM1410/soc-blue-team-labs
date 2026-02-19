# Case 01 – Suspicious PowerShell Execution (EncodedCommand)

## Summary

Suspicious PowerShell execution detected using:

- -ExecutionPolicy Bypass
- -EncodedCommand

Mapped to:

- MITRE ATT&CK T1059.001 – PowerShell
- MITRE ATT&CK T1027 – Obfuscated / Encoded Files

---

## Environment

- Windows 10 endpoint
- Sysmon installed
- Splunk Enterprise (local SIEM)
- Log source: Microsoft-Windows-Sysmon/Operational

---

## Attack Simulation

Executed on the Windows endpoint:

```powershell
powershell -ExecutionPolicy Bypass -EncodedCommand ZQBjAGgAbwAgACIASABlAGwAbABvACIA
```

Decoded content:

```
echo "Hello"
```

---

## Telemetry Collected

### Sysmon Event ID 1 – Process Creation

Key fields observed:

- Image: powershell.exe
- CommandLine: contains encoded command
- ParentImage: explorer.exe (example)
- User: interactive logged-in user

---

## Detection Logic (Splunk SPL)

```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1 Image="*powershell.exe"
| search CommandLine="*-EncodedCommand*" OR CommandLine="*-ExecutionPolicy Bypass*"
| table _time Computer User Image CommandLine ParentImage
| sort - _time
```

---

## Analyst Triage Workflow

1. Validate user context (admin or normal user?)
2. Review ParentImage
3. Extract Base64 string
4. Decode payload offline
5. Determine malicious vs legitimate usage

---

## Risk Assessment

Encoded PowerShell execution is commonly associated with:

- Fileless malware
- Initial access payloads
- Defense evasion
- Obfuscated script execution

Legitimate usage is uncommon on standard workstations.

---

## Conclusion

The detection successfully identified suspicious PowerShell execution based on command-line analysis.

Sysmon command-line logging enabled accurate attribution and investigation.

---

## Improvements / Future Enhancements

- Add detection for:
  - -nop
  - -w hidden
  - IEX
- Add correlation with Sysmon Event ID 3 (network activity)
- Add base64 length thresholding
