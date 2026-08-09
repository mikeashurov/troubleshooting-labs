# IT Troubleshooting Labs

## Overview

This repository contains evidence-based troubleshooting case studies based on real endpoint and networking problems. Each lab documents the reported symptoms, diagnostic process, technical evidence, root-cause assessment, and resolution.

## Completed Labs

### [Mobile Hotspot Connectivity Troubleshooting](hotspot-connectivity/)

Investigated slow browsing and delayed video loading on a Windows 11 laptop connected through a Samsung mobile hotspot.

Testing validated the local Wi-Fi connection, DHCP, gateway connectivity, DNS resolution, HTTPS access, internet latency, route behavior, and LTE-versus-5G performance.

**Outcome:** The laptop and local hotspot connection were functioning. Elevated latency and jitter beyond the hotspot gateway identified the cellular network path as the most likely source of the inconsistent performance.

**Tools:** `netsh` · `ipconfig` · `ping` · `nslookup` · `curl` · `tracert`

## Skills Demonstrated

Network troubleshooting · TCP/IP analysis · Wi-Fi diagnostics · DHCP and DNS validation · Latency and jitter analysis · Route tracing · LAN-versus-WAN fault isolation · Technical documentation
