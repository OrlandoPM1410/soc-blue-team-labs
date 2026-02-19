# Kali Linux – Attacker Component

## Role in the Lab

Kali Linux is used to simulate controlled internal attack scenarios 
against the monitored Windows endpoint.

The objective is not exploitation, but detection validation.

All activity generated from this machine must be observable 
through endpoint telemetry and SIEM analysis.

---

## Network Configuration

Host-Only IP: 192.168.171.13  
Network Segment: 192.168.171.0/24  

All attack traffic is generated through the internal Host-Only network.

A secondary NAT interface is used only for internet access 
(tool downloads and updates).

---

## Scope of Activity

Kali is used to simulate:

- Network discovery (MITRE T1046)
- Port scanning
- Brute-force attempts (controlled)
- Suspicious command execution patterns

No exploitation frameworks or uncontrolled payloads are used in this lab.

---

## Validation Objective

Every simulated action must produce:

1. Endpoint telemetry (Sysmon)
2. Indexed events in Splunk
3. Searchable behavioral evidence
4. Optional alert trigger (if rule exists)

This ensures the lab focuses on detection engineering 
rather than offensive tooling.

