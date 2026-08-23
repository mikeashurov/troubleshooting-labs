# IT Troubleshooting Labs

## Overview

This repository contains evidence-based troubleshooting case studies based on real endpoint and networking problems. Each lab documents the reported symptoms, diagnostic process, technical evidence, root-cause assessment, and resolution.

## Completed Labs

### [Mobile Hotspot Connectivity Troubleshooting](hotspot-connectivity/)

Investigated slow browsing and delayed video loading on a Windows 11 laptop connected through a Samsung mobile hotspot.

Testing validated the local Wi-Fi connection, DHCP, gateway connectivity, DNS resolution, HTTPS access, internet latency, route behavior, and LTE-versus-5G performance.

**Outcome:** The laptop and local hotspot connection were functioning. Elevated latency and jitter beyond the hotspot gateway identified the cellular network path as the most likely source of the inconsistent performance.

**Tools:** `netsh` · `ipconfig` · `ping` · `nslookup` · `curl` · `tracert`

### [Dual Monitor Display Troubleshooting with a Lenovo USB-C Dock (40AY)](dual-monitor-dock-troubleshooting/)

Investigated resolution limitations and inconsistent sleep/reconnection behavior affecting two 2560 × 1440 monitors connected through a USB-C dock.

Testing compared direct and docked display paths, single- and dual-monitor behavior, cable replacement, laptop port selection, alternate dock hardware, firmware, and Intel graphics events.

**Outcome:** Hardware substitution substantially ruled out the monitors, cables, docks, and power adapters. Updating the Intel graphics driver restored normal dual-monitor operation.

**Tools:** Windows Display Settings · Device Manager · Lenovo Vantage · Dock firmware utility · Hardware substitution testing

## Skills Demonstrated

Network troubleshooting · TCP/IP analysis · Wi-Fi diagnostics · DHCP and DNS validation · Latency and jitter analysis · Route tracing · Display troubleshooting · Hardware substitution testing · Driver and firmware diagnostics · Root-cause isolation · Technical documentation
