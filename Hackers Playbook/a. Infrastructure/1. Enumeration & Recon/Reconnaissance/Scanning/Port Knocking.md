It is a method of externally opening ports by sending a sequence of connection attempts to pre-specified ports. It is used to prevent an attacker from identifying which ports and services are potentially vulnerable by port scanning the system. Unless the correct knock sequence is sent to the port, it will appear closed.

The Linux 'Knockd Daemon' allows system owners to run commands when a certain sequence of TCP ports are 'knocked'. The knocking can be done manually using Netcat connections or using the Knock client (part of the Knockd package).

One disadvantage of port knocking, however, is that it's dependent on the robustness of the port-knocking daemon. If the daemon fails, all users will be denied port access, which would be an undesirable single point of failure. However, this can be mitigated by providing a process-monitoring daemon, which can restart a failed or stalled port-knocking daemon process. Port knocking can be problematic on networks exhibiting high latency and should not be used as the primary authentication mechanism for a server.

![](https://il-labforge-assets.origin.immersivelabs.team/uploads/mWcMf_wvVMu7nDuWIUXc2btej0cp8uWTbGHX8i7adlw.png)

`knock [target IP address] [first port] [second port] [third port]`

