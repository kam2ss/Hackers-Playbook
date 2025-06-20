## Antivirus disk scanning
During security assessments, penetration testers often encounter antivirus solutions on the target servers or workstations. As most scripts used in security assessments are also used in malicious hacking activities, the solutions will interfere with the penetration tests.

![Mimikatz failing to run because the file contains a virus.](https://il-labforge-assets.origin.immersivelabs.team/uploads/LIVpjsPjH94tISn5G7rDAbuL6B81ionE0utPkgkD_5U.png)

As seen in the example above, the Invoke-Mimikatz.ps1 module (the PowerShell version of the well-known Mimikatz tool) was downloaded from GitHub and saved locally. However, when trying to source the module, Windows Defender caught it as a malicious file and blocked it. After catching it, the antivirus will generally immediately remove the file from the disk.

## DownloadString-InvokeExpression
In PowerShell, this is done by creating a new `Net.WebClient` object, calling its `DownloadString` function with the URL of the malicious file as an argument, and finally calling InvokeExpression  (`IEX`) on the resulting object returned by the `DownloadString` function.

![Mimikatz being launched successfully](https://il-labforge-assets.origin.immersivelabs.team/uploads/Q9XIlFMbZK6VoF6EUyBl07kI2-G7PLT63HjCpKRUnAM.png)

As seen above, when using this technique, the antivirus does not catch it. The Invoke-Mimikatz command is successfully executed and the password dumping initiates without any error.

### Note
This is a ***fileless malware*** technique where the file is not saved on the disk but rather in the memory of the computer, which can also helps to cover the tracks .