# Lab – SOC Architecture & Detection Environment

This lab simulates a controlled SOC environment focused on:
- Endpoint telemetry generation
- SIEM ingestion
- Detection engineering
- Controlled attack simulation

## Architecture
See: architecture/telemetry-pipeline.md  
See: architecture/network-architecture.md  

## Components
- Windows Endpoint + Sysmon → components/windows-endpoint/
- Kali Attacker → components/kali-attacker/

## Scenarios
- Suspicious PowerShell Execution → scenarios/suspicious-powershell-execution.md
- Internal Network Discovery → scenarios/network-discovery.md
- Suspicious Outbound Connection → scenarios/suspicious-outbound-connection.md

## Detections
Detection logic is documented under `/detections`.
