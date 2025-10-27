The Tuoni command and control (C2) framework employs various listeners to manage communication between compromised devices, known as agents, and the C2 server.

#### Listeners Types
Listeners are categorized into three primary types:
- **Listeners operating in C2 for built agents:** Manages agents that establish persistent communication back to the C2 server.
- **Relay listeners for lateral movement:** Facilitate movement within a network by creating pathways through intermediary systems.
- **Listeners in C2 for simple shell connections:** Handle basic command execution without needing full agent capabilities.

#### Listeners Operating in C2 for Built Agents
**Reverse HTTP Listener**
- Uses URIs for ***GET and POST requests to retrieve commands*** and submit execution results.
- Ability to **rotate between different hosts and modify HTTP host headers** based on predefined rules helps in obfuscation and domain fronting, enhancing operational security. The rotation can be configured using rules like `FAILOVER`, `ROTATE`, and `RANDOM`.

**Reverse TCP Listener**
- Agents connect over a direct TCP socket for quick and uninterrupted agent-server communication.
- Lacks obfuscation, making it easier to detect.

**Relay Listeners for Lateral Movement**
- ***`Relay listeners (all three below listeners) utilizes existing agent to listen and connect.`***
- Facilitate internal network communication by routing through intermediary systems.
- Types:
	- **Bind TCP Listener:** This listener ***operates without creating a listening socket on C2 server***. Instead, it configures an existing agent to listen for TCP connections on a specified port, adding ***obfuscation***. ***For e.g. `connect-tcp:` command is used to connect the listener.***
	- **Bind SMB Listener:** Uses ***SMB named pipes for stealthy, Obfuscation, and firewall-evasive communication***. For e.g. `connect-smb:` to connect. 
	- **Reverse TCP Listener:** Existing agent listens for new connections, routing through the selected agent for stealth.

**Listeners in C2 for Simple Shell Connections**
These listeners are used for simpler payloads that don't require full capabilities.
- **Reverse Shell Generator:** Basic TCP connection for ***redirecting terminal I/O (stdin/stdout) through a socket***, ideal for straightforward tasks with tools like Netcat.

**Special Listeners**
- **External Listener:** Not meant for direct agent connections. Primarily, it integrates Tuoni with with external listeners and C2 framework via WebSocket for streamlined, cross-platform agent management.

#### Managing Listeners
Listeners are created and managed using the Tuoni client. Open the listener by browsing to `hxxps://local-c2:12702/listeners`.
