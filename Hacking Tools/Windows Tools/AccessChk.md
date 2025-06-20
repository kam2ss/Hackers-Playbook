**AccessChk** (part of the Windows Sysinternals suite) is a command-line tool that ***checks the permissions of Windows objects – including services***. 

**Display the Privilege Levels of all the Services for your current user**
```
accesschk.exe -uwcqv "USERNAME" *
```

**Check Service Registry Key Permissions**
```
accesschk.exe /accepteula "USERNAME" -kvuqsw HKLM\System\CurrentControlSet\Services
```