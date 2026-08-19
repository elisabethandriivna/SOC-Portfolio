# Splunk Sysmon Incident Investigation

## Overview

In this project, I investigated Windows Sysmon logs using Splunk.

The goal was to identify suspicious process activity, investigate related events, and build a timeline based on available evidence.

During the investigation, I identified a PowerShell process with unusual command-line arguments. The process was started by cmd.exe and used ExecutionPolicy bypass together with an encoded command.

I then investigated the process using its Process ID and Process GUID and reviewed related Sysmon events.

## Tools Used

- Splunk Enterprise
- Windows Sysmon logs
- SPL (Search Processing Language)

## Key Findings

- A PowerShell process was started by cmd.exe.
- The PowerShell command used `-Exec bypass`.
- The command contained the `-enc` parameter, indicating an encoded PowerShell command.
- The suspicious PowerShell process had Process ID `5948`.
- Sysmon Event ID 11 showed a temporary PowerShell-related `.ps1` file creation.
- No DNS events related to this process were found in the investigated time window.
- The available evidence was not enough to confirm a successful system compromise.

## Investigation Screenshots

1. `01-process-overview.png` — overview of process creation activity.
2. `02-suspicious-powershell.png` — suspicious PowerShell process and command line.
3. `03-powershell-event-timeline.png` — Sysmon events around the PowerShell execution.
4. `04-powershell-file-creation.png` — file creation associated with the PowerShell process.

## Conclusion

The investigation identified suspicious PowerShell execution using ExecutionPolicy bypass and an encoded command.

These characteristics can be associated with malicious PowerShell activity, but they are not enough by themselves to confirm malware execution.

Based on the available Sysmon logs, the activity should be treated as suspicious and investigated further, but a confirmed compromise cannot be established from the available evidence.
