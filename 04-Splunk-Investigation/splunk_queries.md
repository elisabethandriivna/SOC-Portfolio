# Splunk Investigation Queries

This file contains the main SPL queries used during the Sysmon investigation.

## 1. Process Overview

This search was used to review process creation activity and identify unusual processes.

```spl
index=incident_lab source="windows-sysmon.txt" EventCode=1
| stats count by Image
| sort - count
```

This search helped identify a Windows PowerShell process among mostly Splunk Universal Forwarder processes.

---

## 2. PowerShell Process Investigation

This search was used to review the PowerShell process in more detail.

```spl
index=incident_lab source="windows-sysmon.txt" EventCode=1
Image="*\\powershell.exe"
| table _time User Image ParentImage CommandLine ProcessId ParentProcessId ProcessGuid
```

The search showed that PowerShell was launched by `cmd.exe` and used the `-Exec bypass` and `-enc` parameters.

---

## 3. PowerShell Event Correlation

This search used the Process GUID to find Sysmon events related to the same PowerShell process.

```spl
index=incident_lab source="windows-sysmon.txt"
ProcessGuid="{E983936C-DE28-6006-9809-00000000A301}"
| table _time EventCode User Image CommandLine QueryName QueryResults TargetFilename TargetObject ProcessId ProcessGuid
| sort _time
```

This correlated the PowerShell process creation event with a related file creation event.

---

## 4. File Creation Analysis

This search was used to review files created after the PowerShell execution.

```spl
index=incident_lab source="windows-sysmon.txt" EventCode=11
earliest="01/19/2021:13:27:04"
latest="01/19/2021:13:28:10"
| table _time User Image TargetFilename ProcessId ProcessGuid
| sort _time
```

A temporary PowerShell-related `.ps1` file was identified.

---

## 5. DNS Activity Check

This search checked for DNS activity after the PowerShell execution.

```spl
index=incident_lab source="windows-sysmon.txt" EventCode=22
earliest="01/19/2021:13:27:04"
latest="01/19/2021:13:29:00"
| table _time Image ProcessId QueryName QueryStatus QueryResults
| sort _time
```

No DNS events were found during this time window.

---

## 6. Process Activity After PowerShell

This search reviewed processes created after the investigated PowerShell execution.

```spl
index=incident_lab source="windows-sysmon.txt" EventCode=1
earliest="01/19/2021:13:27:04"
latest="01/19/2021:13:32:00"
| table _time User Image ParentImage CommandLine ProcessId ParentProcessId
| sort _time
```

Most of the following process activity was related to Splunk Universal Forwarder.

No clear malicious child process was identified.

---

## 7. Process Access Check

This search checked whether the investigated PowerShell process accessed other processes.

```spl
index=incident_lab source="windows-sysmon.txt" EventCode=10 SourceProcessId=5948
| table _time SourceImage SourceProcessId TargetImage TargetProcessId GrantedAccess CallTrace
| sort _time
```

No matching Process Access events were found for PowerShell Process ID `5948`.

---

## Investigation Result

The SPL searches helped investigate and correlate the suspicious PowerShell execution.

The command initially appeared suspicious because it used ExecutionPolicy bypass and an encoded command.

The encoded Base64 value was:

`VwByAGkAdABlAC0ASABvAHMAdAAgACIASABlAGwAbABvACAAVwBvAHIAbABkACIA`

After decoding the command, the result was:

```powershell
Write-Host "Hello World"
```

The decoded command did not contain a malicious payload.

No additional evidence in the available Sysmon logs confirmed malicious activity or a successful system compromise.

**Final assessment: Suspicious activity investigated — malicious activity not confirmed.**
