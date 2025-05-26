### What is Pivoting?
Gaining ***access to another internal network (unreachable networks)*** or subnet by routing traffic through a compromised host. It can be ***used in internal networks when firewall rules restrict direct connections to certain machines***.

### SSH Tunnelling
#### Local Port Forwarding
Local port forwarding ***lets an SSH client forward traffic from a local port to a specified destination via the SSH server (creates an SSH Tunnel from your machine to intermediary server)***, even if the destination isn't directly accessible.

Useful to either ***remotely access local ports on the SSH server or ports on hosts to which there is no direct connection from the SSH client***.

```bash
# ssh -L [LOCAL_IP:]LOCAL_PORT:DESTINATION:DESTINATION_PORT [USER@]SSH_SERVER
ssh -L 4444:<end_server>:443 root@<middle_server>
```
- `[LOCAL_IP]:` is ***optional*** syntax. If you don't specify it, the ***SSH client will listen on all available local IP addresses***.
	- `loopback (127.0.0.1), local network ip addresses, public ip address`

	**Visual representation of SSH Tunnelling**
	`Local Client (127.0.0.1:4444)  -->  SSH Tunnel (via SSH/Middle server)  -->  Remote Server (10.0.0.5:443)`

![[Local Port Forwarding.png]]

#### Remote Port Forwarding
Remote port forwarding ***allows the SSH server to listen on a port and forward any traffic received on that port to a port on the SSH client, which then forwards it to the destination host***. 

This method is useful when there's ***no direct connection from a compromised host back to the attacking host***.

Machines involved:
- Kali (internal/private network) - SSH client (`localhost:443)
- Remote Server (public IP) - SSH server (remote-server:4444)
- External User (anywhere) - wants access to internal service behind kali

```bash
# ssh -R [REMOTE_IP:]REMOTE_PORT:DESTINATION:DESTINATION_PORT [USER@]SSH_SERVER
ssh -R <4444>:<localhost>:443 root@<remote_server>
```
- `Secure way:` Don't mention your ip, instead use `localhost`.
![[Remote Port Forwarding.png]]

#### Dynamic Port Forwarding
Dynamic port forwarding sets up a SOCKS proxy on the local (SSH client) machine, which forwards traffic through an SSH server to the actual destination.

1. **Create a SOCKS proxy**
```
ssh -D [LOCAL_IP:]LOCAL_PORT -N -f [USER@]SSH_SERVER
```
- `-D:` specifies the SOCKS proxy
- `-N:` No command to execute, just set up the tunnel
- `-f:` Runs the SSH connection in the background

2. To use this proxy, **modify `/etc/proxychains.conf`** to use the specified port (default is 9050).
3. Route the traffic through the SOCKS proxy using `proxychains:`
	- **SSH Traffic:** ```proxychains ssh <user>@<IP behind the pivot host>``` 
	- **HTTP Traffic:** ```proxychains wget <IP behind the pivot host>```
![[Dynamic Port Forwarding.png]]
### Meterpreter PORTFWD
**Meterpreter** allows **local port forwarding** when SSH access isn't available during penetration tests.

**Local Port Forwarding**
```
portfwd add -l <PORT_ON_LOCAL_MACHINE> -p <REMOTE_PORT> -r <DESTINATION_HOST>
```

### Summary
- `Pivoting:` **new network**
- `Lateral movement:` **same network**