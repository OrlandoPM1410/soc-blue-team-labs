# Case 03 – Suspicious Outbound Connection (IP:443) with Process Attribution

## Summary
Observed outbound connections to a specific external IP over HTTPS (443).
Correlated network telemetry (Sysmon EID 3) with process creation (Sysmon EID 1)
to identify the responsible process and execution context.

---

## Environment
- Endpoint: Windows 10 + Sysmon
- SIEM: Splunk Enterprise
- Sysmon log: `Microsoft-Windows-Sysmon/Operational`

---

## Detection Hypothesis
A suspicious outbound connection is more actionable when we can answer:
- **What process created the connection?**
- **What command line executed it?**
- **Was it spawned by a suspicious parent?**

---

## Evidence Collected
- Sysmon EID 3: outbound network connection(s) to `184.231.223.45:443`
- Sysmon EID 1: process creation events for the correlated ProcessGuid

---

## Detection (Splunk SPL)

### Network-only (indicator-focused)
```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=3 DestinationIp="184.231.223.45" DestinationPort=443
| stats count as net_events values(Image) as image values(User) as user by ProcessGuid, Computer, SourceIp
| sort - net_events
```

### Process + network correlation (enrichment)
```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" (EventCode=1 OR EventCode=3)
| eval is_target=if(EventCode=3 AND DestinationIp="184.231.223.45" AND DestinationPort=443,1,0)
| stats 
    max(is_target) as hit
    values(Image) as image
    values(CommandLine) as cmd
    values(User) as user
    values(ParentImage) as parent
    count(eval(EventCode=3)) as net_events
  by ProcessGuid, Computer, SourceIp
| where hit=1
| sort - net_events
```

---

## Triage Notes (Analyst Workflow)
- Confirm destination reputation / business context (was it expected?)
- Review `Image`:
  - Browser vs scripting interpreter vs LOLBin
- Review `CommandLine` for encoded/obfuscated patterns
- Review `ParentImage` to understand process chain
- Decide: allowlist / block / isolate / escalate

---

## Conclusion
Correlation provides high-value context for outbound indicators by attributing the
traffic to a process, reducing ambiguity and improving investigation speed.

---

## Improvements / Next Iteration
- Add allowlist for known browser processes (optional)
- Add thresholding (burst detection) for repeated connections
- Add enrichment fields (host role, asset tags)
