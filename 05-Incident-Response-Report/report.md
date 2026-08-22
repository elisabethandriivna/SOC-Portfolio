# Incident Response Report – Cerber Ransomware

## 1. Incident Information

**Incident Type:** Ransomware  
**Malware:** Cerber Ransomware  
**Severity:** High  
**Affected Host:** `we8105desk`  
**Affected User:** `WAYNECORPINC\bob.smith`  
**Affected IP:** `192.168.250.100`  

## 2. Incident Summary

A security investigation was performed after Cerber ransomware activity was detected in the network logs.

The investigation showed suspicious DNS activity, malicious file execution, command-line activity, VBScript execution, and deletion of Windows shadow copies.

Splunk was used to connect network and endpoint events and understand the attack.

The investigation also identified suspicious file hashes and a Cerber-related domain. These indicators were used to search the available logs and check the scope of the incident.

---

## 3. Initial Detection

The investigation started with network alerts in Splunk.

The alerts showed suspicious activity from the internal IP address:

`192.168.250.100`

One of the important alerts was:

`ETPRO TROJAN Ransomware/Cerber Onion Domain Lookup`

This indicated possible communication related to Cerber ransomware.

### Evidence

![Network Alerts Overview](screenshots/01-network-alerts-overview.png)

![Cerber Onion Domain Alert](screenshots/02-cerber-onion-domain-alert.png)

---

## 4. Endpoint Investigation

After identifying the suspicious network activity, I investigated the affected Windows host.

The host was:

`we8105desk`

The affected user was:

`WAYNECORPINC\bob.smith`

Suspicious process activity was found in Sysmon logs.

The investigation showed unusual execution related to `osk.exe` and other processes.

### Evidence

![Ransomware Process Activity](screenshots/03-ransomware-process-activity.png)

![Malicious OSK Process Chain](screenshots/04-malicious-osk-process-chain.png)

To understand the execution chain, I investigated the parent process information.

![OSK Process Parent ID](screenshots/05-osk-process-parent-id.png)

![OSK Parent AdapterTroubleshooter](screenshots/06-osk-parent-adaptertroubleshooter.png)

![AdapterTroubleshooter Parent Explorer](screenshots/07-adaptertroubleshooter-parent-explorer.png)

This helped connect the suspicious processes and understand how they were executed.

---

## 5. Execution Timeline

I created a timeline of the suspicious process activity.

This helped identify the order of events during the incident.

![Ransomware Execution Timeline](screenshots/08-ransomware-execution-timeline.png)

A suspicious temporary file was executed from the user's directory.

![Malicious Temp File Execution](screenshots/09-malicious-temp-file-execution.png)

Further investigation showed that `cmd.exe` was involved in the execution.

![CMD Launches Malicious Temp File](screenshots/10-cmd-launches-malicious-temp-file.png)

VBScript activity was also found. `wscript.exe` was used to execute a VBS script.

![VBS Executed by WScript](screenshots/11-vbs-executed-by-wscript.png)

Another important finding was suspicious activity related to Microsoft Word and an obfuscated command.

![Word Spawns Obfuscated Command](screenshots/12-word-spawns-obfuscated-command.png)

![Malicious Word Document](screenshots/13-malicious-word-document.png)

These events showed that different processes and scripts were used during the ransomware execution.

---

## 6. Ransomware Activity and Impact

The investigation found activity related to the ransomware note.

![Ransom Note Execution](screenshots/14-ransom-note-execution.png)

Another important event was the deletion of Windows shadow copies.

![Shadow Copy Deletion](screenshots/15-shadow-copy-deletion.png)

Shadow copies can be used to restore previous versions of files.

Deleting them makes system recovery more difficult after a ransomware attack.

This behavior is commonly connected with ransomware because the attacker wants to reduce recovery options.

---

## 7. Network and DNS Investigation

The network investigation showed Cerber-related DNS activity.

A Cerber onion domain alert was identified in the Suricata logs.

![Cerber Onion Domain Alert](screenshots/16-cerber_onion_domain_alert.png)

The suspicious domain was:

`cerberhhyed5frqa.xmfir0.win`

DNS activity related to this domain was investigated.

