# Indicators of Compromise (IOCs)

## Overview

During the Cerber ransomware investigation, several indicators of compromise were identified.

These indicators were collected from Sysmon, Suricata, DNS, and network logs in Splunk.

---

## Affected System

| Type | Value |
|---|---|
| Host | `we8105desk` |
| User | `WAYNECORPINC\bob.smith` |
| IP Address | `192.168.250.100` |

These values identify the main system and user connected with the ransomware activity.

---

## Malicious Domain

| Type | Value |
|---|---|
| Domain | `cerberhhyed5frqa.xmfir0.win` |

The domain appeared in DNS and network events during the incident.

It was also connected with a Cerber ransomware network alert.

### Evidence

![Cerber Domain Detection](screenshots/29-cerber-domain-detection.png)

---

## Malicious File Hash

### SHA256

`37397f8d8e4b3731749094d7b7cd2cf56cacb12dd69e0131f07dd78dff6f262b`

This hash was found in Sysmon process events on the affected host.

The hash was checked with VirusTotal and was detected as malicious by many security vendors.

### Evidence

![VirusTotal Malicious Hash](screenshots/26-virustotal-malicious-hash.png)

---

## Suspicious Files

The investigation identified several suspicious files connected with the incident.

Examples include:

- `121214.tmp`
- `osk.exe`
- suspicious `.vbs` script

The same malicious hash appeared in multiple process events.

### Evidence

![Incident IOC Files](screenshots/27-incident-ioc-files.png)

---

## Suspicious Processes

Important processes observed during the investigation included:

| Process | Why It Was Investigated |
|---|---|
| `osk.exe` | Connected with suspicious execution activity |
| `cmd.exe` | Used during command execution |
| `wscript.exe` | Used to execute VBScript |
| Microsoft Word | Connected with suspicious and obfuscated command execution |
| `AdapterTroubleshooter.exe` | Appeared in the suspicious process chain |

These processes are not always malicious by themselves. They became suspicious because of their behavior and their connection with other events in this incident.

---

## Network Indicators

| Indicator | Value |
|---|---|
| Affected IP | `192.168.250.100` |
| Malicious Domain | `cerberhhyed5frqa.xmfir0.win` |
| Alert | `ETPRO TROJAN Ransomware/Cerber Onion Domain Lookup` |

### Evidence

![Cerber Network IOC](screenshots/28-cerber-network-ioc.png)

---

## IOC Scoping

After identifying the main IOCs, I searched for them across the available logs.

### Hash Scoping

The malicious SHA256 hash was searched in Sysmon events.

![IOC Hash Scoping](screenshots/30-ioc-hash-scoping.png)

This showed where the malicious hash appeared on the affected host.

### Domain Scoping

The Cerber-related domain was searched across the available network and DNS events.

![Cerber Domain Scoping](screenshots/31-cerber-domain-scoping.png)

This helped identify other events connected with the same domain.

---

## IOC Summary

| Type | Indicator |
|---|---|
| Affected Host | `we8105desk` |
| Affected User | `WAYNECORPINC\bob.smith` |
| Affected IP | `192.168.250.100` |
| Malicious Domain | `cerberhhyed5frqa.xmfir0.win` |
| Malicious SHA256 | `37397f8d8e4b3731749094d7b7cd2cf56cacb12dd69e0131f07dd78dff6f262b` |
| Suspicious File | `121214.tmp` |
| Suspicious File | `osk.exe` |
| Suspicious Script | `20429.vbs` |

---

## Recommended IOC Actions

The identified IOCs can be used for further detection and response.

Recommended actions:

- Block the malicious domain.
- Search other hosts for the malicious SHA256 hash.
- Search DNS logs for the malicious domain.
- Review systems that communicated with the domain.
- Isolate affected systems if malicious activity is confirmed.
- Remove confirmed malicious files.
- Continue monitoring for similar Cerber ransomware activity.
