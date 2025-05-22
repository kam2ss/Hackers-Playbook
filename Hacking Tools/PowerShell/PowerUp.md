### PowerUp
PowerUp is an enumeration tool used to audit client systems for common Windows privilege escalation vectors. It uses various service abuse checks, .dll hijacking opportunities, registry checks and more to enumerate common ways that you might be able to elevate privileges on a target system.  
The tool and its usage can be found in [PowerShellMafia](https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc)'s repository.

### The Checks
PowerUp can be used to check specific vulnerabilities or to check all the common ways of escalating on a Windows machine. When invoking all checks PowerUp has to offer, it performs the following enumeration:

- Checks if user is in a local group with administrative privileges
- Checks for unquoted service paths
- Checks service executable and argument permissions
- Checks service permissions
- Checks %PATH% for potentially hijackable DLL locations
- Checks for AlwaysInstallElevated registry key
- Checks for Autologon credentials in registry
- Checks for modifiable registry autoruns and configs
- Checks for modifiable schtask files/configs
- Checks for unattended install files
- Checks for encrypted web.config strings
- Checks for encrypted application pool and virtual directory passwords
- Checks for plaintext passwords in McAfee SiteList.xml files

### The functions
Once the PowerUp.ps1 module has been imported, all its functions become available in the current PowerShell session.

**Service Enumeration**
- `Get-ServiceUnquoted` returns services with unquoted paths that also have a space in the name.
- `Get-ModifiableServiceFile` returns services where the current user can write to the service binary path or its config.
- `Get-ModifiableService` returns services the current user can modify.
- `Get-ServiceDetail` returns detailed information about a specified service.

**Service Abuse**
- `Invoke-ServiceAbuse` modifies a vulnerable service to create a local admin or execute a custom command.
- `Write-ServiceBinary` writes out a patched C# service binary that adds a local admin or executes a custom command.
- `Install-ServiceBinary` replaces a service binary with one that adds a local admin or executes a custom command.
- `Restore-ServiceBinary` restores a replaced service binary with the original executable.

**DLL Hijacking**
- `Find-ProcessDLLHijack` finds potential DLL hijacking opportunities for currently running processes.
- `Find-PathDLLHijack` finds service %PATH% DLL hijacking opportunities.
- `Write-HijackDll` writes out a hijackable DLL.

**Registry Checks**
- `Get-RegistryAlwaysInstallElevated` checks if the AlwaysInstallElevated registry key is set.
- `Get-RegistryAutoLogon` checks for Autologon credentials in the registry.
- `Get-ModifiableRegistryAutoRun` checks for any modifiable binaries/scripts (or their configs) in HKLM autoruns.

**Miscellaneous Checks**
- `Get-ModifiableScheduledTaskFile` find schtasks with modifiable target files.
- `Get-UnattendedInstallFile` finds remaining unattended installation files.
- `Get-Webconfig` checks for any encrypted web.config strings.
- `Get-ApplicationHost` checks for encrypted application pool and virtual directory passwords.
- `Get-SiteListPassword` retrieves the plaintext passwords for any found in McAfee's SiteList.xml files.

**Other Functions:**
- `Get-ModifiablePath` tokenises an input string and returns the files in it that the current user can modify.
- `Get-CurrentUserTokenGroupSid` returns all SIDs that the current user is a part of, whether they are disabled or not.
- `Add-ServiceDacl` adds a DACL field to a service object returned by Get-Service.
- `Set-ServiceBinPath` sets the binary path for a service to a specified value through Win32 API methods.
- `Test-ServiceDaclPermission` tests one or more passed services or service names against a given permission set.
- `Write-UserAddMSI` writes out an MSI installer that prompts for a user to be added.
- `Invoke-AllChecks` runs all current enumeration checks and returns a report.

### Note
```
# Load the `PowerUp` module:
Import-Module '.\path\to\PowerUp.ps'

# Once the moudle is loaded, perform the checks:
Invoke-AllChecks
```