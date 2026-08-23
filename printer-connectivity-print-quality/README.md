# HP LaserJet Pro M102w Connectivity and Print-Quality Troubleshooting

## Overview

This case study documents the diagnosis and resolution of two printer problems: failed iPhone connectivity and a persistent linear mark on printed pages.

## Environment

| Component        | Configuration                |
| ---------------- | ---------------------------- |
| Printer          | HP LaserJet Pro M102w        |
| Client           | Apple iPhone                 |
| Connection       | Wi-Fi                        |
| Replacement part | HP 19A Imaging Drum (CF219A) |

## Symptoms

* The iPhone could not connect to the printer.
* Printer settings showed an APIPA address.
* Printed pages contained a persistent linear mark.

## Diagnostic Results

| Test                             | Key Result                         | Interpretation                                       |
| -------------------------------- | ---------------------------------- | ---------------------------------------------------- |
| Network settings                 | Printer had an APIPA address       | Valid network addressing had not been obtained       |
| Static IPv4 configuration        | iPhone connected successfully      | Incorrect addressing caused the connectivity failure |
| Interior inspection and cleaning | Linear mark remained               | Loose debris was unlikely to be the cause            |
| Drum inspection                  | Imaging drum was visibly scratched | Drum damage was the likely print-quality fault       |
| Drum replacement                 | Test page printed normally         | Damaged drum confirmed as the root cause             |

## Technical Evidence

### 1. Connectivity

The printer had assigned itself an APIPA address. Configuring a valid static IPv4 address restored wireless connectivity with the iPhone.

### 2. Print Quality

The printer interior was inspected and cleared of possible debris, but the linear mark remained. Further inspection revealed scratches on the imaging drum.

The original CF219A part number was verified before a sealed genuine HP replacement was installed. A test page then printed normally.

## Root-Cause Assessment

Two separate faults were identified:

1. Invalid APIPA addressing prevented wireless connectivity.
2. A damaged imaging drum caused the linear print defect.

## Resolution and Recommendations

A static IPv4 address restored printer connectivity, and replacing the damaged imaging drum resolved the print-quality problem.

For recurrence, reserve the printer’s address in DHCP or keep its static address outside the DHCP allocation range. Inspect the imaging drum when persistent lines remain after basic cleaning.

## Skills Demonstrated

Printer troubleshooting · Wi-Fi connectivity · APIPA identification · Static IPv4 configuration · Print-quality diagnostics · Hardware inspection · Component replacement · Root-cause isolation
