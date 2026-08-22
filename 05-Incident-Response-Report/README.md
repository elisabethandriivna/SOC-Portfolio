# Incident Response Report – Cerber Ransomware Investigation

## Overview

In this project, I investigated a Cerber ransomware incident using Splunk.

The investigation included network alerts, DNS activity, Windows Sysmon logs, process execution, suspicious files, and file hashes.

The main goal was to understand what happened, identify the affected system, find indicators of compromise (IOCs), and build a timeline of the attack.

## Tools Used

- Splunk
- Sysmon
- Suricata IDS
- VirusTotal
- MITRE ATT&CK

## Incident Summary

The investigation showed activity related to Cerber ransomware.

The affected system generated a Cerber ransomware network alert and communicated with a suspicious domain.

Further investigation showed suspicious process execution, including activity from the user's AppData directory, command-line execution, VBScript activity, and an obfuscated command related to Microsoft Word.

The attacker also attempted to delete Windows shadow copies. This can prevent the victim from restoring files after ransomware encryption.

## Key Findings

- Cerber ransomware activity was detected in network logs.
- The affected IP address was `192.168.250.100`.
- The affected host was `we8105desk`.
- The affected user was `WAYNECORPINC\bob.smith`.
- A suspicious Cerber-related domain was identified.
- Suspicious files were executed from the user's AppData directory.
- `cmd.exe` and `wscript.exe` were involved in the execution chain.
- Microsoft Word was connected to suspicious command execution.
- Windows shadow copies were deleted.
- Suspicious file hashes were collected and investigated.
- A malicious SHA256 hash was confirmed using VirusTotal.
- The malicious hash and domain were searched across the logs to check the scope of the incident.

## MITRE ATT&CK Mapping

The following MITRE ATT&CK techniques were identified during the investigation:

| Technique | ID | Description |
|---|---|---|
| Inhibit System Recovery | T1490 | Shadow copies were deleted to prevent system recovery. |
| User Execution: Malicious File | T1204.002 | A malicious file was executed on the system. |
| Command and Scripting Interpreter: Visual Basic | T1059.005 | VBScript was used during the execution chain. |
| Obfuscated Files or Information: Command Obfuscation | T1027.010 | An obfuscated command was identified during the investigation. |
| Command and Scripting Interpreter: Windows Command Shell | T1059.003 | Windows Command Shell was used to execute commands. |

## Investigation Evidence

### 1. Initial Detection

The investigation started with network security alerts. The alerts showed activity related to Cerber ransomware.

![Network Alerts Overview](screenshots/01-network-alerts-overview.png)

A specific alert showed a Cerber-related onion domain lookup.

![Cerber Onion Domain Alert](screenshots/02-cerber-onion-domain-alert.png)

### 2. Process Investigation

After the network alert, I investigated process activity on the affected system.

Suspicious process activity was found and the process chain was analyzed.

![Ransomware Process Activity](screenshots/03-ransomware-process-activity.png)

![Malicious OSK Process Chain](screenshots/04-malicious-osk-process-chain.png)

The parent processes were also investigated to understand how the suspicious process started.

![OSK Process Parent ID](screenshots/05-osk-process-parent-id.png)

![OSK Parent AdapterTroubleshooter](screenshots/06-osk-parent-adaptertroubleshooter.png)

![AdapterTroubleshooter Parent Explorer](screenshots/07-adaptertroubleshooter-parent-explorer.png)

### 3. Execution Timeline

I created a timeline to understand the order of suspicious events.

![Ransomware Execution Timeline](screenshots/08-ransomware-execution-timeline.png)

The investigation showed suspicious file execution from the user's temporary/AppData location.

![Malicious Temp File Execution](screenshots/09-malicious-temp-file-execution.png)

`cmd.exe` was used to launch the suspicious file.

![CMD Launches Malicious Temp File](screenshots/10-cmd-launches-malicious-temp-file.png)

VBScript activity was also identified.

![VBS Executed by WScript](screenshots/11-vbs-executed-by-wscript.png)

Microsoft Word was connected to an obfuscated command.

![Word Spawns Obfuscated Command](screenshots/12-word-spawns-obfuscated-command.png)

![Malicious Word Document](screenshots/13-malicious-word-document.png)

### 4. Ransomware Impact

The investigation identified activity related to the ransomware note.

![Ransom Note Execution](screenshots/14-ransom-note-execution.png)

Windows shadow copies were also deleted. This is important because ransomware can delete recovery data to make file restoration more difficult.

![Shadow Copy Deletion](screenshots/15-shadow-copy-deletion.png)

### 5. Network Investigation

I investigated the Cerber-related network activity and DNS requests.

![Cerber Onion Domain Alert](screenshots/16-cerber_onion_domain_alert.png)

![Cerber DNS Lookup](screenshots/17_cerber_dns_lookup.png)

The network and endpoint events were then compared to build a clearer attack timeline.

![Ransomware Attack Timeline](screenshots/18_ransomware_attack_timeline.png)

![Key Ransomware Evidence](screenshots/19_key_ransomware_evidence.png)

### 6. MITRE ATT&CK Mapping

The observed activity was mapped to MITRE ATT&CK techniques.

![MITRE T1490](screenshots/20-mitre-t1490-inhibit-system-recovery.png)

![MITRE T1204.002](screenshots/21-mitre-t1204.002-malicious-file.png)

![MITRE T1059.005](screenshots/22-mitre-t1059.005-visual-basic.png)

![MITRE T1027.010](screenshots/23-mitre-t1027.010-command-obfuscation.png)

![MITRE T1059.003](screenshots/24-mitre-t1059.003-windows-command-shell.png)

### 7. IOC Investigation

Suspicious file hashes were collected from the affected system.

![Suspicious File Hashes](screenshots/25-suspicious-file-hashes.png)

The SHA256 hash was checked using VirusTotal and was detected as malicious by multiple security vendors.

![VirusTotal Malicious Hash](screenshots/26-virustotal-malicious-hash.png)

Additional file indicators were collected from the incident.

![Incident IOC Files](screenshots/27-incident-ioc-files.png)

Network indicators related to Cerber were also investigated.

![Cerber Network IOC](screenshots/28-cerber-network-ioc.png)

![Cerber Domain Detection](screenshots/29-cerber-domain-detection.png)

### 8. Incident Scoping

Finally, I searched for the malicious hash and domain across the available logs.

This helped determine where the indicators appeared and check the scope of the incident.

![IOC Hash Scoping](screenshots/30-ioc-hash-scoping.png)

![Cerber Domain Scoping](screenshots/31-cerber-domain-scoping.png)

## Conclusion

The investigation confirmed activity related to Cerber ransomware on the affected system.

Using Splunk, I connected network alerts with endpoint activity, analyzed the process chain, identified suspicious files and commands, collected IOCs, and mapped the activity to MITRE ATT&CK.

I also searched for the identified indicators across the logs to understand the scope of the incident.

This project helped me practice a complete SOC investigation from initial detection to incident analysis and scoping.
