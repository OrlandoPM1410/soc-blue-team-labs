# Lab IP Plan

## Purpose
Define a stable and reproducible addressing scheme for the SOC lab.

## Networks
### Host-Only (Internal Lab Network)
- CIDR: 192.168.171.0/24
- Purpose: attacker ↔ victim ↔ SIEM internal traffic (no internet dependency)

### NAT (Internet Access)
- Provided by VirtualBox NAT
- Purpose: updates/downloads only (not used for lab attack traffic)

## Hosts
| Role | Hostname | Host-Only IP | Notes |
|---|---|---:|---|
| SIEM | splunk | 192.168.171.14 | Splunk Web runs on port 8001 (lab) |
| Victim endpoint | win-endpoint | 192.168.171.12 | Sysmon enabled, sends telemetry to Splunk |
| Attacker | kali | 192.168.171.13 | Used only to generate controlled attack traffic |

## Notes
- All machines have two adapters:
  - Adapter 1: Host-Only (internal lab)
  - Adapter 2: NAT (internet)
- All attack traffic should target Host-Only IPs (192.168.171.x).
