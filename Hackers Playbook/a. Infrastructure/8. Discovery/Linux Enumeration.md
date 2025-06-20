### System
**Information on the Linux distribution and release version**
```
ls /etc/*-release

# or 

ls /etc/os-release
```

**Find the System's name**
```
hostname
```

**To find the installed applications you may consider listing files in `/usr/bin/` and `/sbin/`**
```
ls -lh /usr/bin
```

**List installed packages**
```
dpkg -l
```

---
### Users
**Show who is logged in**
```
who
```

**Show who is logged in and what they are doing**
```
w
```

**Display the last logged-in users**
```
last
```

---
### Networking
**Show IP addresses**
```
ip a s
```

**Find DNS servers**
```
cat /etc/resolv.conf
```

**Show network connections, routing tables, and interface statistics**
```
netstat
```
Note: `To get all PID (Process ID) and program names, use **sudo**`

**`lsof` (List Open Files) to only display Internet and Network connections**
```
sudo lsof -i 
```

**Limit the output to port `25`**
```
sudo lsof -i :25
```

---
### Running Services
**List every process on the system**
```
ps -e
```

**To list a process tree**
```
ps axf
```