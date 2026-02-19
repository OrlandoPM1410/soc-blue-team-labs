# Scenario – Internal Network Discovery (Ping Sweep / Scan)

## Technique
**MITRE ATT&CK – T1046 (Network Service Discovery)**

---

## Objective
Simulate internal network reconnaissance from an attacker machine to validate:

- Endpoint telemetry (Sysmon)
- SIEM ingestion (Splunk)
- Detection logic based on network behavior + process attribution

---

## Attack Execution
Command executed from Kali Linux (**192.168.171.13**):

```bash
nmap -sn 192.168.171.0/24
```

This performs a ping sweep across the internal Host-Only network segment.

---

## Telemetry Source
**Sysmon Event ID 3 – Network Connection**  
**Sysmon Event ID 1 – Process Creation**

Log channel:
```
Microsoft-Windows-Sysmon/Operational
```

Ingested into Splunk via WinEventLog.

---

## Detection Logic (SPL)

### 1) Basic visibility (network)
```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=3
```

### 2) Behavior-focused (high connection volume)
```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=3
| bucket _time span=1m
| stats count as connection_count by _time SourceIp
| where connection_count > 50
| sort - connection_count
```

### 3) Correlation (Process + Network) using ProcessGuid (recommended)
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

## Expected Behavior
- Many Sysmon EID 3 events in a short time window
- Source IP: **192.168.171.13**
- Multiple destination IPs in the same subnet
- Correlation should attribute traffic to a single ProcessGuid (when applicable)

---

## Validation Steps
1. Execute the scan from Kali (`nmap -sn ...`)
2. Confirm Sysmon EID 3 is being ingested
3. Run the correlation query and confirm:
   - `Image` / `CommandLine` (if present)
   - High `network_events`
   - Destination spread across the subnet

---

## Detection Rationale
High-frequency connection attempts (and/or many unique destinations) from a single host
in a short interval commonly indicates:

- Internal discovery / scanning
- Lateral movement preparation
- Network mapping

Correlation increases confidence by showing **which process** generated the traffic.

---

## Limitations
- Threshold-based logic may cause false positives in noisy environments
- Requires Sysmon fields (ProcessGuid/Image) to be present and correctly ingested
- Single endpoint visibility limits broader lateral movement detection

---

## Lab Context
- Attacker: **192.168.171.13** (Kali)
- Victim/Endpoint: **192.168.171.12** (Windows 10 + Sysmon + Splunk)
- Network: Host-Only **192.168.171.0/24**
