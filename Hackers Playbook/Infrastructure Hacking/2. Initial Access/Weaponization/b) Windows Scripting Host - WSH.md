WSH is a built-in Windows administration tool that ***runs batch files to automate and manage tasks*** within the operating system. 

It is a Windows native engine, `cscript.exe` (for command-line scripts) and `wscript.exe` (for UI scripts), which are responsible for executing various Microsoft Visual Basic Scripts (VBScript), including `vbs` and `vbe`. It is important to note that the VBScript engine on a Windows operating system runs and executes applications with the ***same level of access and permission as a regular user***.

```
# Welcome Message

Dim message 
message = "Welcome to THM"
MsgBox message
```

##### Using VBScript to run executable files.
To run the Windows calculator `calc.exe`
```
Set shell = WScript.CreateObject("Wscript.Shell")
shell.Run("C:\Windows\System32\calc.exe " & WScript.ScriptFullName),0,True
```

Run using the `wscript`
```
wscript payload.vbs
```

Run using the `cscript`
```
cscript payload.vbs
```

##### Another trick, if the VBS files are blacklisted, then we can rename the file to `.txt` file and run it using `wscript`.
```
wscript /e:VBScript payload.txt
```
- `/e:VBScript:` specifies that the script is written in `VBSscript`.
