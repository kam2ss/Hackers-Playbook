#### What is WinRM?
**Windows Remote Management (WinRM)** is a ***web-based protocol used to send PowerShell commands to Windows hosts remotely***. Most Windows Server installations will have WinRM enabled by default.
- **Ports:** 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
- **Required Group Memberships:** Remote Management Users

#### Connect to a remote PowerShell session from the command line
```
winrs.exe -u:Administrator -p:Password123 -r:target_ip cmd
```

#### Achieve the same from PowerShell
**But to pass different credentials, we will need to create a `PSCredential` object**
```powershell
$username = 'Administrator';
$password = 'Mypass123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force; 
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
```

**Once we have our `PSCredential` object, we can create an interactive session using the `Enter-PSSession` cmdlet**
```powershell
Enter-PSSession -Computername TARGET -Credential $credential
```

**PowerShell also includes the `Invoke-Command cmdlet`, which runs `ScriptBlocks` remotely via WinRM. Credentials must be passed through a `PSCredential` object as well.**
```powershell
Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami}
```
