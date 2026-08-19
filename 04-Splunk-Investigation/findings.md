# Investigation Findings

## Finding 1 — Encoded PowerShell Execution

A Windows PowerShell process was identified during the investigation.

- User: `ATTACKRANGE\Administrator`
- Image: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
- Parent process: `C:\Windows\System32\cmd.exe`
- Process ID: `5948`
- Parent Process ID: `4936`

The command line contained:

`powershell.exe -Exec bypass -enc ...`

### Why It Was Investigated

The command used `-Exec bypass` together with the `-enc` parameter.

ExecutionPolicy bypass and encoded PowerShell commands can be used by attackers to hide or execute malicious commands. Because of this, the process required further investigation.

**Evidence:** [Suspicious PowerShell execution](screenshots/02-suspicious-powershell.png)

---

## Finding 2 — Encoded Command Analysis

The Base64 encoded command was extracted and decoded.

Encoded value:

`VwByAGkAdABlAC0ASABvAHMAdAAgACIASABlAGwAbABvACAAVwBvAHIAbABkACIA`

Decoded command:

`Write-Host "Hello World"`

The decoded command did not contain a malicious payload.

This reduced the severity of the original PowerShell finding. The command line looked suspicious, but the decoded content itself was benign.

---

## Finding 3 — Related File Creation

Sysmon Event ID 11 showed file creation shortly after the PowerShell process started.

The file was:

`C:\Users\Administrator\AppData\Local\Temp\__PSScriptPolicyTest_1py4clft.gbo.ps1`

The event was associated with Process ID `5948` and the same Process GUID as the investigated PowerShell process.

The filename appears related to PowerShell script policy testing. Therefore, this event alone was not considered evidence of malware.

**Evidence:** [PowerShell event timeline](screenshots/03-powershell-event-timeline.png)

**Evidence:** [PowerShell file creation](screenshots/04-powershell-file-creation.png)

---

## Finding 4 — No Related DNS Activity Found

Sysmon Event ID 22 was checked after the PowerShell execution.

No DNS query events were found in the investigated time window.

The available Sysmon data therefore did not show DNS communication related to the investigated PowerShell process.

---

## Finding 5 — No Malicious Follow-Up Activity Confirmed

Process Creation events after the PowerShell execution were reviewed.

Most of the following activity was related to Splunk Universal Forwarder processes started by `splunkd.exe`.

No clear malicious child process was identified.

Sysmon Event ID 10 (Process Access) was also checked for PowerShell Process ID `5948`, but no matching events were found.

---

## Final Assessment

**Verdict: Suspicious activity investigated — malicious activity not confirmed.**

The PowerShell execution initially appeared suspicious because it used:

- ExecutionPolicy bypass
- An encoded command
- `cmd.exe` as its parent process

However, the encoded command was decoded to:

`Write-Host "Hello World"`

No malicious payload, related DNS communication, malicious child process, or suspicious Process Access activity was confirmed in the available Sysmon logs.

The activity required investigation, but the available evidence was not sufficient to confirm a system compromise.
