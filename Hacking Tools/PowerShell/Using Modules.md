### Modules in PowerShell
A PowerShell module is a package that contains various functions, workflows and variables that can operate as a single mini program. The code for each module is logically grouped together and stored in a folder with all its dependant files, and can be imported into the current PowerShell session to be used.

The functionality of modules is generally focused around a common theme, such as managing aspects of an application like Microsoft Exchange or Active Directory.

### PowerShell Gallery
The PowerShell Gallery is a repository for sharing useful PowerShell scripts and modules, some created by Microsoft and some created by the PowerShell community. The PowerShell Gallery is typically the default repository and when used in real life can be set to `Trusted` using the following command, but there's no need to run this in the lab.
```powershell
Register-PSRepository -Default -InstallationPolicy Trusted
```

### View modules
The `Get-Module` cmdlet allows all currently loaded modules to be listed. Using the `-ListAvailable` option with this command will also allow you to view all modules that are available but not yet imported.

The `-ListAvailable` option can also be used when a specific module has been provided to list all the available functions within that module.
```powershell
# Shows all available modules
Get-Module -ListAvailable
# Shows the functions contained within the PSReadline module
Get-Module PSReadline -ListAvailable
```

### Import modules
Modules need to be imported to your session before the cmdlets and functions from that module can be used. Modules can be loaded into the current PowerShell session by using the Import-Module cmdlet and specifying the module either by name (-Name) or by path (-Path).
```powershell
# Imports the PSReadline module using its name
Import-Module -Name ‘PSReadline’
```

### Installing modules
If a module is not listed as available, then it can be installed from a repository such as the PowerShell Gallery using the `-InstallModule` cmdlet. Note that you will need an internet connection to install modules and therefore will not be able to install modules inside the lab environment.

### Remove modules
When you remove a module, the commands that the module added are deleted from the session. This is particularly useful when developing your own modules as you may need to remove and re-import a module when you make changes to it.
```powershell
# Removes the PSReadline module
Remove-Module -Name ‘PSReadline’
```
