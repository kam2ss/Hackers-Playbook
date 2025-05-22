Port forwarding allows ports on compromised private network hosts to be exposed as if they were local ports on your kali machine.

#### When Firewall is not Compromised
**Setup Port Forward using Meterpreter to expose firewall's web interface**
```
meterpreter > portfwd add -r FIREWALL_IP -p 80 -l 8080
```

**On Kali**
```
http://localhost:8080
```

#### When Firewall is Compromised (Meterpreter is on the Firewall)
**Setup Port Forward using Meterpreter to expose firewall's web interface**
```
meterpreter > portfwd add -r 127.0.0.1 -p 80 -l 8080
```