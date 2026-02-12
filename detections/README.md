# Detections

This directory contains detection logic developed as part of the SOC Blue Team Labs.

Each detection is:

- Based on real Windows telemetry (Sysmon / Event Logs)
- Mapped to MITRE ATT&CK techniques
- Implemented in SPL (Splunk)
- Designed following a structured detection engineering workflow

Future improvements will include:

- Sigma-compatible versions of rules
- Cross-SIEM translations (e.g., KQL equivalents)
- Correlation-based detections
- Metadata documentation (severity, false positive considerations, tuning notes)

Detections are validated through controlled execution inside the lab environment.
