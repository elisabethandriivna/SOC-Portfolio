# Incident Investigation Report

## 1. Investigation Summary

Windows Sysmon logs were analyzed in Splunk to investigate suspicious process activity.

During the analysis, a PowerShell process was identified with unusual command-line arguments:

`powershell.exe -Exec bypass -enc ...`

The process was started by `cmd.exe` under the `ATTACKRANGE\Administrator` account.

Because ExecutionPolicy bypass and encoded PowerShell commands can be used during malicious activity, the process was investigated further.

---

## 2. Initial Process Analysis

Sysmon Event ID 1 (Process Creation) events were reviewed to identify processes executed on the system.

The process overview showed mostly Splunk Universal Forwarder processes. However, a Windows PowerShell process was also identified:

- Image: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
- User: `ATTACKRANGE\Administrator`
- Parent process: `C:\Windows\System32\cmd.exe`
- Process ID: `5948`
- Parent Process ID: `4936`

The command line contained:

`-Exec bypass -enc`

This activity was selected for further investigation.

**Evidence:** `01-process-overview.png`, `02-suspicious-powershell.png`

---

## 3. PowerShell Investigation

The PowerShell process was investigated using its Process ID and Process GUID.

Process GUID:

`{E983936C-DE28-6006-9809-00000000A301}`

Using the Process GUID allowed related Sysmon events to be correlated with the same PowerShell process.

The investigation identified two relevant events around the execution time:

- Event ID 1 — PowerShell process creation
- Event ID 11 — file creation

The events occurred within milliseconds of each other.

**Evidence:** `03-powershell-event-timeline.png`

---

## 4. File Creation Activity

Sysmon Event ID 11 showed that the investigated PowerShell process created a temporary `.ps1` file:

`C:\Users\Administrator\AppData\Local\Temp\__PSScriptPolicyTest_1py4clft.gbo.ps1`

The Process ID associated with the event was `5948`, matching the investigated PowerShell process.

This file creation was therefore correlated with the PowerShell execution.

The file name suggests PowerShell-related script policy activity. By itself, this event does not prove that a malicious file was created.

**Evidence:** `04-powershell-file-creation.png`

---

## 5. DNS Investigation

Sysmon Event ID 22 (DNS Query) events were searched around the PowerShell execution time.

No DNS events were found in the investigated time window.

Because of this, the available logs do not provide evidence of DNS-based network communication associated with the PowerShell process.

---

## 6. Additional Sysmon Analysis

The wider time window contained:

- Event ID 1 — Process Creation
- Event ID 10 — Process Access
- Event ID 11 — File Creation

Process Access events were also reviewed for the suspicious PowerShell Process ID.

No Event ID 10 records were found with the investigated PowerShell process as the source process.

Therefore, no additional suspicious process-access activity could be correlated with this PowerShell execution using the available data.

---

## 7. Findings

The investigation identified the following:

- PowerShell was launched by `cmd.exe`.
- The process ran under `ATTACKRANGE\Administrator`.
- The command used ExecutionPolicy bypass.
- The command contained an encoded PowerShell argument.
- The PowerShell Process ID was `5948`.
- A related temporary `.ps1` file creation event was identified.
- No related DNS activity was identified in the investigated time window.
- No additional Process Access activity from the PowerShell process was identified.

---

## 8. Assessment

The PowerShell execution should be considered suspicious because encoded commands and ExecutionPolicy bypass are techniques that may be used to hide or execute malicious PowerShell activity.

However, the available evidence does not confirm that the encoded command contained a malicious payload or that the system was successfully compromised.

Additional evidence would be required for a confirmed malicious verdict.

Useful additional sources could include:

- PowerShell Script Block Logging
- Endpoint Detection and Response (EDR) telemetry
- Network traffic
- Antivirus alerts
- Additional Windows event logs

---

## 9. Conclusion

The investigation identified suspicious PowerShell execution and correlated Sysmon activity.

The strongest indicator was the combination of ExecutionPolicy bypass and an encoded PowerShell command.

Based on the available Sysmon evidence, the final assessment is:

**Suspicious activity — compromise not confirmed.**
