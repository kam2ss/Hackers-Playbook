### User and Group Enumeration
```bash
id
```

**To inspect which groups your user is a part of**
```bash
groups
```

#### Operating System and Software
**Distribution and Version**
```bash
cat /etc/issue
cat /etc/*-release
```

**Kernel Version**
```bash
cat /proc/version
uname -a
```

**Applications Installed**
```bash
ls -alh /usr/bin/
ls -alh /sbin/
dpkg -l
```

#### Cronjobs
```bash
`crontab -l`  
`ls -alh /var/spool/cron`  
`ls -al /etc/ | grep cron`  
`ls -al /etc/cron*`  
`cat /etc/cron*`  
`cat /etc/at.allow`  
`cat /etc/at.deny`  
`cat /etc/cron.allow`  
`cat /etc/cron.deny`  
`cat /etc/crontab`  
`cat /etc/anacrontab`  
`cat /var/spool/cron/crontabs/root`
```

#### SUID Binaries
```
find / -perm -g=s -o -perm -4000 ! -type l -maxdepth 3 -exec ls -ld {} \; 2>/dev/null
```

#### Users
```
sudo -l
```

#### Check for `.ssh` folder for Private keys
**If there are multiple users**
```bash
ls -al user1 user2 user3
```

**If single user**
```bash
ls -al user
```

#### Check the PAM Configuration File
```bash
cat /etc/pam.d/su
```

### Environment Variables
A shell's behaviour is stored in an environment. Environment variables are the variables that define the current shell session environment. They affect various aspects of a system, including processes and programs. 

**Check the current environment variables**
```bash
env || set
```

### Root Permissions
**List all the commands that the current user has access to perform as root**
```bash
sudo -l
```

### Abusing Sudo Permissions
If you find a binary `e.g., (gdb)` that you can run as root, there's a chance that you can abuse this privilege. 
```bash
sudo gdb -nx -ex '!sh' -ex quit
```

### Vulnerable software
**Check installed software**
```bash
dpkg -l
```

### Passwords
```
grep -ri 'password' /home/user
```

### Weak File Permissions
```bash
ls -al /etc/passwd
```

**Find a binary that is `Writable` and `Executable` by the current user**
```
find / -type f -writable -executable 2>/dev/null
```



