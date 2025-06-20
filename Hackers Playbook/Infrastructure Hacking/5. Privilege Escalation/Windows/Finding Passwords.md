(Un)fortunately, credentials are frequently stored in locations that are easily accessible.

### Searching for Keywords
#### File Content
```powershell
findstr /m /s /i secret *.txt
```
- `/m:` output is extensive and this parameter is used to print the file name of the results.
- `/s:` specifies that subdirectories should be included
- `/i:` ignore the case of the string

**Find files for a User**
```powershell
findstr /m /si USERNAME *.txt
```

To search for multiple keywords, you can create a wordlist (e.g., `wordlist.txt`). The following command loops through each keyword in a file:
```powershell
for /F %i in (wordlist.txt) do ( findstr /M /C:%i /S *.txt )
```

---
#### Filenames
some files may also be named specifically for their passwords or credentials.
```cmd
dir /s *credentials*
```

You can search for multiple keywords at once by separating each keyword with `==`.
```
dir /s *pass* == *cred* == *secret*
```

---
### Files of Interest
#### Configuration Files
Configuration files are used to ***configure the parameters and settings*** for the Windows operating system and applications.
```
dir /s *config* == *configuration* == *conf*
```

configuration files can often be found in the format **.xml**, or **`.ini`**. An `.ini` file is a configuration file used by Windows to initialize program settings, whereas an Extensible Markup Language (.xml) is a markup language (like HTML) used to store and transport data.
```
findstr /m /si password *.xml *.ini
```

#### PowerShell Scripts
The `findstr` command can be used once again to find keywords in any PowerShell scripts (.ps1):
```
findstr /m /si password *.ps1
```

Even if a PowerShell script doesn't contain credentials, it may include references to other files that do contain credentials.
```
$username = "restricted"
$password = Get-Content -Path C:\Users\restricted1\password.txt
```

---
### Windows Registry
#### Searching for Keywords
In some situations, the ***registry can contain sensitive data*** such as passwords, application configuration options, and system decryption keys and sometimes this data is stored insecurely.

**Query for any Registry keys that contains the string Password**
```
reg query HKCU /f password /t REG_SZ /s
reg query HKLM /f password /t REG_SZ /s
```
This will search the entirety of the **HKEY_CURRENT_USER (HKCU)** registry hive, followed by the **HKEY_LOCAL_MACHINE (HKLM)** hive, for any keys that contain the word "password".

#### Saved RDP Connections
When administrators repeatedly authenticate to a system via RDP, they may save their connection details for quicker access. These credentials can be ***stored in the registry***. However, if **Credential Guard** is enabled, it ***prevents RDP credentials from being stored*** in the registry. In this case, administrators might ***save the credentials in files instead***.

**Display a list of RDP:**
```
reg query "HKCU\Software\Microsoft\Terminal Server Client\Servers"

# Below are the outputs
HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client\Servers\192.168.1.1
HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client\Servers\3.208.88.91
HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client\Servers\3.249.70.23
```

**Then query any of these registry keys:**
```
reg query "HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client\Servers\3.208.88.91"

# Output
HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client\Servers\3.208.88.91
UsernameHint REG_SZ a.user@windows.domain
```

You can also check to see if you have permissions to view other user's saved RDP connections.
**Query the HKEY_USERS Hive to get a list of user SIDs**
```
reg query "HKEY_USERS"

# Output
HKEY_USERS\.DEFAULT
HKEY_USERS\S-1-5-19
HKEY_USERS\S-1-5-20
HKEY_USERS\S-1-5-21-2017622231-2370434281-3899583407-1001
HKEY_USERS\S-1-5-21-2017622231-2370035192-3899583507-1002
HKEY_USERS\S-1-5-21-2017622231-2370434281-3899583407-1001_Classes
HKEY_USERS\S-1-5-21-2017622231-2370035192-3899583507-1002_Classes
HKEY_USERS\S-1-5-18
```

**Query the relevant registry key for each User**
```
reg query "HKEY_USERS\S-1-5-21-2017622231-2370035192-3899583507-1002\Software\Microsoft\Terminal Server Client\Servers"

HKEY_USERS\S-1-5-21-2017622231-2370035192-3899583507-1002\Software\Microsoft\Terminal Server Client\Servers\192.168.3.5
```