# VirtualBox Networking Setup

## Goal
Ensure all VMs can:
1) communicate internally (Host-Only)
2) optionally access the internet (NAT)

## VirtualBox Host-Only Network
- Adapter: "VirtualBox Host-Only Ethernet Adapter #2" (example)
- DHCP: Enabled (recommended) OR static IPs on VMs

## VM Network Adapters (All VMs)
### Adapter 1 (Internal)
- Attached to: Host-Only Adapter
- Name: VirtualBox Host-Only Ethernet Adapter #2
- Cable connected: Yes

### Adapter 2 (Internet)
- Attached to: NAT
- Cable connected: Yes

## Common Fixes
- If Windows gets 169.254.x.x: run `ipconfig /renew`
- If Kali shows eth0 without IP: use NetworkManager to connect or set static IP
