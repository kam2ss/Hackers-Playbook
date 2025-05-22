PATH in Linux is an environmental variable that tells the operating system where to search for executables. For any command that is not built into the shell or that is not defined with an absolute path, Linux will start searching in folders defined under PATH.
![[Pasted image 20230702234232.png]]

For demo purposes, we will use the script below:
![[Pasted image 20230702234447.png]]
This script tries to launch a system binary called "thm", can be any binary

We compile this into an executable and set the SUID bit:
![[Pasted image 20230702234606.png]]

Once executed "path" will look for an executable named "thm" inside folder listed under PATH.
If any writable folder is listed under PATH we could create a binary named thm under that directory and have our "path" script run it.
	$ find / -writable 2>/dev/null | cut -d "/" -f 2 | sort -u	![[Pasted image 20230702235410.png]]
	We see a number of folders under /usr
	$ find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u (alternate cmd)
	
The folder that will be easier to write to is probably /tmp. At this point because /tmp is not present in PATH we will need to add it.
	$ export PATH=/tmp:$PATH
	![[Pasted image 20230702235840.png]]
	
	![[Pasted image 20230703000020.png]]
	
  