![Cerber DNS Lookup](screenshots/17_cerber_dns_lookup.png)

Network events and endpoint events were then compared to understand the incident timeline.

![Ransomware Attack Timeline](screenshots/18_ransomware_attack_timeline.png)

The most important ransomware events were collected together.

![Key Ransomware Evidence](screenshots/19_key_ransomware_evidence.png)

---

## 8. MITRE ATT&CK Mapping

The observed activity was mapped to MITRE ATT&CK.

### T1490 – Inhibit System Recovery

Windows shadow copies were deleted.

This can prevent the victim from recovering files after ransomware encryption.

![MITRE T1490](screenshots/20-mitre-t1490-inhibit-system-recovery.png)

### T1204.002 – User Execution: Malicious File

A malicious file was executed on the affected system.

![MITRE T1204.002](screenshots/21-mitre-t1204.002-malicious-file.png)

### T1059.005 – Command and Scripting Interpreter: Visual Basic

VBScript activity was identified during the investigation.

![MITRE T1059.005](screenshots/22-mitre-t1059.005-visual-basic.png)

### T1027.010 – Obfuscated Files or Information: Command Obfuscation

An obfuscated command was found in the process activity.

Obfuscation can make malicious commands more difficult to detect and analyze.

![MITRE T1027.010](screenshots/23-mitre-t1027.010-command-obfuscation.png)

### T1059.003 – Command and Scripting Interpreter: Windows Command Shell

`cmd.exe` was involved in suspicious command execution.

![MITRE T1059.003](screenshots/24-mitre-t1059.003-windows-command-shell.png)

---

## 9. File Hash Investigation

File hashes were collected from suspicious files found during the investigation.

![Suspicious File Hashes](screenshots/25-suspicious-file-hashes.png)

One important SHA256 hash was:

`37397f8d8e4b3731749094d7b7cd2cf56cacb12dd69e0131f07dd78dff6f262b`

The hash was checked using VirusTotal.

VirusTotal showed that many security vendors detected the file as malicious.

![VirusTotal Malicious Hash](screenshots/26-virustotal-malicious-hash.png)

This provided additional evidence that the file was malicious.

Additional files connected with the same hash were also identified.

![Incident IOC Files](screenshots/27-incident-ioc-files.png)

---

## 10. IOC Investigation

The investigation identified both file and network indicators.

Important indicators included:

- A malicious SHA256 hash
- Suspicious files
- The Cerber-related domain
- Cerber network alerts
- The affected host and IP address

Network activity related to the Cerber IOC was reviewed.

![Cerber Network IOC](screenshots/28-cerber-network-ioc.png)

The Cerber domain was also found in the logs.

![Cerber Domain Detection](screenshots/29-cerber-domain-detection.png)

---

## 11. Incident Scoping

After identifying the main indicators, I searched for them across the available logs.

First, I searched for the malicious SHA256 hash.

![IOC Hash Scoping](screenshots/30-ioc-hash-scoping.png)

The hash appeared in multiple process events on the affected host.

I also searched for the Cerber-related domain.

![Cerber Domain Scoping](screenshots/31-cerber-domain-scoping.png)

The domain appeared in DNS/network events connected with the incident.

This step helped identify where the known indicators appeared in the available data.

---

## 12. Recommended Response Actions

Based on the investigation, the following actions should be taken:

1. Isolate the affected host from the network.
2. Block the malicious domain.
3. Block known malicious file hashes.
4. Remove malicious files from the affected system.
5. Scan the host with endpoint security tools.
6. Review the affected user account for additional suspicious activity.
7. Search other systems for the same IOCs.
8. Restore affected files from a clean backup if available.
9. Reset credentials if account compromise is suspected.
10. Continue monitoring for similar ransomware activity.

---

## 13. Final Assessment

The investigation found strong evidence of Cerber ransomware activity on the affected system.

The incident included suspicious network communication, malicious file execution, script and command-line activity, and deletion of Windows shadow copies.

Splunk helped connect events from different log sources and build a timeline of the attack.

The identified file hashes and domain were also used for IOC scoping to check for related activity in the available logs.

**Final Verdict:** Confirmed Cerber ransomware activity  
**Severity:** High
