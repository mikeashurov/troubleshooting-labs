# Dual Monitor Display Troubleshooting with a Lenovo USB-C Dock (40AY)

## Overview

This case study documents a real display investigation involving resolution limitations and inconsistent dual-monitor behavior on a Lenovo ThinkPad T15 connected through a USB-C dock.

Testing progressed through laptop port verification, cable replacement, direct-monitor testing, single- and dual-monitor comparisons, alternate dock substitution, firmware updates, and graphics-driver investigation. The objective was to identify whether the problem originated from the monitors, cables, dock hardware, laptop interface, or graphics subsystem.

## Environment

| Component              | Configuration                                  |
| ---------------------- | ---------------------------------------------- |
| Laptop                 | Lenovo ThinkPad T15 Gen 1                      |
| Operating system       | Windows 11 Pro                                 |
| Primary dock           | Lenovo USB-C Dock 40AY with 90 W adapter       |
| Alternate dock         | Dell WD19-family (K20A-001) with 180 W adapter |
| Monitors               | 2 × MSI MAG274QRF-QD                           |
| Native resolution      | 2560 × 1440                                    |
| Docked connections     | DisplayPort                                    |
| Direct test connection | Laptop HDMI output                             |

## Symptoms

* With both monitors connected through the dock, one display could be limited to 1280 × 720.
* The resolution limitation could move between the two physical monitors.
* Windows sometimes reverted the affected display to 1024 × 768 after sleep or reconnection.
* Display arrangement and Extend settings could become inconsistent after reconnecting the dock.
* After sleep, both external monitors once remained black even though Windows detected all three displays.
* Disconnecting and reconnecting USB-C restored the external displays.

## Diagnostic Results

| Test                         | Key Result                                                                                                         | Interpretation                                                                           |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| Laptop port verification     | Lenovo documentation identified the higher-capability USB-C/Thunderbolt port; using it did not resolve the issue   | Incorrect port selection was ruled out                                                   |
| Cable and connection testing | HDMI/DisplayPort changes and a new DisplayPort cable produced the same behavior                                    | Cable failure was substantially ruled out                                                |
| Direct-monitor test          | One MSI monitor connected directly through laptop HDMI supported its expected high-resolution mode                 | The monitor and laptop could produce the expected resolution through a direct path       |
| Single monitor through dock  | Maximum available mode was 1920 × 1080 at 60 Hz; desktop and active signal resolutions matched                     | The docked display path was already limited with one monitor                             |
| Two monitors through dock    | One display could be limited to 1280 × 720, and the limitation could move between monitors                         | A defective individual monitor was unlikely                                              |
| Alternate dock test          | Dell WD19-family dock produced the same low-resolution behavior                                                    | The Lenovo dock and its 90 W adapter were substantially ruled out                        |
| Sleep/wake test              | Displays remained black while Windows detected three displays and docked USB devices continued working             | Dock connectivity remained active, but the display path failed to reinitialize correctly |
| Firmware and driver review   | Dock firmware was updated, Lenovo updates were current, and repeated `Device not started (IGFX)` events were found | Evidence pointed toward the Intel graphics subsystem                                     |

## Technical Evidence

### 1. Connection Path Validation

Lenovo’s official port documentation was reviewed to identify the ThinkPad’s higher-capability USB-C/Thunderbolt interface. Moving the dock to that port did not resolve the resolution limitation.

HDMI/DisplayPort connection changes and a new DisplayPort cable were also tested without changing the behavior.

### 2. Direct and Docked Display Comparison

One MSI monitor was connected directly to the laptop’s HDMI output and its expected high-resolution mode was available.

When the same display was connected through the dock by itself, Windows allowed a maximum of 1920 × 1080 at 60 Hz. With both monitors docked, one display could be limited to 1280 × 720, which was also the highest mode shown in Windows.

The limitation could move between the two physical monitors, indicating that neither monitor was the consistent point of failure.

### 3. Alternate Dock Substitution

The Lenovo 40AY and its 90W adapter were replaced temporarily with a Dell WD19-family dock and 180 W adapter.

The same low-resolution behavior occurred with the alternate dock, substantially ruling out the original dock and power adapter as the primary cause.

### 4. Sleep and Reconnection Behavior

After sleep, both external monitors once remained black even though Windows continued to detect all three displays. Keyboard, mouse, and other dock-connected USB devices remained operational.

Disconnecting and reconnecting USB-C restored the external displays, although display arrangement settings could become inconsistent after reinitialization. This indicated a display-initialization problem rather than complete dock failure.

### 5. Firmware and Graphics Investigation

The Lenovo 40AY firmware was updated to version 3.98, and Lenovo Vantage reported no remaining updates.

System information reviewed during the investigation included:

* BIOS: Lenovo N2XET46W 1.36, dated 2026-06-02
* Intel graphics driver provider: Intel Corporation
* Intel graphics driver version: 31.0.101.2137
* Intel graphics driver date: 2025-08-28

Device Manager graphics events contained repeated `Device not started (IGFX)` entries. Combined with the hardware-substitution results and display reinitialization behavior, this shifted the investigation toward the Intel graphics driver.

## Root-Cause Assessment

Testing substantially ruled out the individual monitors, DisplayPort cable, Lenovo dock, dock power adapter, and laptop port selection as the primary cause.

The repeated Intel `IGFX` startup events, resolution limitations across two different docks, and sleep/wake initialization failures pointed to the Intel graphics driver. Updating the driver resolved the dual-monitor problem, confirming the driver as the root cause.

## Resolution and Recommendations

The Intel graphics driver was updated, restoring normal dual-monitor operation through the USB-C dock and resolving the resolution limitation.

Recommended actions for recurrence:

1. Check Device Manager graphics events for `IGFX` startup failures.
2. Verify Lenovo-supported graphics, BIOS, and dock firmware updates.
3. Compare direct and docked monitor behavior before replacing hardware.
4. Test both displays after sleep, restart, and USB-C reconnection.

## Skills Demonstrated

Display troubleshooting · Hardware substitution testing · Cable and port validation · Windows display configuration · Driver diagnostics · Firmware and BIOS verification · Device Manager event analysis · Sleep/wake troubleshooting · Root-cause isolation · Evidence-based technical documentation
