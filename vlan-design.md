# VLAN Design and Switch Configuration

## Design Rationale

The lab previously ran flat on 192.168.11.0/24 — every device on the
same broadcast domain with no firewall policy between services. The
renovation segments traffic into four security zones enforced at
OPNsense, not just at the switch.

Key decisions:
- OPNsense on a single vNIC using VLAN sub-interfaces
  (vtnet0_vlan1, vtnet0_vlan10, vtnet0_vlan20, vtnet0_vlan30,
  vtnet0_vlan100) — no second physical NIC required
- Transit VLAN 100 isolates the WAN handoff from the ER7206
- Pi5 placed on MGMT so AdGuard DNS is reachable at 192.168.0.5
  before any VLAN comes up

## VLAN Layout

| VLAN | Name | Subnet | Gateway |
|---|---|---|---|
| 1 | MGMT | 192.168.0.0/24 | 192.168.0.1 |
| 10 | LAB | 192.168.11.0/24 | 192.168.11.1 |
| 20 | GUEST | 192.168.20.0/24 | 192.168.20.1 |
| 30 | IOT | 192.168.30.0/24 | 192.168.30.1 |
| 100 | TRANSIT | 192.168.100.0/24 | 192.168.100.1 |

## Switch Port Assignments (GS308EP)

| Port | Device | PVID | Tagged VLANs |
|---|---|---|---|
| 1 | Spare | 10 | — |
| 2 | su2 trunk | 1 | 1, 10, 20, 30, 100 |
| 3 | su1 trunk | 1 | 1, 10, 20, 30, 100 |
| 4 | AX1800 AP | 20 | — |
| 5 | Pi5 | 1 | — |
| 6 | PlayStation | 20 | — |
| 7 | Admin jack | 1 | — |
| 8 | ER7206 | 1 | 100 |

## DNS Chain

All VLANs point to AdGuard Home at 192.168.0.5.
AdGuard conditionally forwards homelab.local to LAB-DC at 192.168.11.20.
All other queries go to 1.1.1.1 and 8.8.8.8.

## DHCP

OPNsense handles DHCP for all VLANs via Dnsmasq.
LAB-DC DHCP scope deactivated post-migration.
