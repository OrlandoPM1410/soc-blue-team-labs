# 1. Endpoint Configuration

## 1.1 Operating System

- Windows 10 (dedicated VM)
- Used exclusively for telemetry generation and attack simulation

---

## 1.2 Sysmon Deployment

Sysmon (System Monitor) is installed to enhance default Windows logging.

Telemetry source:

Microsoft-Windows-Sysmon/Operational

### Relevant Event Types Enabled

- Event ID 1 – Process Creation
- Parent-child process relationships
- Command-line logging
- User and integrity level context

Sysmon provides enriched visibility required for behavior-based detection.

