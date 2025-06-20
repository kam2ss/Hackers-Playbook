**Responder** is a network poisoning tool used to ***gain user hashes from Windows hosts on the local network***. `If an attacker is connected to the same subnet as the victims, they will be able to poison the local link layer connection (Layer 2)`. It does this by answering local name resolution broadcasts with fake responses, tricking victims into connecting to it via SMB. During the SMB handshake, the victim unknowingly sends their user hash to the attacker.

### Using Responder
```
sudo python Responder.py -h
```

If the Responder successfully poison a connection, it ***captures the user's Net-NTLMv2 hash via an SMB handshake***. This hash ***cannot be used for `pass-the-hash`***, the hash required for the pass-the-hash attacks is the similarly name NTLMv2 hash.

However, this hash can be used for:
- `Relay attack:` Relayed to another service 
- `Crack offline:` to reveal the user's password using tools like ***John the Ripper***.

### Defending against Responder
**Disable LLMNR**
	To disable LLMNR you can use the following DWORD registry value:
	
	**Path:** `HKLM/SYSTEM/CurrentControlSet/Services/Dnscache/Parameters/`  
	**Key:** `EnableMulticast`  
	**Value:** 0

**Disable NBT-NS**
	To disable NBT-NS you must set the following DWORD registry value:
	
	**Path:** `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NetBT\Parameter\`  
	**Key:** `NodeType`  
	**Value:** 2

**SMB Signing**
	SMB Signing is used to prove that a connection between client and server has not been tampered with, thus preventing machine-in-the-middle attacks. This feature is enforced on Windows Servers by default; however, it is not a default configuration on non-server Windows operating systems.
	
	To enforce SMB signing you will need to set the following two DWORD registry values:
	
	**Path:** `System\CurrentControlSet\Services\LanManServer\Parameter`  
	**Key:** `EnableSecuritySignature`  
	**Value:** 1
	
	**Key:** `RequireSecuritySignature`  
	**Value:** 1

**Secure WPAD via DNS records**
	If you provide DNS for all of your clients, you can create a WPAD domain add a record to the clients' host files.
	The record does not need to resolve; it just needs to resolve before the client queries the wider network.