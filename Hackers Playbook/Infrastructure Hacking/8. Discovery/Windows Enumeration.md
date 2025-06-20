### System
**Show info about the System, such as its Build number and Installed Patches**
```
systeminfo
```

**Check Installed Updates**
```
wmic qfe get Caption,Description
```

**Check the installed and started Services**
```
net start
```

**Check Installed Apps**
```
wmic product get name,version,vendor
```

---
### Users
**Check who you are**
```
whoami
```

**Check Privileges**
```
whoami /priv
```

**Check Group**
```
whoami /groups
```

**Check Users**
```
net user
```

**Check Available Group**
```
net localgroup
```

**Users belong to the Local Administrator's Group**
```
net localgroup administrators
```

**Check Local Settings**
```
net accounts
```

**Check Domain Settings**
```
net accounts /domain
```

---
### Networking
**Check Network Configuration**
```
ipconfig
```

**Check Network Information such as Ports, Connections, etc**
```
netstat -anbo
```

**Discover Other Systems on the same LAN**
```
arp -a
```

