# Investigation Findings

## Summary

| Item | Value |
|------|-------|
| Host | DESKTOP-PTRKJI2 |
| Suspicious Account | analyst_lab |
| Investigation Tool | Event Viewer |
| Log Source | Windows Security Log |

---

## Key Events

| Event ID | Description |
|----------|-------------|
| 4720 | User account created |
| 4732 | User added to Administrators |
| 4688 | Process creation |
| 4625 | Failed logon |
| 4624 | Successful logon |

---

## Commands Observed

- whoami
- net user

---

## MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Create Account | T1136.001 |
| Account Manipulation | T1098 |
| System Owner/User Discovery | T1033 |
| Account Discovery | T1087.001 |

---

## Investigation Result

✔ Local account created

✔ Administrator privileges assigned

✔ Discovery commands executed

✔ Failed logon attempts detected

✔ Successful logon recorded

✔ Event timeline reconstructed
