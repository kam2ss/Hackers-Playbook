### Account Permissions
Check the permissions of your current user account to see what you can do with the foothold account.
```
net localgroup administrators
```

### PowerShell history
PowerShell can keep a history of commands run during a session.
```
history
```

PowerShell also stores the command history of all sessions in the user's profile directory. To find this file:
```
Windows key + R
%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

or

notepad (Get-PSReadLineOption | Select-Object -ExpandProperty HistorySavePath)
```

### Folders
Checking certain folders on the target, for example, the user's `Recycle Bin` contains all files deleted by a user. Some deleted files may interest you if this has not been regularly emptied.

You should also see if you can access other user's home folders on the system. To view the permissions of a folder:
```
Right-click on the folder
Properties
Security
```

### Scheduled Tasks
Another place to look is the ***Scheduled Tasks*** folder. You can find this in:
```
C:\Windows\System32\Tasks
```

**System32**: Contains all the ***operating system files*** and ***critical software files*** - files contained in this folder are typically ***only modifiable by Administrator users***.

***Non-administrator*** users can the `Task Scheduler` app to view all programs scheduled to be launched.

### Startup Programs
Any executables added to the ***All-Users Startup folder*** `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup` will be launched whenever a user authenticates to the system. 

Any executables added to the ***individual user's Startup folder*** `C:\Users\[AccountUsername]\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup` will be launched whenever that specific user authenticates to the Windows system.

If either of these folders are misconfigured, for example, if you low-privileged user has access to `All-Users` Startup directory, you could add a malicious binary to this folder.

Startup programs can also be store in the `Windows Registry`. For example, the `Run` and `RunOnce` registry keys are used by the system to run programs once a user has logged in. The `Run` key makes the program run with each log in, whereas `RunOnce` makes the program run once before deleting the key. 
```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce
```

***Startup programs added through the `Registry` or the `Startup folder` are run under the privileges of the user account authenticating to the Windows system. `Scheduled tasks`, however, can be launched and run as a different user account (including the SYSTEM account.)***

### Software
Sometimes, high-privileged users may download vulnerable software to the system. To output a list of installed software to a text file:
```
C:/Users/restricted1%3E%20wmic%20/OUTPUT:my_software.txt%20product%20get%20name
```

### LOLBAS
`Living Off the Land` techniques seek to use the software unintentionally, often using legitimate tools in malicious ways.

