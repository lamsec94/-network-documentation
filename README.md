# Network Documentation

Multi-VLAN homelab network — OPNsense firewall, Netgear GS308EP managed
switch, AdGuard DNS, Tailscale remote access.

## Contents

- [VLAN design and switch configuration](vlan-design.md)
- [OPNsense firewall rules](firewall-rules.md)

## Physical Topology

Two Proxmox nodes (su1 Lenovo M910t, su2 HP EliteDesk) connected via independent 1G uplinks to a Netgear GS308EP managed switch. OPNsense
runs as a VM on su1 with a single vNIC trunk carrying all VLAN traffic.
Upstream: Ubiquiti ER7206 edge router demoted to transit-only — hands
off a /24 to OPNsense WAN via VLAN 100.
