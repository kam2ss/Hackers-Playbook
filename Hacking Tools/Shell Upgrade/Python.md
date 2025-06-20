#### If Python is Installed
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

#### Extra commands to make the shell work better if there are some issues
**Background**
```bash
ctrl +z
```

**Fix keys like `ctrl+C`, `arrows keys`, `backspace keys`**
```
stty raw -echo
```

**Foreground**
```
fg
```

