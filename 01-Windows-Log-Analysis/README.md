# Windows Security Event Log Analysis

## Project Overview

This project demonstrates a Windows Security Event Log investigation.

The goal was to identify suspicious account activity by analyzing Windows Security events.

The investigation was performed on a Windows 10 virtual machine using Event Viewer.

---

## Scenario

A suspicious local user account was created and added to the local Administrators group.

Later, the account executed several system discovery commands and attempted to log in.

The objective was to reconstruct the activity using Windows Security Event Logs.

---

## Objectives

- Enable Windows Security auditing
- Analyze Security Event Logs
- Identify suspicious account activity
- Investigate process creation events
- Reconstruct the attack timeline

---

## Tools

- Windows 10
- Event Viewer
- Local Security Policy
- Command Prompt

---

## Investigation Steps

1. Enabled Windows auditing.
2. Created a local user account.
3. Added the account to the Administrators group.
4. Executed discovery commands.
5. Generated failed logon events.
6. Generated a successful logon.
7. Reviewed Windows Security logs.
8. Reconstructed the investigation timeline.

---

## Important Event IDs

| Event ID | Description |
|----------|-------------|
|4720|User account created|
|4732|User added to Administrators|
|4688|Process creation|
|4625|Failed logon|
|4624|Successful logon|

---

## MITRE ATT&CK

- T1136 – Create Account
- T1098 – Account Manipulation
- T1087 – Account Discovery
- T1033 – System Owner/User Discovery

---

## Project Structure

```
01-Windows-Log-Analysis
│
├── README.md
├── report.md
├── findings.md
└── screenshots
```

---

## Screenshots

- Audit Policy Configuration
- User Account Created
- User Added to Administrators
- Process Creation (whoami)
- Process Creation (net user)
- Failed Logon
- Successful Logon
- Security Log Overview
- Investigation Timeline

---

## Skills Demonstrated

- Windows Event Log Analysis
- Windows Auditing
- Security Investigation
- Event Correlation
- Incident Timeline Reconstruction
- MITRE ATT&CK Mapping
