# Investigation Findings

## Finding 1 — Suspicious PowerShell Execution

A Windows PowerShell process was identified:

- User: `ATTACKRANGE\Administrator`
- Image: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
- Parent process: `C:\Windows\System32\cmd.exe`
- Process ID: `5948`
- Parent Process ID: `4936`

The command line contained:

`powershell.exe -Exec bypass -enc ...`

### Why It Is Suspicious

`-Exec bypass` allows PowerShell to run without normal Execution Policy restrictions.

`-enc` indicates that the command was encoded. Encoded PowerShell commands can be used to hide the actual command being executed.

These parameters are not proof of malicious activity, but their combination makes the execution worth investigating.

**Evidence:** `02-suspicious-powershell.png`

---

## Finding 2 — Related File Creation

Sysmon Event ID 11 was identified shortly after the PowerShell process started.

The PowerShell process was associated with the creation of:

`C:\Users\Administrator\AppData\Local\Temp\__PSScriptPolicyTest_1py4clft.gbo.ps1`

The event used Process ID `5948` and the same Process GUID as the investigated PowerShell process.

The filename appears related to PowerShell script policy testing. Therefore, this event alone is not considered evidence of malware.

**Evidence:** `03-powershell-event-timeline.png`, `04-powershell-file-creation.png`

---

## Finding 3 — No Related DNS Activity Found

Sysmon Event ID 22 was searched around the PowerShell execution.

No DNS query events were found in the investigated time window.

This means the available Sysmon data did not show DNS activity related to the investigated PowerShell execution.

---

## Finding 4 — No Related Process Access Found

Sysmon Event ID 10 events were reviewed.

No Process Access events were found where PowerShell Process ID `5948` was the source process.

The available data therefore did not show additional process-access activity from this PowerShell process.

---

## Final Assessment

**Verdict: Suspicious activity — compromise not confirmed.**

The strongest suspicious indicators were:

- PowerShell launched from `cmd.exe`
- ExecutionPolicy bypass
- Encoded PowerShell command

However, the available Sysmon logs do not provide enough evidence to confirm malware execution or a successful compromise.

Further investigation would require additional endpoint, PowerShell, or network telemetry.
