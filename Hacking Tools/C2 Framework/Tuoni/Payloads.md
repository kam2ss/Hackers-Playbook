The process of creating payloads has evolved from a simple GET request to a more structured, multi-step method to enhance control and customization.

- **Initial Payload Creation:** Send a POST request that includes: 
	- *payload template ID.*
	- *listener's ID that the payload will communicate with.*
	- *additional configuration settings.*
	
- **Retrieve Payload ID**
	- **Server Response:** After processing, the server returns a ***unique Payload ID***.
	- **Purpose:** This ID identifies and tracks the payload for future interactions.
	
- **Access the Generated Payload:** Use the ***payload ID*** to retrieve the generated payload via a ***GET request***.

#### Windows Payload Templates
Types of windows payloads:
- `executable:` A standard executable file (.exe) that ***runs without displaying any visible effects***.
- `dll:` A Dynamic Link Library file containing the payload, ***activated upon loading the DLL***.
- `service:` An executable designed to ***run as a background service process***.
- `shellcode:` A sequence of instructions used as a payload in the form of shellcode.
- `debug_executable:` A debug version of the executable that creates a console windows for output visibility.

#### Configuration Parameters
- **Type:** Selecting from the options mentioned above to suit different deployment and operational objectives.
- **PaddingSize:** Determines the ***number of bytes appended to the end of the payload*** to artificially inflate its size, this technique ***evades detection*** that rely on payload size.
- **InitialWait:** Sets how long, in seconds, ***the payload will delay before initiating its primary functionality***.

#### Plugin and Temple IDs
- **Plugin ID:** `shelldot.payload.default`
- **Template IDs:** 
	- `shelldot.payload.windows-x64`: For Windows 64-bit.
	- `shelldot.payload.windows-x86:` For Windows 32-bit systems.

#### Payload Download Format
```
hxxp{s}://{tuoni_host_ip}:{listener_port}/{stagedUri}?{stagedUriPayloadId}={payload_id}
```

Substituting the above values and using a `payload_id` of `5`.
```
hxxps://IP/fileshare?fileId=5
```