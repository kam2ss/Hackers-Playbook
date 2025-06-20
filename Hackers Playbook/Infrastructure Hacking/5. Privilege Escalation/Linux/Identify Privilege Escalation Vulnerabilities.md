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

### Groups
**To inspect which groups your user is a part of**
```bash
groups
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