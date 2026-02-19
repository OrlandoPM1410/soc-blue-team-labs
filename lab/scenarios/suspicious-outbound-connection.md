# Scenario – Suspicious Outbound Connection with Process Attribution

## Technique
- **MITRE ATT&CK – T1071 (Application Layer Protocol)** (generic outbound C2 channel behavior)
- **MITRE ATT&CK – T1059 (Command and Scripting Interpreter)** (if PowerShell/cmd is used)

---

## Objective
Detect an outbound connection to a suspicious external IP and attribute it to a process using Sysmon.

This scenario focuses on:  
**“Which process is responsible for the connection?”** (Process + Network correlation)

---

## Simulation (Example)
Example destination used in this lab:
- Destination IP: `184.231.223.45`
- Destination port: `443`

> The simulation method can vary (PowerShell, curl, browser, etc.).
> The key is validating Sysmon EID 3 ingestion and correlating to EID 1.

---

## Telemetry Source
- Sysmon EID 3 – Network Connection
- Sysmon EID 1 – Process Creation  
Log channel:
```
Microsoft-Windows-Sysmon/Operational
```

---

## Detection Logic (SPL)

### 1) Find outbound connections to the IP/port
```spl
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=3 DestinationIp="184.231.223.45" DestinationPort=443
| stats count as net_events values(Image) as image values(User) as user by ProcessGuid, Computer, SourceIp
| sort - net_events
```

### 2) Correlate to process creation for CommandLine/Parent context
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

## Expected Behavior
- Sysmon EID 3 shows connections to the destination IP on 443
- Correlation query enriches with:
  - `Image`
  - `CommandLine`
  - `ParentImage`

---

## Validation
- Confirm the EID 3 record exists
- Confirm the correlated process context makes sense
- Identify whether the process is legitimate (browser) or suspicious (PowerShell, rundll32, etc.)

---

## Limitations
- If Sysmon logging is incomplete, some fields may be missing
- Some connections (e.g., browser) may appear noisy and require tuning/allowlisting

---

## Lab Context
- Windows 10 endpoint with Sysmon + Splunk
- Focus: attributing outbound traffic to a process
