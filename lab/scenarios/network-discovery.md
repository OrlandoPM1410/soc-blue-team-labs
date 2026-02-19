# Scenario – Internal Network Discovery

## Technique

**MITRE ATT&CK – T1046 (Network Service Discovery)**

---

## Objective

Simulate internal network reconnaissance activity from an attacker machine  
to validate network telemetry ingestion and detection logic.

This scenario focuses on identifying abnormal volumes of network connection  
attempts originating from a single internal host.

---

## Attack Execution

Command executed from Kali Linux (192.168.171.13):

```bash
nmap -sn 192.168.171.0/24
```

This performs a ping sweep across the internal Host-Only network segment.

---

## Telemetry Source

**Sysmon Event ID 3 – Network Connection**

Log channel:

```
Microsoft-Windows-Sysmon/Operational
```

Ingested into Splunk via WinEventLog input.

---

## Detection Logic (SPL)

### Basic visibility query

```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=3
```

### Behavior-focused query

```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=3
| bucket _time span=1m
| stats count as connection_count by _time SourceIp
| where connection_count > 50
| sort - connection_count
```

---

## Expected Behavior

- Multiple network connection events  
- Source IP: 192.168.171.13  
- High volume of connections within a short time window  

This pattern indicates potential internal reconnaissance activity.

---

## Detection Rationale

High-frequency connection attempts from a single host  
within a short time interval are commonly associated with:

- Network discovery  
- Lateral movement preparation  
- Internal scanning  

This detection is behavior-based and does not rely on static signatures.

---

## Validation

Detection is validated by:

1. Executing the scan from Kali  
2. Confirming Event ID 3 ingestion in Splunk  
3. Observing increased connection counts from the attacker IP  

---

## Limitations

- Threshold-based detection may generate false positives in noisy environments  
- No cross-event correlation implemented yet  
- Single monitored endpoint limits lateral movement visibility  

---

## Lab Context

In this controlled environment:

- Attacker IP: 192.168.171.13  
- Victim IP: 192.168.171.12  
- Network: Internal Host-Only segment
