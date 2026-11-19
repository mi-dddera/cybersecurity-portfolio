# SEC-001 — Windows Security and Access Control Analysis

## 1. Objective

Create local user accounts and groups, configure group membership, and analyze Windows authentication events.

## 2. Environment

- OS: Windows 10
- Host: WIN10-LABS
- Tools: PowerShell, Windows Event Viewer, Local Users and Groups

### Created Accounts and Groups

**User accounts:**
- EMP1
- EMP2
- Fin1
- Fin2
- Sup1

**Custom groups:**
- ADMIN
- Finance
- IT-support
- Testers

## 3. Lab Execution

### Step 1 — Account and Group Management

- Created 5 local test accounts.
- Created 4 custom groups representing different departments and roles.
- Enabled test accounts.
- Added users to the corresponding groups.

<img width="1280" height="1024" alt="CreatedUserGroupsList" src="https://github.com/user-attachments/assets/33ad9977-c085-4627-b3ea-13b30d970f2d" />


### Step 2 — Failed Logon Event Collection

- Retrieved failed authentication events using PowerShell.
- Identified 7 failed logon events (Event ID 4625).

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4625}
```

<img width="1289" height="1062" alt="PSHFailedLogsList" src="https://github.com/user-attachments/assets/61589ab6-a17c-4940-b841-8271e9b00b0f" />

### Step 3 — Event Filtering

- Filtered Windows Security events by Event ID 4625.
- Reviewed the returned authentication events.

<img width="1293" height="1066" alt="filter4625Log" src="https://github.com/user-attachments/assets/59328872-43c6-4f66-9258-1d7f9e640daf" />
<img width="1281" height="1072" alt="filteredLogTo4625" src="https://github.com/user-attachments/assets/ad66d3d5-a333-46b7-b743-8eda18f5734a" />

### Step 4 — Event Log Review

- Exported selected Security events.
- Imported and opened saved logs in Windows Event Viewer.
- Examined individual event details.

<img width="1294" height="1068" alt="SavedAndOpenedLogs" src="https://github.com/user-attachments/assets/eb19cc0b-88b9-41ac-b725-7785c426e71e" />

### Step 5 — Group Access Permissions

- Reviewed enabled local accounts and their group memberships.
- Configured separate folder permissions for Finance, IT-support, and Testers.
- Disabled permission inheritance on all three department folders.
- Assigned Modify permissions to the corresponding department groups.
- Retained Full Control for the built-in Administrators group.

  <img width="1352" height="1024" alt="accessList" src="https://github.com/user-attachments/assets/58d68b20-94a8-4465-841d-835dac3def34" />



## 4. Findings

- Created 5 local test accounts and 4 custom groups.
- Assigned users to groups according to their roles.
- Retrieved 7 failed logon events.
- Identified Event ID 4624 — Successful logon.
- Identified Event ID 4625 — Failed logon.
- Examined target account, logon type, source IP, status, and substatus.
- Identified SubStatus `0xC000006A` — incorrect password.
- Observed `127.0.0.1` as the source address in the investigated event.

## 5. Conclusion

Created and organized a basic local user and group environment. Collected and analyzed Windows authentication events using PowerShell and Windows Event Viewer.

All 7 failed logon attempts were intentionally generated during testing. No malicious activity was established.

## 6. Skills Practiced

- Local user and group management
- Group membership configuration
- Windows Security Event Log analysis
- PowerShell event collection and filtering
- Authentication event identification
- Basic log investigation
- Evidence collection and documentation
