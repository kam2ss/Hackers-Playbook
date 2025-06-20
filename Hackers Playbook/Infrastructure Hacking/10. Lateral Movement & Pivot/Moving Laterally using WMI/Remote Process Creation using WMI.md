#### Requirements
- **Ports:**
	- - 135/TCP, 49152-65535/TCP (DCERPC)
	- 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
- **Requirement Group Memberships:** Administrators

#### Remotely spawn a process from PowerShell by leveraging WMI
It does this by ***sending a WMI request to the Win32_Process class to spawn the process*** under the session we created before:
```powershell
$command = "powershell.exe -Command Set-Content -Path C:\text.txt -Value munrawashere";

Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{
CommandLine = $Command
}
```
- Notice, that WMI won't allow you to see the output of any command but will indeed ***create the required process silently***.

***Previous process `$Session` was create in `Connecting to WMI from PowerShell lesson`.***

#### On legacy systems, the same can be done using WMIC from command prompt
```
wmic.exe /user:<Administrator> /password:<Password123> /note:<target> process call create "cmd.exe /c calc.exe" 
```