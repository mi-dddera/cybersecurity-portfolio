# SEC-001 — Windows Security Event Log Analysis

## 1. Objective
Explore Windows Security Event Logs and analyze successful and failed authentication attempts.

## 2. Environment
- OS: Windows 10
- Host: WIN10-LABS
- Tools: PowerShell, Windows Event Viewer

### Test Accounts and Groups

| Group | Members |
|---|---|
| ADMIN | vboxuser |
| Finance | Fin1, Fin2 |
| IT-support | EMP1, EMP2 |
| Testers | Sup1 |

## 3. Lab Execution

**Step 1 — Environment preparation**
- Created local test accounts and groups.
- Configured a basic simulated corporate environment.

**Step 2 — Event generation**
- Generated successful and unsuccessful logon attempts using test accounts.

**Step 3 — Event collection**
- Retrieved failed authentication events using PowerShell:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4625}
```

- Identified successful logon events (Event ID 4624).

**Step 4 — Log review**
- Exported selected Security events.
- Opened saved logs in Windows Event Viewer.
- Examined individual event details.

## 4. Findings
- Identified Event ID 4624 — Successful logon.
- Identified Event ID 4625 — Failed logon.
- Retrieved 7 failed logon events.
- Examined target account, logon type, source IP, status, and substatus.
- Identified SubStatus `0xC000006A` — incorrect password.
- Observed `127.0.0.1` as the source address in the investigated event.

## 5. Conclusion
Successfully collected and reviewed Windows authentication events using PowerShell and Windows Event Viewer.

All 7 failed logon attempts were intentionally generated during testing. No malicious activity was established.

## 6. Evidence

### Evidence 01 — Failed Logon Events
<img width="1289" height="1062" alt="PSHFailedLogsList" src="https://github.com/user-attachments/assets/59d30a6d-85cd-4779-82ed-aa1586dd8003" />


### Evidence 02 — Filtered Logon Events
<img width="1281" height="1072" alt="filteredLogTo4625" src="https://github.com/user-attachments/assets/3ea34104-f7d9-4df2-bd55-1bf9a9973772" />


### Evidence 03 — Imported Logs in Windows Event Viewer

<img width="1294" height="1068" alt="SavedAndOpenedLogs" src="https://github.com/user-attachments/assets/ab45096b-70b4-40bb-bcc4-093187ed45ec" />

## 7. Skills Practiced
- Windows Security Event Log analysis
- PowerShell event collection
- Authentication event identification
- Basic log investigation
- Evidence collection and documentation
