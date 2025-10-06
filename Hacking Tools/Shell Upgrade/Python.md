#### If Python is Installed
**Check Python**
```
which python python2 python3
```
- Checking which version of python is installed in the remote system.

**Spawn a pseudo-terminal (PTY)**
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
- Makes the reverse shell behave like a normal Linux terminal.

**Background**
```
ctrl +z
```

**Fix keys like `ctrl+C`, `arrows keys`, `backspace keys`**
```
stty raw -echo; fg
```

**Back in the reverse shell**
```
export TERM=xterm
clear
```

