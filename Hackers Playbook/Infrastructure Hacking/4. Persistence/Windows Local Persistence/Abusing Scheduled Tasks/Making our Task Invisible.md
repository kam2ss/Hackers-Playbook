#### How to hide a Scheduled Task
To ***hide a scheduled backdoor task*** from users (including Administrators), ***delete its Security Descriptor (SD)***, which defines who can access the task. 
- Without the SD, no user can see or manage the task.

#### SD values are located in the Windows Registry
`
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree\`

***To delete the SD for a task, you need SYSTEM privileges***. You can achieve this using tools like `psexec` to launch `Regedit` as SYSTEM and manually remove the SD value.

#### PsExec to delete the SD (Security Descriptor)
```cmd
c:\tools\pstools\PsExec64.exe -s -i regedit
```
![[Delete SD.png]]

#### Query the Task again
```cmd
schtasks /query /tn thm-taskbackdoor
```