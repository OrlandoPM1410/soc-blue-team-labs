# 1. Log Ingestion in Splunk

## 1.1 Input Configuration

Splunk ingests logs directly from the Sysmon channel using:

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index = main
disabled = 0


This configuration allows Splunk to subscribe to the Sysmon event channel via the Windows Event Log API.

---

## 1.2 Service Context

The Splunk service runs under:

- Local System account

This ensures proper permissions to read the Sysmon event channel.

---

