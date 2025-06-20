#### Requirements
- **Ports:**
	- 135/TCP, 49152-65535/TCP (DCERPC)
	- 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
	
- **Required Group Memberships:** Administrators

#### What is an MSI?
**MSI** is a ***file format used for installers***. If we can copy an MSI package to the target system, we can then use WMI to attempt to install it for us. Once the MSI file is in the target system, we can attempt to install it by invoking the Win32_Product class through WMI.
```powershell
Invoke-CimMethod -CimSession $Session -ClassName Win32_Product -MethodName Install -Arguments @{PackageLocation = "C:\Windows\myinstaller.msi"; Options = ""; AllUsers = $false}
```

#### Achieve the same by using WMIC in legacy systems
```shell-session
wmic /node:TARGET /user:DOMAIN\USER product call install PackageLocation=c:\Windows\myinstaller.msi
```

