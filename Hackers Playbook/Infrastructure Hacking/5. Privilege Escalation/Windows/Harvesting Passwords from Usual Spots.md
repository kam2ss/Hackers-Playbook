### Unattended Windows Installations
When deploying Windows to many hosts, administrators often use ***Windows Deployment Services***, which allows a ***single OS to be deployed to several hosts*** through the network, for unattended installations that require no user input. 

These installations use an administrator account for setup, and its credentials might be stored in files such as:
- `C:\Unattend.xml`
- `C:\Windows\Panther\Unattend.xml`
- `C:\Windows\Panther\Unattend\Unattend.xml`
- `C:\Windows\system32\sysprep.inf`
- `C:\Windows\system32\sysprep\sysprep.xml`

---
### PowerShell History
PowerShell stores the commands into a file and if a user runs a command that includes a password directly as part of the PowerShell command line, it can later be retrieved by following cmd:

**Cmd.exe**
```cmd.exe
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

**PowerShell**
```powershell
type $Env:userprofile\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

---
### Saved Windows Credentials
**List saved credentials:**
```
cmdkey /list
```

**Trying the found Creds with `runas` and `/savecred`:**
```
runas /savecred /user:admin cmd.exe
```

---
### IIS Configuration
The configuration of websites on IIS is stored in a file called `web.config` and can ***store passwords for databases*** or configured authentication mechanisms.

We can find the `web.config` in one of the following locations:
- `C:\inetpub\wwwroot\web.config`
- `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config`

**Find database connection strings on the file**
```
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```

---
### Retrieve Credentials from Software: Putty
Putty is a Windows SSH client that ***allows users to save session configurations like IPs and usernames***, but not SSH passwords. However, it can ***store proxy credentials in cleartext***.

**Retrieve the stored proxy credentials:**
```
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```
Note: `Simon Tatham` is the creator or Putty and his name is part of the path (hardcoded).