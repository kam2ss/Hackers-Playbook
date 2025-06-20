#### What is PsExec?
It ***allows an administrator user to run commands or processes remotely on any PC where the attacker has access to***. PsExec is one of many Sysinternals tools and can be downloaded from [here](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec).

**Requirements**
- **Port**: 445/TCP (SMB)
- **Required Group Memberships**: Administrators

#### The way PsExec works is as follows:
- ***Connect to Admin$ share and upload a service binary***. PsExec uses `psexesvc.exe` as the name.
- ***Connect to the Service Control Manager*** to ***create and run a service named PSEXECSVC*** and ***associate the service binary with `C:\Windows\psexesvc.exe`***
- Create some named pipes to handle stdin/stdout/stderr.

#### To run PsExec, we need to supply the administrator credentials
```cmd
psexec64.exe \\MACHINE_IP -u Administrator -p Password123 -i cmd.exe
```

![[PsExec.png]]




