# Case Studies

This folder contains simulated SOC investigations performed in a controlled lab environment.

Each case includes:
- Scenario description
- Data sources used
- Timeline reconstruction
- Investigation process
- Detection logic
- MITRE ATT&CK mapping
- Response recommendations

## 🧪 Case Studies

### 🔹 Case 01 – Suspicious PowerShell Execution

**Scenario:**  
Investigation of suspicious encoded PowerShell activity on a Windows endpoint.

**What I Practiced**
- Log triage
- Process analysis
- MITRE ATT&CK mapping

**Telemetry Used**
- Security Event ID 4688
- Sysmon Event ID 1

**Outputs**
- Sigma detection rule
- Timeline reconstruction
- IoCs extracted

## Detections

| Detection Name | Technique | Format | Related Case |
|---------------|------------|--------|--------------|
| Suspicious Encoded PowerShell | T1059.001 | Sigma | Case 01 |
| SMB Lateral Movement | T1021.002 | Wazuh Rule | Case 02 |

## Lab Environment

- Windows 10 endpoint
- Windows Server 2019 (Domain Controller)
- Sysmon v15 with custom config
- Wazuh SIEM
- MITRE ATT&CK mapping applied

[Attacker VM] → [Windows 10] → [Domain Controller]


[View Full Analysis](case-studies/case-01/)

