#### Create `PSCredential` Object with username and password before connecting to WMI
```powershell
$username = 'Administrator';
$password = 'Password123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
```

We then ***proceed to establish a WMI session*** using either of the following:
- `DCOM:` ***RPC over IP will be used for connecting to WMI***. This protocol uses port 135/TCP and ports 49152-65535/TCP.
- `Wsman:` ***WinRM will be used for connecting to WMI***. This protocol uses ports 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS).

#### Establish a WMI session from PowerShell
Use the following commands and ***store the session on the `$Session` variable***, which can be used in the future.
```powershell
$Opt = New-CimSessionOption -Protocol DCOM
$Session = New-Cimsession -ComputerName <TARGET> -Credential $credential -SessionOption $Opt -ErrorAction Stop
```
- `New-CimSessionOption:` cmdlet is used to ***configure the connection options for the WMI session***, including the connection protocol.
- The options and credentials are then passed to `New-CimSession` cmdlet to ***establish a session against a remote host***.

