Lab Architecture & Telemetry Pipeline
1. Purpose
This lab environment was built to understand and validate the complete detection lifecycle in a controlled SOC-like setup:

Endpoint telemetry generation
Log ingestion into SIEM
Detection logic implementation
Alert configuration
Controlled validation
The objective is to work with real Windows telemetry instead of static datasets.

2. General Architecture
High-Level Architecture
Windows 10 VM
- Sysmon installed
- Windows Event Logging
- Controlled attack simulation
                        |
                        | WinEventLog API
                        v
Splunk Enterprise (SIEM)
- WinEventLog ingestion
- SPL detection logic
- Scheduled alerts
- Manual investigation
This architecture simulates a minimal SOC monitoring pipeline.

Extended Architecture – Internal Attack Simulation

In addition to the Windows endpoint, an attacker machine (Kali Linux) has been integrated into the lab to simulate internal threat activity.

Updated Architecture:

Kali Linux (Attacker) | | Internal Host-Only Network (192.168.171.0/24) v Windows 10 VM

Sysmon installed
Controlled attack simulation
Splunk local instance

