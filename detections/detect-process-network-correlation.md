# Detection – Process and Network Correlation

## Objective

Correlate process creation events (Sysmon Event ID 1) with network connection
events (Sysmon Event ID 3) to attribute suspicious network activity to a
specific process.

This improves detection fidelity compared to monitoring network events alone.

---

## Technique Mapping

- MITRE ATT&CK T1046 – Network Service Discovery
- MITRE ATT&CK T1059 – Command and Scripting Interpreter

---

## Data Sources

- Sysmon Event ID 1 – Process Creation
- Sysmon Event ID 3 – Network Connection
- Log source: Microsoft-Windows-Sysmon/Operational
- SIEM: Splunk

---

## Detection Logic (SPL)

### Correlation using ProcessGuid

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

## Detection Description

This query:

1. Retrieves Sysmon process creation and network events.
2. Groups activity by ProcessGuid.
3. Aggregates process metadata and network connections.
4. Flags processes generating a high number of outbound connections.

---

## Use Case Example

Internal scan executed from Kali:

```bash
nmap -sn 192.168.171.0/24
```

Expected behavior:

- High number of Event ID 3 records
- Single ProcessGuid responsible
- Multiple DestinationIp values
- Short time window

---

## Detection Rationale

Correlating process creation and network activity:

- Identifies which executable generated network traffic.
- Reduces false positives.
- Improves investigation speed.
- Provides context for SOC triage.

---

## Tuning Recommendations

- Adjust network_events threshold based on baseline.
- Create allowlists for known IT scanning tools.
- Add asset context (server vs workstation).

---

## Limitations

- Requires Sysmon properly configured.
- High-traffic legitimate processes may trigger alerts.
- Single-host visibility limits lateral movement detection.

---

## Analyst Triage Checklist

When alert triggers:

- Review Image and CommandLine.
- Confirm User context.
- Validate Destination IP patterns.
- Check ParentImage for suspicious ancestry.
- Determine whether activity matches baseline behavior.

---

## Lab Context

Environment:

- Windows 10 endpoint with Sysmon
- Splunk SIEM
- Kali attacker (192.168.171.13)
- Victim host (192.168.171.12)
- Host-Only network 192.168.171.0/24
