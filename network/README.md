# Network Architecture

## Overview

This laboratory environment is designed to simulate an internal enterprise network 
where an attacker (Kali Linux) targets a Windows endpoint. 
All telemetry is centralized in Splunk for detection and analysis.

The network is segmented to clearly separate:

- Internal attack simulation traffic
- Internet access for updates and tool downloads

---

## Network Design

### 1. Host-Only Network (Internal Lab Segment)

CIDR: 192.168.171.0/24

Purpose:
- Simulate internal lateral movement
- Generate attack traffic from Kali to Windows
- Send logs from Windows to Splunk

Hosts:

| Machine | IP Address | Role |
|----------|-------------|------|
| Splunk + Sysmon| 192.168.171.12 | SIEM & Telemetry |
| Windows Endpoint | 192.168.171.12 | Victim |
| Kali Linux | 192.168.171.13 | Attacker |

This network simulates an internal corporate LAN.

---

### 2. NAT Network (Internet Access)

Purpose:
- Software updates
- Downloading tools
- External connectivity

This interface is not used for attack simulation.

---

## Design Principles

- All attack traffic must use Host-Only IP addresses.
- Detection logic must focus on internal lateral activity.
- Internet connectivity is isolated from lab detection scenarios.
- Static IPs are used for consistency and reproducibility.

---

## Future Expansion

This network design allows the future integration of:

- Firewall appliance (e.g., FortiGate)
- IDS/IPS simulation
- Additional endpoints
