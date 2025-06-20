#### Requirements
**Ports Used**
- `135/TCP`, `49152–65535/TCP` – for DCE/RPC
- `445/TCP`, `139/TCP` – for RPC over SMB (Named Pipes)

**Required Privileges**
- Must be a member of the `Administrators` group on the remote machine.

#### Process
Windows services can also be leveraged to ***run arbitrary commands since they execute a command when started***. While a service executable is technically different from a regular application, ***if we configure a Windows service to run application, it will execute it and fail afterwards***.

When using `sc`, it will try to ***connect to the Service Control Manager (SVCCTL) remote service program through RPC*** in several ways:
1. A ***connection attempt will be made using DCE/RPC***. The ***client will first connect to the Endpoint Mapper (EPM)*** at port 135. The ***EPM will then respond with the IP and port to connect to SVCCTL***, which is usually a dynamic port in the range of 49152-65535.
![[sc.png]]

2. If the latter connection fails, `sc` will ***try to reach SVCCTL through SMB named pipes***, either on port 445 (SMB) or 139 (SMB over NetBIOS).
![[sc2.png]]

#### Create and start a service named `THMservice`
```cmd
sc.exe \\TARGET create THMservice binPath= "net user <username> <password> /add" start= auto
sc.exe \\TARGET start THMservice
```
- The `net user` command will be executed when the service is started, creating a new local user on the system. Since the OS is in charge of starting the service, you won't be able to look at the command output.

#### Stop and delete the service
```cmd
sc.exe \\TARGET stop THMservice
sc.exe \\TARGET delete THMservice
```



