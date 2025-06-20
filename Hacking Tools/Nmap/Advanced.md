
### Advanced Port Scans
| Port Scan Type                 | Example Command                                       |
| ------------------------------ | ----------------------------------------------------- |
| TCP Null Scan                  | sudo nmap -sN 10.10.64.183                            |
| TCP FIN Scan                   | sudo nmap -sF 10.10.64.183                            |
| TCP Xmas Scan                  | sudo nmap -sX 10.10.64.183                            |
| TCP Maimon Scan                | sudo nmap -sM 10.10.64.183                            |
| TCP ACK Scan                   | sudo nmap -sA 10.10.64.183                            |
| TCP Window Scan                | sudo nmap -sW 10.10.64.183                            |
| Custom TCP Scan                | sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.10.64.183 |
| Spoofed Source IP              | sudo nmap -S SPOOFED_IP 10.10.64.183                  |
| Spoofed MAC Address            | --spoof-mac SPOOFED_MAC                               |
| Decoy Scan                     | nmap -D DECOY_IP,ME 10.10.64.183                      |
| Idle (Zombie) Scan             | sudo nmap -sI ZOMBIE_IP 10.10.64.183                  |
| Fragment IP data into 8 bytes  | -f                                                    |
| Fragment IP data into 16 bytes | -ff                                                   |
These scan types rely on setting TCP flags in unexpected ways to prompt ports for a reply. 
- ***`Null`, `FIN`, and `Xmas` scan provoke a response from closed ports*** (RST replies), 
- ***`ACK`, and `Window` map out firewall rulesets, stateful or not, indicating which ports are filtered or unfiltered***.
- ***`Maimon:` provoke a response from closed ports***

| Option                 | Purpose                                  |
| ---------------------- | ---------------------------------------- |
| --source-port PORT_NUM | specify source port number               |
| --data-length NUM      | append random data to reach given length |



| Option   | Purpose                               |
| -------- | ------------------------------------- |
| --reason | explains how Nmap made its conclusion |
| -v       | verbose                               |
| -vv      | very verbose                          |
| -d       | debugging                             |
| -dd      | more details for debugging            |

Port Scanning Techniques

| Option                                        | Description                                                                                                                                                                                                                                                                                                     |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -sS (TCP SYN scan)(half-open TCP scan)        | It can be performed quickly, scanning thousands of ports per second on a fast network not hampered by restrictive firewalls. It is also relatively unobtrusive and stealthy since it never completes TCP connections.                                                                                           |
| -sT (TCP connect scan)                        | TCP connect scan is the default TCP scan type when SYN scan is not an option. This is the case when a user does not have raw packet privileges.                                                                                                                                                                 |
| -sU (UDP scans)                               | ​While most popular services on the Internet run over the TCP protocol, UDP services are widely deployed. Because UDP scanning is generally slower and more difficult than TCP, some security auditors ignore these ports.                                                                                      |
| -sY (SCTP INIT scan)                          | SCTP is a relatively new alternative to the TCP and UDP protocols, combining most characteristics of TCP and UDP, and also adding new features like multi-homing and multi-streaming. It is mostly being used for SS7/SIGTRAN related services but has the potential to be used for other applications as well. |
| -sN, -sF, -sX (TCP NULL, FIN, and Xmas scans) | These three scan types exploit a subtle loophole in the TCP RFC to differentiate between open and closed ports.                                                                                                                                                                                                 |
| -sA (TCP ACK scan)                            | It is used to map out firewall rulesets, determining whether they are stateful or not and which ports are filtered.                                                                                                                                                                                             |
| -sW (TCP Window scan)                         | Window scan is exactly the same as ACK scan except that it exploits an implementation detail of certain systems to differentiate open ports from closed ones, rather than always printing unfiltered when a RST is returned.                                                                                    |
| -sM (TCP Maimon scan)                         | This technique is exactly the same as NULL, FIN, and Xmas scans, except that the probe is FIN/ACK.                                                                                                                                                                                                              |
| –scanflags (Custom TCP scan)                  | ​The –scanflags option allows you to design your own scan by specifying arbitrary TCP flags.                                                                                                                                                                                                                    |
| -sI <zombie host>[:<probeport>] (idle scan)   | This advanced scan method allows for a truly blind TCP port scan of the target (meaning no packets are sent to the target from your real IP address).                                                                                                                                                           |
| -sO (IP protocol scan)                        | ​IP protocol scan allows you to determine which IP protocols (TCP, ICMP, IGMP, etc.) are supported by target machines.                                                                                                                                                                                          |
| -sC (invokes default scripts)                 | The most basic way of running Nmap scripts is by using the -sC option, which invokes the default scripts. This is shown in the example below. Although the -sV option is not strictly required to run an Nmap script, most scripts perform better with this option enabled.                                     |

SSH

If port 22 is open, it is possible to use Nmap scripts to execute bash commands. The ssh-run NSE script will enable you to authenticate on SSH and remotely run commands on the target and return the commands output. To specify the username and password, you will need to use the --script-args option.

nmap [IP ADDRESS] --script ssh-run --script-args="ssh-run.cmd=[COMMAND],username=<USERNAME>, password=<PASSWORD>"

E.g.

nmap --script ssh-run 10.102.10.81 --script-args="ssh-run.cmd=cat password.txt, username=tommy, password=coachella"

Starting Nmap 7.92 ( [https://nmap.org](https://nmap.org) ) at 2022-09-20 19:23 UTC

NSE: [ssh-run] Authenticated

NSE: [ssh-run] Running command: cat password.txt

NSE: [ssh-run] Output of command: token: t0mmysSecuR3Pa44w0rd

Nmap scan report for ip-10-102-10-81.eu-west-1.compute.internal (10.102.10.81)

Host is up (0.00030s latency).

Not shown: 997 closed tcp ports (conn-refused)

PORT   STATE SERVICE--

21/tcp open  ftp

22/tcp open  ssh

| ssh-run:

|   output:

|_    token: t0mmysSecuR3Pa44w0rd

80/tcp open  http