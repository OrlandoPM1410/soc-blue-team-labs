# Example Detection – Internal Network Discovery

Technique:
MITRE ATT&CK – T1046 (Network Service Discovery)

Command executed from Kali:
nmap -sn 192.168.171.0/24

Detection Logic:
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=3

Behavior:
Multiple network connection attempts originating from 192.168.171.13.
