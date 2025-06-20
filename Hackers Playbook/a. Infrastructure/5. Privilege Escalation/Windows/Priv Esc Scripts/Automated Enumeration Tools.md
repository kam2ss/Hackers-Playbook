### WinPEAS
The **Windows Privilege Escalation Awesome Script** (`winPEAS`) is an enumeration script that searches for possible privilege escalation routes on a Windows host.

WinPEAS can be downloaded [here](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS).

**Run the Tool**
```powershell
winPEAS.exe > output.txt
```

---
### PrivescCheck
PrivescCheck is a PowerShell script that ***searches common privilege escalation*** on the target system. It provides an alternative to WinPEAS without requiring the execution of a binary file ***(PrivescCheck is a PowerShell script (.ps1), not an executable binary (.exe)***.

PrivescCheck can be downloaded [here](https://github.com/itm4n/PrivescCheck).

**You may need to bypass the execution policy restrictions to run PrivescCheck**
```powershell
Set-ExecutionPolicy Bypass -Scope process -Force
. .\PrivescCheck.ps1
```

**Run the Tool**
```powershell
Invoke-PrivescCheck > output.txt
```

---
### WES-NG: Windows Exploit Suggester - Next Generation
Some scripts like **`winPEAS`** require ***uploading to the target***, which may trigger antivirus detection. To avoid this, **WES-NG** ***runs on your attacking machine*** (e.g., Kali). It's a Python script that ***identifies missing patches*** leading to privilege escalation vulnerabilities.

**Update the database, once installed**
```bash
wes.py --update
```

**To use the script, you will need to gather System Info on the target system**
```powershell
systeminfo > systeminfo.txt
```
**Transfer the `systeminfo.txt` to the Attacker Machine**

**Run the Tool**
```bash
wes.py systeminfo.txt
```

---
### Metasploit
If you already have a Meterpreter shell on the target system.

**Run the Tool**
```bash
multi/recon/local_exploit_suggester
```

---
### PowerUp
Part of the **PowerSploit** suite, PowerUp is made up of a series of PowerShell functions that aim to identify common Windows privilege escalation vectors ***due to misconfigurations***. PowerUp pulls together a range of legitimate PowerShell commands for service enumeration and outputs them as a single result – saving you from manually running each command.

**Import the module into PowerShell**
```powershell
Import-Module ./PowerUp.ps1
```

**Run the Tool**
```powershell
Invoke-AllChecks > output.txt
```

---
### Seatbelt
Seatbelt is another project that is intended to be used to identify possible misconfigurations on Windows, performing `safety checks` on a Windows target, including checks for possible privilege escalation vectors.

**Run the Tool to run all checks**
```command prompt
Seatbelt.exe -group=all > output.txt
```