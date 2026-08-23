# Mobile Hotspot Connectivity Troubleshooting

## Overview

This case study documents a real connectivity investigation involving slow browsing and delayed video loading on a Lenovo ThinkPad T15 connected to a Samsung phone hotspot on the Rogers cellular network.

Testing progressed from the local Wi-Fi connection to the hotspot gateway, internet path, DNS, HTTPS, and application performance. The objective was to identify whether the problem originated from the laptop, local hotspot connection, name resolution, or cellular network path.

## Environment

| Component         | Configuration              |
| ----------------- | -------------------------- |
| Laptop            | Lenovo ThinkPad T15        |
| Wi-Fi adapter     | Intel Wi-Fi 6 AX201 160MHz |
| Hotspot           | Samsung mobile hotspot     |
| Carrier           | Rogers                     |
| Hotspot band      | 5 GHz                      |
| Wireless security | WPA3-Personal              |
| Operating system  | Windows 11 Pro             |

## Symptoms

* Web browsing worked but was intermittently slow.
* YouTube videos loaded, but startup and seeking were delayed.
* Internet access remained available throughout testing.
* Performance changed after reconnecting the phone to the cellular network.

## Diagnostic Results

| Test                             | Key result                                                       | Interpretation                                                   |
| -------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| `netsh wlan show interfaces`     | 94% signal, −40 dBm RSSI, 433.3 Mbps transmit/receive rate       | Strong local Wi-Fi connection                                    |
| `ipconfig`                       | Valid DHCP address and IPv4/IPv6 gateways                        | Hotspot DHCP was functioning                                     |
| Gateway ping                     | 0% loss, 2 ms minimum, 79 ms maximum, 22 ms average              | Local hotspot path was reachable, with some latency variation    |
| Internet ping                    | 0% loss, 32 ms minimum, 418 ms maximum, 105 ms average           | Significant latency and jitter appeared beyond the local gateway |
| `nslookup google.com`            | Successful IPv4 and IPv6 resolution                              | DNS was functioning                                              |
| `curl -I https://www.google.com` | Successful HTTP `200 OK` response                                | HTTPS connectivity was working                                   |
| `tracert 8.8.8.8`                | Destination reached; first hop 2–3 ms; later spikes up to 220 ms | Route completed despite carrier hops that did not answer ICMP    |
| LTE versus 5G                    | LTE averaged 53 ms; 5G averaged 64 ms with a 194 ms maximum      | LTE was more consistent during this test                         |

## Technical Evidence

### 1. Wireless Link

```cmd
netsh wlan show interfaces
```

![Wi-Fi interface details](screenshots/01-netsh-wlan.png)

*The laptop maintained a strong 5 GHz hotspot connection with excellent signal and a 433.3 Mbps negotiated link rate.*

<br>

### 2. IP Configuration

```cmd
ipconfig
```

![IP configuration](screenshots/02-ipconfig.png)

*The hotspot assigned a valid IP configuration and default gateways, confirming successful DHCP operation.*

<br>

### 3. Local Gateway Test

```cmd
ping 10.184.239.57 -n 20
```

![Gateway ping results](screenshots/03-ping-gateway.png)

*The phone hotspot responded with no packet loss. The local path was substantially better than the external internet test, although some latency variation remained.*

<br>

### 4. External Internet Test

```cmd
ping 8.8.8.8 -n 20
```

![Internet ping results](screenshots/04-ping-internet.png)

*The external test showed no packet loss but produced latency spikes up to 418 ms, indicating unstable performance beyond the hotspot gateway.*

<br>

### 5. DNS Resolution

```cmd
nslookup google.com
```

![DNS lookup results](screenshots/05-nslookup.png)

*Google resolved successfully to multiple IPv4 and IPv6 addresses, ruling out a DNS-resolution failure.*

<br>

### 6. HTTPS Connectivity

```cmd
curl -I https://www.google.com
```

![HTTP connectivity test](screenshots/06-curl-google.png)

*The server returned HTTP `200 OK`, confirming that HTTPS traffic was functioning.*

<br>

### 7. Network Path

```cmd
tracert 8.8.8.8
```

![Traceroute results](screenshots/08-tracert-8.8.8.8.png)

*The trace reached its destination. Intermediate timeouts were not treated as failures because carrier and backbone routers commonly block or deprioritize ICMP responses.*

## LTE and 5G Comparison

| Network mode | Minimum | Maximum | Average | Packet loss |
| ------------ | ------: | ------: | ------: | ----------: |
| LTE          |   40 ms |   76 ms |   53 ms |          0% |
| 5G           |   36 ms |  194 ms |   64 ms |          0% |

Both modes provided connectivity, but LTE produced more consistent latency during the test. The 5G connection had a similar minimum response time but substantially larger spikes.

## Root-Cause Assessment

The evidence did not indicate a primary failure of the laptop’s Wi-Fi adapter, hotspot signal, DHCP, DNS, or general web connectivity.

The largest performance degradation appeared between the hotspot gateway and external internet destinations. Combined with the LTE-versus-5G results and improved performance after reconnection, this pointed to instability in the cellular data session or carrier network path.

The exact carrier-side change could not be confirmed without access to Rogers infrastructure or telemetry. Possible factors included cellular congestion, tower or sector selection, radio-band changes, or carrier routing.

## Resolution and Recommendations

Performance improved after switching between LTE and 5G and re-establishing the hotspot connection. These actions forced the phone to create a new cellular data session.

Recommended actions for recurrence:

1. Compare LTE and 5G latency and use the more stable mode.
2. Toggle airplane mode or reconnect the hotspot to establish a new cellular session.
3. Repeat gateway and internet ping tests to distinguish local Wi-Fi latency from carrier-path latency.
4. Test from another location or time period to identify coverage or congestion patterns.

## Skills Demonstrated

Network troubleshooting · Wi-Fi diagnostics · TCP/IP configuration · DHCP verification · Latency and jitter analysis · DNS testing · HTTP validation · Route tracing · LAN-versus-WAN fault isolation · Evidence-based root-cause analysis

## Scope

This diagnosis is based on observed endpoint and network behavior during the test period. Carrier-side telemetry was unavailable, so the cellular network path is identified as the most likely cause rather than a conclusively proven carrier fault.
