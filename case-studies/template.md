# Case XX – Title

## Objective
Brief description of what is being analyzed.

## Scenario
Explain what happened in the lab.

## Data Sources
- Windows Security Log
- Sysmon (Event ID X, Y, Z)
- PowerShell logs

## Timeline
| Time | Event |
|------|-------|
| 10:01 | Suspicious process executed |
| 10:02 | Network connection established |

## Investigation Process

### Initial Triage
What triggered the alert?

### Deep Analysis
What did you check?
What fields were important?
What hypothesis did you test?

## MITRE ATT&CK Mapping
- Txxxx – Technique name

## Detection Logic

```yaml
title: Suspicious PowerShell Encoded Command
logsource:
  product: windows
detection:
  selection:
    CommandLine|contains: "-enc"
  condition: selection
