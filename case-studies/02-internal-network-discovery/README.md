# Case 02 – Internal Network Discovery (Nmap Ping Sweep)

## Summary
Detected internal reconnaissance behavior consistent with **MITRE ATT&CK T1046**.
High volume of network connections was observed from a single internal source.

---

## Environment
- Endpoint: Windows 10 + Sysmon + Splunk (SIEM)
- Sysmon log: `Microsoft-Windows-Sysmon/Operational`
- Attacker (Kali): `192.168.171.13`
- Endpoint (Victim): `192.168.171.12`
- Network: Host-Only `192.168.171.0/24`

---

## Attack Simulation
Executed on Kali:

```bash
nmap -sn 192.168.171.0/24
```

---

## Evidence Collected
### Telemetry
- **Sysmon Event ID 3** (Network Connection)
- **Sysmon Event ID 1** (Process Creation)

### Observed Behavior
- Many network connection events within a short window
- Source IP consistently: `192.168.171.13`
- Multiple destinations across `192.168.171.0/24`

---

## Detection (Splunk SPL)

### Network burst (1 minute)
```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=3
| bucket _time span=1m
| stats count as connection_count by _time SourceIp
| where connection_count > 50
| sort - connection_count
```

### Correlation (Process + Network)
```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" (EventCode=1 OR EventCode=3)
| eval event_type=case(EventCode=1,"process", EventCode=3,"network")
| stats 
    values(User) as user
    values(Computer) as host
    values(Image) as image
    values(CommandLine) as cmd
    values(ParentImage) as parent_image
    values(DestinationIp) as dest_ip
    values(DestinationPort) as dest_port
    values(Protocol) as protocol
    count(eval(event_type="network")) as network_events
    min(_time) as first_seen
    max(_time) as last_seen
  by ProcessGuid
| where network_events >= 50
| sort - network_events
```

---

## Triage Notes (What an Analyst Checks)
- Identify the generating process (`Image`, `CommandLine`, `ParentImage`)
- Check if the user context is expected
- Confirm spread: `DestinationIp` count/pattern
- Validate if the asset is a scanner/IT tool or unexpected workstation activity

---

## Conclusion
High-confidence internal discovery activity based on volume + destination spread.
Correlation improves fidelity by attributing network activity to a process.

---

## Improvements / Next Iteration
- Tune thresholds (baseline per environment)
- Allowlist known IT scanners / vulnerability tools
- Add asset context (server/workstation tags) to reduce noise

