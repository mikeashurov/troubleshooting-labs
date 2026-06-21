# Mobile Hotspot Connectivity Troubleshooting
## Objective

Troubleshoot slow browsing and delayed web loading on a Lenovo ThinkPad T15 using a Samsung phone hotspot on Rogers mobile data. Determine whether the issue is caused by laptop hardware, Wi-Fi hotspot connection, DNS, browser performance, or the cellular network path.

## Environment

* Device: Lenovo ThinkPad T15
* Wi-Fi Adapter: Intel Wi-Fi 6 AX201 160MHz
* Phone Hotspot: Samsung Mobile Hotspot
* Carrier: Rogers
* Hotspot Band: 5 GHz
* Security: WPA3-Personal

## Tools Used

* netsh
* ipconfig
* ping
* nslookup
* curl
* tracert
* Task Manager
* Browser testing

## Step 1 – Verify Wi-Fi Adapter and Hotspot Connection

Command:

```cmd
netsh wlan show interfaces
```

Key Results:

```text
Description: Intel(R) Wi-Fi 6 AX201 160MHz
Band: 5 GHz
Radio type: 802.11ac
Authentication: WPA3-Personal
Receive rate: 433.3 Mbps
Transmit rate: 433.3 Mbps
Signal: 97%
RSSI: -35
```

Conclusion:

The laptop-to-phone Wi-Fi connection was healthy. Signal strength was excellent and the negotiated link speed was more than sufficient for normal internet use.

## Step 2 – Identify the Hotspot Gateway

Command:

```cmd
ipconfig
```

Key Results:

```text
IPv4 Address: 10.209.222.26
Subnet Mask: 255.255.255.0
Default Gateway: 10.209.222.124
```

Conclusion:

The phone hotspot was acting as the default gateway for the laptop.

## Step 3 – Test Local Connectivity to the Phone

Command:

```cmd
ping 10.209.222.124 -n 20
```

Key Results:

```text
Minimum = 1ms
Maximum = 14ms
Average = 3ms
Packet loss = 0%
```

Conclusion:

Local wireless connectivity was stable with extremely low latency and no packet loss.

## Step 4 – Test Internet Latency

Command:

```cmd
ping 8.8.8.8 -n 20
```

Key Results:

```text
Minimum = 90ms
Maximum = 534ms
Average = 262ms
Packet loss = 0%
```

Conclusion:

No packet loss was observed, but latency was very high and inconsistent, indicating a WAN-side issue rather than a local wireless problem.

## Step 5 – Test DNS Resolution

Command:

```cmd
nslookup google.com
```

Conclusion:

DNS resolution succeeded, confirming that name resolution was functioning correctly.

## Step 6 – Test HTTP Connectivity

Command:

```cmd
curl -I https://www.google.com
```

Key Results:

```text
HTTP/1.1 200 OK
```

Conclusion:

The laptop successfully reached Google over HTTPS, ruling out a complete network connectivity failure.

## Step 7 – Test Real-World Application Performance

Observations:

* Google Search loaded quickly
* YouTube videos loaded slowly
* Seeking within videos took several seconds
* Facebook loaded slowly

Conclusion:

Simple websites loaded normally, while content-heavy applications experienced delays. This suggested latency and jitter rather than insufficient bandwidth.

## Step 8 – Trace the Network Path

Commands:

```cmd
tracert facebook.com
tracert youtube.com
```

Observations:

* First hop to the phone was extremely fast
* Latency increased after traffic left the hotspot
* Some hops exceeded 200–500 ms

Conclusion:

The issue originated beyond the phone hotspot and was not caused by the laptop or local wireless connection.

## Step 9 – Compare LTE vs 5G

LTE Results:

```text
Minimum = 40ms
Maximum = 76ms
Average = 53ms
Packet loss = 0%
```

5G Results:

```text
Minimum = 36ms
Maximum = 194ms
Average = 64ms
Packet loss = 0%
```

Conclusion:

LTE produced more stable latency while 5G showed larger spikes. Reconnecting improved 5G performance, suggesting tower congestion, band selection issues, or an unstable cellular path.

## Final Diagnosis

The Lenovo ThinkPad T15 was not the source of the problem.

Ruled Out:

* SSD issues
* CPU issues
* RAM issues
* Wi-Fi adapter issues
* Weak hotspot signal
* WPA3 configuration issues
* DNS failures

Most Likely Cause:

Cellular network latency and jitter on the Rogers mobile connection, particularly while connected to 5G.

## Recommended Fixes

1. Use LTE when it provides lower and more stable latency.
2. Toggle airplane mode to force the phone modem to reconnect.
3. Use USB tethering when possible.
4. Retest with ping during periods of poor performance.
5. Compare performance from different physical locations.
6. Avoid unnecessary hardware upgrades until local issues are confirmed.

## Skills Demonstrated

* Network troubleshooting methodology
* Wireless LAN diagnostics
* Gateway identification
* Latency analysis
* DNS troubleshooting
* HTTP connectivity testing
* Route tracing
* WAN vs LAN fault isolation
* Evidence-based troubleshooting
