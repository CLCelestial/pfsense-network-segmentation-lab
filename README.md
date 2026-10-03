# pfSense Network Segmentation Lab

A virtualized network lab demonstrating LAN segmentation and firewall-based access control using pfSense.

## Overview

I built a segmented network lab in VirtualBox using pfSense as a router and firewall, with two client VMs (Kali Linux and Ubuntu) placed on separate LANs, `192.168.10.0/24` and `192.168.20.0/24`. Both LANs had full internet access via NAT through pfSense's WAN interface.

## Topology

![Network topology diagram](screenshots/topology-diagram.png)

| Component | Details |
|---|---|
| Firewall / Router | pfSense CE 2.9.0 |
| WAN | VirtualBox NAT (`10.0.2.15/24`) |
| LAN1 | `192.168.10.0/24`, gateway `192.168.10.1` |
| LAN2 | `192.168.20.0/24`, gateway `192.168.20.1` |
| Client 1 | Kali Linux, `192.168.10.100` (DHCP) |
| Client 2 | Ubuntu, `192.168.20.100` (DHCP) |

## What I configured

- Created the pfSense VM in VirtualBox with a WAN adapter on NAT (for internet access) and two LAN adapters on isolated internal networks
- Installed pfSense CE 2.9.0 and set up LAN1 (`192.168.10.1/24`) and LAN2 (`192.168.20.1/24`) with DHCP enabled on both
- Connected Kali Linux to LAN1 and Ubuntu to LAN2, confirmed both received DHCP leases and had internet access through pfSense
- Configured a firewall rule on the pfSense LAN interfaces to block all traffic between `192.168.10.0/24` and `192.168.20.0/24`, while leaving WAN/internet access unaffected
- Verified the rule using ICMP (ping) as the test protocol: confirmed devices on each subnet could reach the internet but not each other, and confirmed the block held with logging enabled

## Before / After

**Before:** both LANs could freely communicate with each other and with the internet.

**After:** inter-LAN traffic is blocked at the firewall (verified via ICMP testing and firewall logs); internet access remains fully functional for both subnets.

## What I learned

How firewall rule order and priority determine which rule actually applies when multiple rules could match the same traffic, and how to validate a security control empirically (via testing and logs) rather than just trusting the configuration as written.

## Next steps

- Add more granular rules (e.g., allow only specific ports/services between LANs, or restrict outbound internet access to an allowlist of destinations)
- Layer an IDS/IPS (Suricata/Snort) on top of this lab to detect and log intrusion attempts
- Use the logs this lab generates as input for a Python/SQL log-analysis project

## Screenshots

> Add pfSense firewall rule screenshots, firewall logs, and before/after ping test results to this folder, then reference them below.

<!-- 
![Firewall rules](screenshots/firewall-rules.png)
![Firewall logs](screenshots/firewall-logs.png)
![Before test](screenshots/ping-before.png)
![After test](screenshots/ping-after.png)
-->
