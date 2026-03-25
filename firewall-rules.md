# OPNsense Firewall Rules

Inter-VLAN policy enforced at OPNsense. Default deny on all interfaces
except MGMT, which has full access to all zones.

## MGMT (VLAN 1)
Full access to all zones — management plane only, no user devices.

## LAB (VLAN 10)
- DNS to AdGuard (192.168.0.5:53)
- NPM to Nextcloud (192.168.11.108 to 192.168.11.19:8080)
- All hosts to Splunk forwarder port (192.168.11.105:9997)
- Forgejo web and SSH (192.168.11.3:3000, :22)
- Ansible controller SSH (192.168.11.107:22)
- General outbound to internet

## GUEST (VLAN 20)
- DNS to AdGuard (192.168.0.5:53) — explicit allow above block rule
- Block all RFC1918 private ranges
- Internet only

## IOT (VLAN 30)
- DNS to AdGuard (192.168.0.5:53) — explicit allow above block rule
- Block all RFC1918 private ranges
- HTTP/HTTPS to internet only
- Default deny everything else

## WAN
- Pass inbound 80/443 to NPM (192.168.11.108) only
- Implicit block all else

## Docker Host Containment
A specific rule blocks the Docker host (192.168.11.108) from initiating
outbound connections to other internal hosts — limits blast radius if
NPM or any container is compromised. Explicit allow rules for
NPM to Nextcloud and forwarder to Splunk sit above the block.
