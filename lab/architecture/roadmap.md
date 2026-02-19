# Lab Roadmap

## 1. Current Limitations

- No behavioral correlation across multiple event types
- Limited network telemetry correlation (Event ID 3 used but not correlated across techniques)
- No baseline-based anomaly detection
- No threat intelligence enrichment
- Single monitored endpoint (no multi-host lateral movement yet)

---

## 2. Planned Improvements

- Parent-child anomaly detection (Office → PowerShell)
- Network-based correlation (process + outbound connection)
- Encoded command entropy/length detection
- Sigma rule portability
- Multi-host simulation

---

## Vision

This lab is designed to evolve progressively from:

Single-endpoint detection engineering

to:

Multi-host detection with behavioral correlation and enrichment.
